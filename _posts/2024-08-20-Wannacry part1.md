---
layout: post
title: Wannacry Ransomware Analysis - Part 1
date: 2024-08-20
description: In depth analysis of the Wannacry Ransomware - part 1
tags: wannacry
categories: malware-analysis
---
# **Table of Contents**
<!-- TOC -->

1. [Table of Contents](#table-of-contents)
2. [What is the Wannacry Ransomware?](#what-is-the-wannacry-ransomware)
3. [Static Analysis](#static-analysis)
    1. [Hash Values and VirusTotal](#hash-values-and-virustotal)
    2. [Floss](#floss)
    3. [PEStudio](#pestudio)
2. [Dynamic Analysis](#dynamic-analysis)
    1. [Regshot](#regshot)
    2. [Procmon](#procmon)
2. [Conclusion](#conclusion)

<!-- /TOC -->
Hey there! Welcome to my first ever attempt at analysing a piece of malware. What's a better place to start than the infamous Wannacry Ransomware. The malware analysis will be broken into 2 parts. This first part will consist of what I call the "basic" analysis where we look at the file properties and signatures. We will also be  detonating the malware and look what it does through tools like procmon and TCPview. In the second part, we look deeper into the code of the malware by opening it in some static and dynamic reverse engineering tools such as Ghidra and x64dbg. I would like to mention that this analysis was done completely blind, I have not looked at any other writeups online and the information presented in this blog post was found and presented solely by me. Now let's get right to it.

# **What is the Wannacry Ransomware?**
Before we start looking into what Wannacry does, let's talk about what Wannacry is.
Wannacry is the name of a crypto ransomware that spread everywhere in May 2017. A ransomware is a type of malicious software (malware) which holds your files and data hostage for a ransom. If you don't pay the ransom, you will not get access back to your files/data. A crypto ransomware is a ransomware that encrypts your data and only decrypts it when the ransom is paid.

Wannacry takes advantage of <a href="https://www.sentinelone.com/blog/eternalblue-nsa-developed-exploit-just-wont-die/">EternalBlue</a> to propagate to other potential victims. EternalBlue was an exploit of the Server Message Block (SMB) protocol on Microsoft Windows developed by the National Security Agency (NSA) and leaked by a group called <i>The Shadow Brokers (TSB)</i>. The USA and UK officially blame North Korea for the development of Wannacry but there are conflicting reports regarding it.

Wannacry affected approximately <a href="https://usa.kaspersky.com/resource-center/threats/ransomware-wannacry">230,000 computers</a> including major industries such as logistics, advertising, automotive etc. The EternalBlue vulnerability has since been fixed and Wannacry has also been neutralised by other means which we shall get into later.

The malware sample has been obtained from <a href="https://github.com/ytisf/theZoo">theZoo</a>, which a github repo of live malware samples FOR EDUCATIONAL PURPOSES ONLY.
# **Static Analysis**

In this section, we shall perform some basic static analysis on the executable file. We shall look at some properties and some other characteristics. For the sake of completion, we will be treating the sample as an unknown piece of malware and perform the full suite of analysis on it without any assumptions.

<a id="markdown-hash-values-and-virustotal" name="hash-values-and-virustotal"></a>

## Hash Values and VirusTotal

First thing we do is to check the hash values of the file and search for it on an online malware database such as <a href="virustotal.com">VirusTotal</a>.

We get the SHA256 and MD5 hashes of the sample using CertUtil.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/hashes.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

There are multiple samples of the Wannacry that are functionally similar. These are the hashes of the sample we will be working with:<br>
MD5: `84c82835a5d21bbcf75a61706d8ab549`<br>
SHA256: `ed01ebfbc9eb5bbea545af4d01bf5f1071661840480439c6e5babe8e080e41aa`



Next step is checking these hash values on VirusTotal and see if we get any matches in the database.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/virustotal.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption col-sm mt-3 mt-md-0 col-lg-12">
    VirusTotal search result for the SHA256 hash.
</div>

VirusTotal shows that 71 out of 75 security vendors have marked this file as malicious and most of them have identified it as Wannacry or some variation of it.
<a href="https://www.virustotal.com/gui/file/ed01ebfbc9eb5bbea545af4d01bf5f1071661840480439c6e5babe8e080e41aa">VirusTotal Search Results</a>.

<a id="markdown-floss" name="floss"></a>

## Floss

Next step, we pass the sample through <a href="https://github.com/mandiant/flare-floss">floss</a>, a commandline utility that extracts and deobfuscastes strings present in binaries.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-6">
        {% include figure.html path="assets/img/wcry/floss4.png" class="img-fluid rounded z-depth-1" style="height: 300px; width: auto;"%}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/wcry/floss2.png" class="img-fluid rounded z-depth-1" style="height: 300px; width: auto;"%}
    </div>
</div>
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/wcry/floss3.png" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/wcry/floss1.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Snippets of Floss output
</div>


There are hundreds of identified strings. Here are the points to note from the floss output:
1. The string `cmd.exe /c "%s"`, this allows the executable to run aribitrary cmd commands.
2. There is also a string `icacls . /grant Everyone:F /T /C /Q` present. We will see what this does in later.
3. There are strings for different languages, maybe to generate messages in different languages. 
4. Strings like `%s\Intel` and `%s\ProgramData` can provide direct access to critical folders if used correctly.
5. We see a number of system functions that are used in the executable regarding directories and files.

<a id="markdown-pestudio" name="pestudio"></a>

## PEStudio

Now, lets look at the malware sample in <a href="https://www.geeksforgeeks.org/what-is-pestudio/">PEStudio</a>. PEStudio is an all-in-one (mostly) tool that allows us to statically look at a ton of informtaion about PE (Portable Executable) files. 

Under the the general properties of the file, we see some basic information of the file. 
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/pestudio1.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
Some relevant ones are:<br>
`size`: 3514368 bytes<br>
`description`: DiskPart<br>
`Compile Stamp`: Sat Nov 20 09:05:05 2010 | UTC<br>

The description shows up as `DiskPart` which is one of the aliases that VirusTotal showed for the file.

Then we look at the raw and virtual size of the file.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/pestudio2.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

We see that the raw file size (3510272 bytes) and the virtual size (3506712 bytes ) of the file is approximately the same. This would indicate that the file is not packed and would not need to be unpacked to analyse further. 

We can also look at the Windows libraries (.dll files) that are imported by the malware.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/pestudio3.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

While these imports are not inherently malicous and are used by benign programs, but these do allow for some pretty serious control over the operating system.

Now we can look at the functions from the library that are imported by the executable. PEStudio is pretty smart and flags functions that are potentially malicious.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/pestudio4.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Out of the 114 functions imported by the program, PEStudio flags 15 of them as malicious. These include functions to set current directory (SetCurrentDirectoryW/SetCurrentDirectoryA), random functions (srand/rand), create and set registry key values (RegCeateKeyW/RegSetValueExA) among others. Seeing these functions in context of what Wannacry does, it makes sense to flag these as malicious functions.

The resources section gives us some useful information. 
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/pestudio5.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

We see a resource called `XIA` which is a `.zip` file. We extract that file and find 2 executables and some files with a `.wnry` extension
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/pestudio6.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Unfortunately, these files are password protected and we do not know the password at this time. We can try looking at these again if we find a password.

That concludes our inital static analysis of the malware. 

# **Dynamic Analysis**

What's a better way to understand what a piece of malware does if not by running it. In dynamic analysis, we run the malware and observe its behabviour. We use certain tools to see what the malware executes on the computer and can trace the sequence of events. It goes without saying that this has to be done under very controlled conditions. Ideally, on a virutal machine in an isolated network. We do not know if/ how the malware propogates and not isolating it on the network could get the host machine infected.

Our lab test environment consists of a FlareVM machine running the malware and a REMnux box running inetsim and wireshark to act as a fake internet connection. These 2 machines are running on Oracle VirtualBox in an isolated network so they cannot reach the host machine or the actual internet.

<a id="markdown-regshot" name="regshot"></a>

## Regshot

The first dynamic analysis tool we use is <a href="https://github.com/Seabreg/Regshot">Regshot</a>. Regshot is an open-source registry compare utility that allows you to quickly take a snapshot of your registry and then compare it with a second one. This way we can compare the what the malware does to our registry. We take a shot before and one after the malware detonation.
Snippets of the comparison results are included.
Comparing the output, we see the whole `HKLM\DRIVERS` key has been deleted. This could be done to prevent the victim machine from performing properly and could potentially prevent any recovery actions.
```
----------------------------------
Keys deleted: 20287
----------------------------------
HKLM\DRIVERS
HKLM\DRIVERS\DriverDatabase
HKLM\DRIVERS\DriverDatabase\DeviceIds
HKLM\DRIVERS\DriverDatabase\DeviceIds\*AEI0276
HKLM\DRIVERS\DriverDatabase\DeviceIds\*AEI9240
HKLM\DRIVERS\DriverDatabase\DeviceIds\*AIW1038
HKLM\DRIVERS\DriverDatabase\DeviceIds\*AKY00A1
HKLM\DRIVERS\DriverDatabase\DeviceIds\*AKY1001
HKLM\DRIVERS\DriverDatabase\DeviceIds\*AKY1005
HKLM\DRIVERS\DriverDatabase\DeviceIds\*AKY1009
HKLM\DRIVERS\DriverDatabase\DeviceIds\*AKY1013
HKLM\DRIVERS\DriverDatabase\DeviceIds\*ANX2101
HKLM\DRIVERS\DriverDatabase\DeviceIds\*AZT0003
HKLM\DRIVERS\DriverDatabase\DeviceIds\*AZT3001
HKLM\DRIVERS\DriverDatabase\DeviceIds\*AZT4001
HKLM\DRIVERS\DriverDatabase\DeviceIds\*AZT4004
HKLM\DRIVERS\DriverDatabase\DeviceIds\*AZT4017
HKLM\DRIVERS\DriverDatabase\DeviceIds\*AZT4021
HKLM\DRIVERS\DriverDatabase\DeviceIds\*BDP0156
HKLM\DRIVERS\DriverDatabase\DeviceIds\*BDP2336
HKLM\DRIVERS\DriverDatabase\DeviceIds\*BDP3336
HKLM\DRIVERS\DriverDatabase\DeviceIds\*BRI1400
```
 Additionally, registry keys regarding firewall rules have also been deleted along with some keys for Windows Error Reporting.
 ```
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\StickyUpdates\657943f1-1efb-430a-a6c8-f77993103709: 0x000001908FF987AC
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\WindowsUpdate\StickyUpdates\46535ccb-755d-4cf3-9c00-8f36a4f7060b: 0x000001908FFC0D23
HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\TermReason\6060\Terminator: "HAM"
HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\TermReason\6060\Reason: 0x00000004
HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\TermReason\6060\CreationTime: 0x01DAF28F12F51679
HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\TermReason\6624\Terminator: "HAM"
HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\TermReason\6624\Reason: 0x00000004
HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\TermReason\6624\CreationTime: 0x01DAF28F1B0CF517
HKLM\SYSTEM\ControlSet001\Control\hivelist\\REGISTRY\MACHINE\DRIVERS: 5C 00 44 00 65 00 76 00 69 00 63 00 65 00 5C 00 48 00 61 00 72 00 64 00 64 00 69 00 73 00 6B 00 56 00 6F 00 6C 00 75 00 6D 00 65 00 31 00 5C 00 57 00 69 00 6E 00 64 00 6F 00 77 00 73 00 5C 00 53 00 79 00 73 00 74 00 65 00 6D 00 33 00 32 00 5C 00 63 00 6F 00 6E 00 66 00 69 00 67 00 5C 00 44 00 52 00 49 00 56 00 45 00 52 00 53 00 00 00 00 00
HKLM\SYSTEM\ControlSet001\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\{3D860988-2D74-4B56-9311-8CEC8F26648C}: "v2.30|Action=Allow|Active=TRUE|Dir=Out|Profile=Domain|Profile=Private|Profile=Public|Name=@{Microsoft.DesktopAppInstaller_1.0.30251.0_x64__8wekyb3d8bbwe?ms-resource://Microsoft.DesktopAppInstaller/Resources/appDisplayName}|Desc=@{Microsoft.DesktopAppInstaller_1.0.30251.0_x64__8wekyb3d8bbwe?ms-resource://Microsoft.DesktopAppInstaller/Resources/appDisplayName}|LUOwn=S-1-5-21-2468057532-1901412999-489411066-1000|AppPkgId=S-1-15-2-2473817148-3930944034-1235795307-187980641-3967865409-1804095407-1113801530|EmbedCtxt=@{Microsoft.DesktopAppInstaller_1.0.30251.0_x64__8wekyb3d8bbwe?ms-resource://Microsoft.DesktopAppInstaller/Resources/appDisplayName}|Platform=2:6:2|Platform2=GTEQ|"
HKLM\SYSTEM\ControlSet001\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\{3A3FF571-CC7C-41F5-9091-9B1370BB0794}: "v2.30|Action=Allow|Active=TRUE|Dir=In|Profile=Domain|Profile=Private|Name=@{Microsoft.DesktopAppInstaller_1.0.30251.0_x64__8wekyb3d8bbwe?ms-resource://Microsoft.DesktopAppInstaller/Resources/appDisplayName}|Desc=@{Microsoft.DesktopAppInstaller_1.0.30251.0_x64__8wekyb3d8bbwe?ms-resource://Microsoft.DesktopAppInstaller/Resources/appDisplayName}|LUOwn=S-1-5-21-2468057532-1901412999-489411066-1000|AppPkgId=S-1-15-2-2473817148-3930944034-1235795307-187980641-3967865409-1804095407-1113801530|EmbedCtxt=@{Microsoft.DesktopAppInstaller_1.0.30251.0_x64__8wekyb3d8bbwe?ms-resource://Microsoft.DesktopAppInstaller/Resources/appDisplayName}|Platform=2:6:2|Platform2=GTEQ|"
 ```

There are a 136 new registry values added. Not all of them may be relevant to our purpose. Browsing through the list, we find a few that may be of relevance.
```
HKU\S-1-5-21-2468057532-1901412999-489411066-1000\SOFTWARE\Microsoft\Windows\CurrentVersion\Run\ksroqtsyfbvuio971: ""C:\Users\taygr\Desktop\tasksche.exe""
HKU\S-1-5-21-2468057532-1901412999-489411066-1000\SOFTWARE\WanaCrypt0r\wd: "C:\Users\taygr\Desktop"
```

The first entry is a value under `SOFTWARE\Microsoft\Windows\CurrentVersion\Run\`. Values under the `Run` key are executed automatically at startup. The program being run is one of the executables we found in the resources of the malware sample. The second entry is clearly made by the wannnacry malware. The key value is just the desktop directory (We will see where this comes from later).

There are 265 modified regsitry values. Out of all of them, only a few are relevant. These are the reason our desktop wallpaper changes.
```
HKU\S-1-5-21-2468057532-1901412999-489411066-1000\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Wallpapers\BackgroundHistoryPath0: "C:\ProgramData\_VM\background.png"
HKU\S-1-5-21-2468057532-1901412999-489411066-1000\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Wallpapers\BackgroundHistoryPath0: "C:\Users\taygr\Desktop\@WanaDecryptor@.bmp"
HKU\S-1-5-21-2468057532-1901412999-489411066-1000\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Wallpapers\BackgroundHistoryPath1: "C:\Windows\web\wallpaper\Windows\img0.jpg"
HKU\S-1-5-21-2468057532-1901412999-489411066-1000\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Wallpapers\BackgroundHistoryPath1: "C:\ProgramData\_VM\background.png"
``` 

<a id="markdown-procmon" name="procmon"></a>

## Procmon

Procmon or Process Monitor is a built-in Windows tool that is extremely useful to keep track of what programs do. We can look at the operations, target files, target paths, links between different processes and undestand the relationships between different processes. To begin our analysis, we filter the entries whose process name is the name of our malware sample `ed01ebfbc9eb5bbea545af4d01bf5f1071661840480439c6e5babe8e080e41aa.exe`.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/procmon2.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Filtering just by the process name gives us close to a million entries. That is of no use to us. We need a way to filter into the entries that matter. One filter that I particularly like to use is to filter for `cmd.exe`. These are usually ways for programs to interact with the system. Further, earlier during the static analysis of the strings, we saw `cmd.exe /c %s`. That should show up somewhere in the logs and we can see what sort of commands were run.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/procmon1a.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

As expected, there are a number of entries that use `cmd.exe`. 2 of them are particularly interesting. The first one is the command line to start `@WanaDecryptor@.exe`
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/procmon3.png" class="img-fluid rounded z-depth-1" style="height: 300px; width: auto;"%}
    </div>
</div>

The second entry of note is one that creates the `Run` registry entry that we saw earlier in Regshot.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/procmon4.png" class="img-fluid rounded z-depth-1" style="height: 300px; width: auto;"%}
    </div>
</div>

Procmon also lets us filter processes that were spawned by a given process. We can use this to see what processes were spawned by our executable. There are a LOT of entries for processes spawned by our executable.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/procmon5.png" class="img-fluid rounded z-depth-1" style="height: 300px; width: auto;"%}
    </div>
</div>

The first one we see is this cmd command `attrib +h .`. This command is used to set the hidden attribute of the current directory. The malware probably creates some hidden files/folders (sneaky).

Another entry we see is for `icacls.exe`
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/procmon6.png" class="img-fluid rounded z-depth-1" style="height: 300px; width: auto;"%}
    </div>
</div>
The command `icacls ./grant Everyone:F /T /C /Q` grants all users permission to the current directory and all subdirectories and files in the current directory. It ensures that the command continues even on failure and suppresses completion and error messages, ensuring the whole process is invisible to the victim.

We see a few more tasks spawned, one of which is the `taskdl.exe` file we saw earlier.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/procmon7.png" class="img-fluid rounded z-depth-1" style="height: 300px; width: auto;"%}
    </div>
</div>

Looking through the entries for this process doesn't reveal much about what it does. Maybe we can come back to this at a later stage when we reverse engineer the executables. Next is a cmd process launching a `299281724124687.bat` file. Similarly, this the entries for this process don't help much and we skip it for now.

The last process that is spawned is `@WanaDecryptor@.exe`. There are multiple instances of this executable being spawned, most of which look similar. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/procmon8.png" class="img-fluid rounded z-depth-1" style="height: 300px; width: auto;"%}
    </div>
</div>

This one is also a difficult to say what it is doing exactly, but we can notice a few behaviours.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/procmon10.png" class="img-fluid rounded z-depth-1" style="height: 300px; width: auto;"%}
    </div>
</div>
It seems to be working using <a href="https://www.torproject.org/about/history/">Tor</a>. This may be to exfiltrate data or talk to a remote server. Additionally, it is performing a lot of read actions on `s.wnry`. And most tellingly, it performs TCP sends and receives.
<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/procmon11.png" class="img-fluid rounded z-depth-1" style="height: 300px; width: auto;"%}
    </div>
</div>

We look at TCPView and see `WanaDecryptor` is communicating over port 9050 to the localhost.
 <div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/procmon12.png" class="img-fluid rounded z-depth-1" style="height: 300px; width: auto;"%}
    </div>
</div>

Opening wireshark on victim machine and looking for traffic to and from port 9050 shows packets that have `.onion`. This confirms that Tor is being used to exfiltrate some sort of information from the computer. However, this is done on the localhost only so it may not be reaching out to a C2 server or a attacker machine. We can confirm more when we look at the decompile of the program.
 <div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/wcry/procmon13.png" class="img-fluid rounded z-depth-1" style="height: 300px; width: auto;"%}
    </div>
</div>
# **Conclusion**

This concludes our initial analysis of the Wannacry Ransomware. We now have general idea of what the malware does, what sort of commands it executes, registry keys made and some modes of persistence. In the next part, we shall perform a deeper analysis by reverse engineering the sample and running it in a debugger. Stay tuned and thanks for reading this! Feel free to reach out on LinkedIn/ Discord/ Email to share your comments and suggestions :)

