---
layout: post
title:  "foren-weekend-crash (CSAW-CTF-2021) writeup"
date:   2021-11-16 00:10:35 +0200
subtitle: "writeup for weekend-crash challange in CSAW CTF 2021 finals"
background: "/img/bg1.png"
---

#### Description
It’s Friday evening and you have been working nonstop for the past few days on your assignment due at midnight. Alas, fate is not on your side as your computer crashed! Luckily, with your forensics knowledge, you were able to take a snapshot of your memory. But now, your frazzled brain can’t remember what the state of your assignment was. Can you recover the information you need?

Challenge Files: https://drive.google.com/file/d/1Xyy__6d_bV3E5mRvF719i2nHJ0PHh7RZ/view?usp=sharing
Password: cs4w_2021_f1n4lz
#### Tools
   1. <a href="https://www.rstudio.com/" style="color:#0000EE;">Rstudio</a>   
   2. <a href="https://github.com/volatilityfoundation/volatility3" style="color:#0000EE;">Volatility3</a>   
   3. <a href="https://sqlitebrowser.org/" style="color:#0000EE;">DB browser for SQLite</a>   
   4. <a href="https://github.com/simsong/bulk_extractor" style="color:#0000EE;">bulk-extractor</a>
   5. <a href="https://gchq.github.io/CyberChef/#recipe=ROT13(true,true,false,13)" style="color:#0000EE;">CyberChef</a>


#### Walkthrough
We were given a 2GB windows 10 memory dump. I usually start my analysis by loading the dump in Rstudio, which scans the file for partitions signature and rebuilds the directory structure based on this structure.   
Starting Rstudio, add the image and scan for partitions. (For NTFS partitions, Rstudio search for $MFT and rebuild the directory structure based on the $MFT file.)   
After Rstudio finishes the scans, Righ-Click on the found partition and choose show files.   
<img src="/img/foren-weekend-crash/foren-weekend-crash-rstudio-analysis.png" alt="sourcecode" width="800" height="440">
The download folders show that the user downloaded a firefox installer. Checking the program files, we found that only firefox is the installed program, so that is our target.   
<img src="/img/foren-weekend-crash/foren-weekend-crash-downloads.png" alt="sourcecode" width="800" height="440">
Now let's move our analysis to Volatility3. Based on the information we were able to extract, we need to dump firefox profiles.   
First, we need to list the files present in the memory dump using the following command `vol3 -f weekend_crash.bin windows.filescan > output.txt`. Looking for firefox files, we found many files.   
<img src="/img/foren-weekend-crash/foren-weekend-crash-filescan.png" alt="sourcecode" width="800" height="440">
Let's check firefox history first, which is stored in places.sqlite. Use `vol3 -f weekend_crash.bin windows.dumpfiles --virtaddr 0x940cdc0252d0` command to dump the places.sqlite file and use DB Browser for SQLite to analyze the content of places.sqlite.   
<img src="/img/foren-weekend-crash/foren-weekend-crash-places.sqlite.png" alt="sourcecode" width="800" height="440">
Examining the history, we found the `http://congon4tor.com:7777/` website, which returns `No flag sorry`.   
<img src="/img/foren-weekend-crash/foren-weekend-crash-noflag.png" alt="sourcecode" width="800" height="440">
Maybe there is a cookie we need to inject, so let's dump cookies.sqlite and examine it with DB Browser for SQLite.   
<img src="/img/foren-weekend-crash/foren-weekend-crash-cookies.png" alt="sourcecode" width="800" height="440">
As we thought, we found a cookie with the name `magic` and value `6670ee0a08c2818f76256954c72ad077. Injecting cookies to the website, we got the flag, but it is encoded with rot13.   
<img src="/img/foren-weekend-crash/foren-weekend-crash-cookiessql.png" alt="sourcecode" width="800" height="440">
Using cyberchef, we were able to decode it.   
<img src="/img/foren-weekend-crash/foren-weekend-crash-flag.png" alt="sourcecode" width="800" height="440">
