---
layout: post
title:  "foren-no-time-to-register (CSAW-CTF-2021) writeup"
date:   2021-11-15 00:10:35 +0200
subtitle: "writeup for no-time-to-register challange in CSAW CTF 2021 finals"
background: "/img/bg1.png"
---

#### Description
James Bonderman has retrieved some files from an enemy agent's system. As one of R Branch's support engineers, Bonderman is trusting you to find any information relevant to his investigation. R has sent you a checklist of what he wants you to find. That should be easy for an agent such as yourself.

nc misc.chal.csaw.io 5015

Author: Regulus915ap

Challenge Files: https://drive.google.com/file/d/1Xyy__6d_bV3E5mRvF719i2nHJ0PHh7RZ/view?usp=sharing
#### Tools
   1. <a href="https://f001.backblazeb2.com/file/EricZimmermanTools/RegistryExplorer_RECmd.zip" style="color:#0000EE;">RegistryExplorer</a>   
   2. <a href="https://github.com/keydet89/RegRipper3.0" style="color:#0000EE;">RegRipper</a>   
   3. <a href="http://www.orionforensics.com/forensics-tools/usb-forensic-tracker/" style="color:#0000EE;">USB Forensic Tracker</a>   
   4. <a href="https://web.archive.org/web/20210126112458/https://www.dfir.training/resources/downloads/windows-registry" style="color:#0000EE;">Windows Registry analysis cheat sheet</a>
   5. <a href="https://www.digital-detective.net/dcode/" style="color:#0000EE;">DCode™ – Timestamp Decoder</a>
   6. <a href="http://www.nirsoft.net/utils/usb_devices_view.html" style="color:#0000EE;">USBDeview</a>
   7. <a href="https://github.com/mrpeardotnet/WinProdKeyFinder/releases/tag/V1.4" style="color:#0000EE;">WinProdKeyFinder</a>


#### Walkthrough
We were given a server to connect to, asking for data to extract from the registry hives (SAM, SYSTEM, SOFTWARE). We will start our analysis by running regrip to scan the registry hives then load them to registry explorer.   
<img src="/img/no-time-to-register-startofanalysis.png" alt="sourcecode" width="800" height="440">
1.**Hostname of the machine**   
   a. Registry Explorer: Check Tcpip in the bookmarks menu or browse to SYSTEM:ControlSet001\Services\Tcpip\Parameters.    
   b. RegRipper: Search for 'Hostname' in the system hive report.txt generated by RegRipper.    
   Answer: 5P3C7r3-1MP3r1UM   
   <img src="/img/foren-no-time-to-register-1.png" alt="sourcecode" width="800" height="440">
2.**Region the machine belong to**   
   a. Registry Explorer: Registry Explorer: check Timezoneinformation in the bookmarks menu or browse to SYSTEM:ControlSet001\Control\TimeZoneInformation.    
   b. RegRipper: Search for 'timezone' in the system hive report.txt generated by RegRipper.    
   Answer: Singapore   
   <img src="/img/foren-no-time-to-register-2.png" alt="sourcecode" width="800" height="440">
3.**User registered on the machine**   
   a. Registry Explorer: check Users in the bookmarks menu or browse to SAM:SAM\Domains\Account\Users.    
   b. RegRipper: Check registered users in the sam hive report.txt generated by RegRipper.    
   Answer: Spectre   
   <img src="/img/foren-no-time-to-register-3.png" alt="sourcecode" width="800" height="440">
4.**GUID of the user from 3**   
   a. Registry Explorer: browse to SOFTWARE:Microsoft\Windows NT\CurrentVersion\ProfileList. Check the keys listed under ProfileList.    
   b. RegRipper: Search for 'spectre' in the software hive report.txt generated by RegRipper.    
   Answer: S-1-5-21-4228526091-1870561526-3973218081-1001   
   <img src="/img/foren-no-time-to-register-4.png" alt="sourcecode" width="800" height="440">
5.**Last login time of user from 3 using the timezone found in 2. (Use format HH:MM:SS MM/DD/YYYY)**   
   a. Registry Explorer: check Users in the bookmarks menu or browse to SAM:SAM\Domains\Account\Users.    
   b. RegRipper: Check spectre in the sam hive report.txt generated by RegRipper.    
   Answer: 2021-11-01 09:01:28Z   
   <img src="/img/foren-no-time-to-register-5.png" alt="sourcecode" width="800" height="440">
6.**Other accounts enabled on the machine**   
   a. Registry Explorer: check Users in the bookmarks menu or browse to SAM:SAM\Domains\Account\Users. Check accounts which has last login date.    
   b. RegRipper: Check users in the sam hive report.txt generated by RegRipper.    
   Answer: Administrator   
   <img src="/img/foren-no-time-to-register-6.png" alt="sourcecode" width="800" height="440">
