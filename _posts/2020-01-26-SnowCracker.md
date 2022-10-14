---
layout: post
title:  "SnowCracker"
date:   2020-09-21 00:10:35 +0200
subtitle: "SnowCracker to bruteforce stegsnow passwords"
background: "/img/bg1.png"
---

#### SnowCracker
Snowcracker used to crack Stegsnow passwords tool can preform decompression on extracted data

#### Install
1. `pip install future`
2. `git clone https://github.com/0xHasanM/SnowCracker.git && cd SnowCracker`

#### Usage

* Without decompression  
   `python3 snowcracker.py -C N -f flag.txt -w wordlist`
* With decompression  
   `python3 snowcracker.py -C Y -f flag.txt -w wordlist`

 <img src="/img/SnowCracker.jpg" alt="Snow-Cracker Usage" width="800" height="440">
