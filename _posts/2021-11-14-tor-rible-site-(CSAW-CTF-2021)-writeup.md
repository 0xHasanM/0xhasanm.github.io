---
layout: post
title:  "tor-rible-site (CSAW CTF 2021) writeup "
date:   2021-11-14 00:10:35 +0200
subtitle: "writeup for tor-rible-site challange in CSAW CTF 2021 finals"
background: "/img/bg1.png"
---

#### Description
Hidden deep within the dark web lies the flag you so need.

Author: Stephen Mondiguing, SecurityScorecard

(NOTE: the author used a slightly different flag format. Capitalize FLAG, e.g. FLAG{3x4mpl3_fl4g}, when submitting to CTFd.)
#### Tools
   1. <a href="https://github.com/OJ/gobuster" style="color:#0000EE;">Gobuster</a>
   2. <a href="https://itsfoss.com/install-tar-browser-linux/" style="color:#0000EE;">Tor proxy</a>
   3. <a href="https://github.com/xmendez/wfuzz/blob/master/wordlist/general/common.txt" style="color:#0000EE;">common wordlist</a>
   4. <a href="https://github.com/dzzie/pdfstreamdumper" style="color:#0000EE;">pdfstreamdumper</a>   
#### Walkthrough

We were given an <a href="http://pmh35xhqgkv52vmzkmdqnkmjzv4zjam25asdirnhykz6s76qfac57pid.onion/">.onion</a> website to examine. Check the source code of the website we found an html comment which states that there is a hidden folder in the website.
<img src="/img/tor-rible-site-sourcecode.png" alt="sourcecode" width="800" height="440">
Now we need to perform a directory bruteforce on the onion website, for that we need to direct our bruteforce traffic through a tor proxy.
First we need to install tor proxy using the following command `sudo apt install -y tor`. Running tor proxy the output shows that the proxy is a SOCKS which runs on port 9050.
<img src="/img/tor-rible-site-torproxy.png" alt="sourcecode" width="800" height="440">
After running tor proxy properly we will use gobuster with common.txt wordlist from wfuzz to bruteforce the directories under the onion website.
Running gobuster `gobuster dir --proxy socks5://127.0.0.1:9050 -u http://pmh35xhqgkv52vmzkmdqnkmjzv4zjam25asdirnhykz6s76qfac57pid.onion/ -w common.txt` gives us two direcotry 'backups' and 'hidden'.
<img src="/img/tor-rible-site-gobuster.png" alt="sourcecode" width="800" height="440">
Browsing backups folder we found two files 'alice.jpg' and 'lookharder.pdf'.
<img src="/img/tor-rible-site-backups.png" alt="sourcecode" width="800" height="440">
Checking the strings of the two files we found a flag.txt in the pdf file.
<img src="/img/tor-rible-site-pdfstrings.png" alt="sourcecode" width="800" height="440">
Loading the pdf file in pdfstreamdumper we found the flag in one of the pdf streams.
<img src="/img/tor-rible-site-flag.png" alt="sourcecode" width="800" height="440">
