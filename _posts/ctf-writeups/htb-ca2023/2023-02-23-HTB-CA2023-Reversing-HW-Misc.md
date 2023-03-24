---
layout: post
title: "HackTheBox Cyber Apocalypse 2023 - Reversing/Hardware/Misc"
category: CTF Writeups
tags: HackTheBox HTB-Cyber-Apocalypse-2023 CTF writeup
date: "2023-03-23 21:00:00 -0400"
---

I decided to throw all of these together since they were fairly short writeups.

# Reversing

![](/assets/img/ctf-writeups/ca2023/reversing/scoreboard.png)

## Shattered Tablet

Difficulty: Very Easy - 300 points

> Deep in an ancient tomb, you've discovered a stone tablet with secret information on the locations of other relics. However, while dodging a poison dart, it slipped from your hands and shattered into hundreds of pieces. Can you reassemble it and read the clues?

In this challenge, we are provided with a binary that asks for what the tablet says.

![](/assets/img/ctf-writeups/ca2023/reversing/shattered_tablet/binary.png)

Throwing it in IDA, the main function shows a bunch of comparisons taking place.

![](/assets/img/ctf-writeups/ca2023/reversing/shattered_tablet/decompiled.png)

Getting the char numbers and manually putting them in order, I then used a Python script to output the flag.

```python
chars = [72,84,66,123,98,114,48,107,51,110,95,52,112,52,114,116,44,110,51,118,101,114,95,116,48,95,98,51,95,114,51,112,52,49,114,51,100,125]

for i in chars:
    print(chr(i),end="")
```

Flag: `HTB{br0k3n_4p4rt,n3ver_t0_b3_r3p41r3d}`

## Needle in a Haystack

Difficulty: Very Easy - 300 points

> You've obtained an ancient alien Datasphere, containing categorized and sorted recordings of every word in the forgotten intergalactic common language. Hidden within it is the password to a tomb, but the sphere has been worn with age and the search function no longer works, only playing random recordings. You don't have time to search through every recording - can you crack it open and extract the answer?

This one was too easy. All I had to do was run strings against the binary to find the flag.

![](/assets/img/ctf-writeups/ca2023/reversing/haystack/flag.png)

Flag: `HTB{d1v1ng_1nt0_th3_d4tab4nk5}`

## Hunting License

Difficulty: Easy - 300 points

> STOP! Adventurer, have you got an up to date relic hunting license? If you don't, you'll need to take the exam again before you'll be allowed passage into the spacelanes!

In this challenge, we have to provide three correct passwords to pass a Hunting License exam.

![](/assets/img/ctf-writeups/ca2023/reversing/hunting_license/binary.png)

Throwing the binary in IDA, we can see the main function calls a function called exam.

![](/assets/img/ctf-writeups/ca2023/reversing/hunting_license/main.png)

In this exam function, we see the first password, and find out that the second password will be reversed, and the third will be XORed against 0x13 (19 decimal).

![](/assets/img/ctf-writeups/ca2023/reversing/hunting_license/exam.png)

Navigating to the variable t, I navigate to it's memory address, where I find the string that is reversed for the second password and the bytes that are XORed for the third.

![](/assets/img/ctf-writeups/ca2023/reversing/hunting_license/passwords.png)

The second password is `P4ssw0rdTw0`, with the third being `ThirdAndfinal!!!`.

![](/assets/img/ctf-writeups/ca2023/reversing/hunting_license/passthree.png)

Spinning up the Docker instance, I answer the necessary questions and obtain the flag.

![](/assets/img/ctf-writeups/ca2023/reversing/hunting_license/flag.png)

Flag: `HTB{l1c3ns3_4cquir3d-hunt1ng_t1m3!}`

# Hardware

![](/assets/img/ctf-writeups/ca2023/hardware/scoreboard.png)

## Timed Transmission

Difficulty: Very Easy - 300 points

> As part of your initialization sequence, your team loaded various tools into your system, but you still need to learn how to use them effectively. They have tasked you with the challenge of finding the appropriate tool to open a file containing strange serial signals. Can you rise to the challenge and find the right tool?