For USB artifacts, we will analyze registry hives with USB FORENSICS TRACKER and USBDeview.    
USB FORENSICS TRACKER: Click on the windows icon in the toolbar and select the folder which contains registry hives.   
<img src="/img/foren-no-time-to-register-USBU.png" alt="sourcecode" width="800" height="440">
USBDeview: Load the system hive from Options menu => Advanced Options   
<img src="/img/foren-no-time-to-register-usbdeview.png" alt="sourcecode" width="800" height="440">
7.**Name of USB attached to the machine**   
   Answer: 3v1L_Dr1v3   
   <img src="/img/foren-no-time-to-register-7-8.png" alt="sourcecode" width="800" height="440">
8.**Letter assigned to USB attached to the machine**   
   Answer: E   
   <img src="/img/foren-no-time-to-register-7-8.png" alt="sourcecode" width="800" height="440">
9.**Serial number of USB attached to the machine**   
   Answer: 04016cd7fe9bdb2e12fdc62886a111831a8be58c0143f781b2179f053e968   
   <img src="/img/foren-no-time-to-register-9-13.png" alt="sourcecode" width="800" height="440">
10.**Timestamp of first connection of USB to the machine. (Use format HH:MM:SS MM/DD/YYYY)**   
   Answer: First install time = 10/30/2021 10:36:59 PM      
11.**Timestamp of last connection of USB to the machine. (Use format HH:MM:SS MM/DD/YYYY)**   
   Answer: Connect time = 11/1/2021 9:49:01 AM      
12.**Timestamp of last write to USB attached to the machine. (Use format HH:MM:SS MM/DD/YYYY)**   
   Answer: Disconnect time = 11/1/2021 9:49:13 AM      
   <img src="/img/foren-no-time-to-register-usbdeview-sandisk.png" alt="sourcecode" width="800" height="440">
13.**GUID of USB attached to the machine**   
   Answer: d53e38d0-36db-11ec-ae51-080027ec0de9   
   <img src="/img/foren-no-time-to-register-9-13.png" alt="sourcecode" width="800" height="440">
14.**Windows Activation Key**   
   For this question, load SYSTEM hive in Registry explorer and browse to SOFTWARE:Microsoft\Windows NT\CurrentVersion. Export CurrentVersion key to reg format.   
   <img src="/img/foren-no-time-to-register-14-registryexplorer.png" alt="sourcecode" width="800" height="440">
   Open the reg file in Notepad and copy the digitalproductid. Use WinProdKeyFinder to decode the digitalproductid and get the activation key.   
   Answer: VK7JG-NPHTM-C97JM-9MPGT-3V66T   
   <img src="/img/foren-no-time-to-register-14-WinProdFinder.png" alt="sourcecode" width="800" height="440">
15.**Windows Edition**   
   a. Registry Explorer: check CurrentVersion (Windows NT) in the bookmarks menu or browse to SOFTWARE:Microsoft\Windows NT\CurrentVersion. Check ProductName.   
   b. RegRipper: search ProductName in the software hive report.txt generated by RegRipper.    
   Answer: Windows 10 pro   
   <img src="/img/foren-no-time-to-register-15.png" alt="sourcecode" width="800" height="440">
16.**Windows Version**   
   a. Registry Explorer: check CurrentVersion (Windows NT) in bookmarks menu or browse to SOFTWARE:Microsoft\Windows NT\CurrentVersion. Check DisplayVersion.   
   Answer: 21H1   
   <img src="/img/foren-no-time-to-register-16.png" alt="sourcecode" width="800" height="440">
17.**Windows Registered Owner**   
   a. Registry Explorer: check CurrentVersion (Windows NT) in bookmarks menu or browse to SOFTWARE:Microsoft\Windows NT\CurrentVersion. Check RegisteredOwner.   
   b. RegRipper: search RegisteredOwner in the software hive report.txt generated by RegRipper.   
   Answer: Spectre   
   <img src="/img/foren-no-time-to-register-17.png" alt="sourcecode" width="800" height="440">
18.**Windows Install Date. (Use format HH:MM:SS MM/DD/YYYY)**   
   a. Registry Explorer: check CurrentVersion (Windows NT) in bookmarks menu or browse to SOFTWARE:Microsoft\Windows NT\CurrentVersion. Check Install time. Use Dcode to decode windows time.    
   <img src="/img/foren-no-time-to-register-18-dcode.png" alt="sourcecode" width="800" height="440">
   b. RegRipper: search InstallDate in the software hive report.txt generated by RegRipper.   
   Answer: 2021-10-26 08:02:09Z   
   <img src="/img/foren-no-time-to-register-18-regripper.png" alt="sourcecode" width="800" height="440">