This challenge provides us with a .sal file that can be opened in Saleae's Logic 2.

![](/assets/img/ctf-writeups/ca2023/hardware/timed_transmission/file.png)

Opening the file, I see some pulses that kind of look like they say something.

![](/assets/img/ctf-writeups/ca2023/hardware/timed_transmission/open.png)

Playing with the zoom a bit, I see that they *do* say something, the flag.

![](/assets/img/ctf-writeups/ca2023/hardware/timed_transmission/zoom.png)


Flag: `HTB{b391N_tH3_HArdWAr3_QU3St}`

## Debug

Difficulty: Easy - 300 points

> Your team has recovered a satellite dish that was used for transmitting the location of the relic, but it seems to be malfunctioning. There seems to be some interference affecting its connection to the satellite system, but there are no indications of what it could be. Perhaps the debugging interface could provide some insight, but they are unable to decode the serial signal captured during the device's booting sequence. Can you help to decode the signal and find the source of the interference?

This challenge has another .sal file. Opening it in Logic, I see some signals but nothing easily identifiable.

![](/assets/img/ctf-writeups/ca2023/hardware/debug/open.png)

Based on the title and description, I determined that running the Async Serial analyzer against this data would be a good idea since this is a debug interface. I researched common baud rates for debug interfaces and tried the most common, 9600 baud, which returned garbled data and framing errors.

![](/assets/img/ctf-writeups/ca2023/hardware/debug/9600.png)

The next common baud rate was 115200 baud, so I tried that next, and got readable data in return.

![](/assets/img/ctf-writeups/ca2023/hardware/debug/115200.png)

Switching to the terminal view, I could read the text easier, and found the flag across three lines.

![](/assets/img/ctf-writeups/ca2023/hardware/debug/flag.png)

Flag: `HTB{547311173_n37w02k_c0mp20m153d}`


# Miscellaneous

![](/assets/img/ctf-writeups/ca2023/misc/scoreboard.png)

## Persistence

Difficulty: Very Easy - 300 points

> Thousands of years ago, sending a GET request to **/flag** would grant immense power and wisdom. Now it's broken and usually returns random data, but keep trying, and you might get lucky... Legends say it works once every 1000 tries.

This one was pretty easy, I just wrote a simple Python script to send a GET request to  `/flag`  repeatedly and break if it got the flag.

```python
import requests
url = 'http://139.59.174.119:31938/flag'

for i in range (0,1000):
    r = requests.get(url)
    
    if "HTB" in r.text:
        print(r.text)
        break 
```

![](/assets/img/ctf-writeups/ca2023/misc/persistence/flag.png)

Flag: `HTB{y0u_h4v3_p0w3rfuL_sCr1pt1ng_ab1lit13S!}`

## Hijack

Difficulty: Easy - 300 points

> The security of the alien spacecrafts did not prove very robust, and you have gained access to an interface allowing you to upload a new configuration to their ship's Thermal Control System. Can you take advantage of the situation without raising any suspicion?

Connecting to the Docker environment, I am greeted by a prompt to create or load a config. I created a config and it returned a Base64 encoded serialized string. 

![](/assets/img/ctf-writeups/ca2023/misc/hijack/config.png)

Plugging this into CyberChef, I find that this is using Python pickle for serialization.

![](/assets/img/ctf-writeups/ca2023/misc/hijack/pickle.png)

With this information, I obtain a template and use it to serialize my own payload.

![](/assets/img/ctf-writeups/ca2023/misc/hijack/ls.png)

I entered this Base64 string into the prompt and got the file listing.

![](/assets/img/ctf-writeups/ca2023/misc/hijack/files.png)

I changed the command to output the flag, and got the flag.

![](/assets/img/ctf-writeups/ca2023/misc/hijack/catflag.png)

![](/assets/img/ctf-writeups/ca2023/misc/hijack/flag.png)

Flag: `HTB{1s_1t_ju5t_m3_0r_iS_1t_g3tTing_h0t_1n_h3r3?}`