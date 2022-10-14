---
layout: post
title:  "Autopsy RegistryExplorer plugin"
date:   2022-02-08 00:10:35 +0200
subtitle: "Autopsy plugin to analyze registry hives"
background: "/img/bg1.png"
---

# Autopsy-Registry-Explorer
Autopsy Module to analyze Registry Hives based on bookmarks provided by <a href="https://github.com/EricZimmerman/RegistryExplorerBookmarks">EricZimmerman</a> for his tool <a href="https://ericzimmerman.github.io/#!index.md">RegistryExplorer</a>

## Specification
* Tested Autopsy version: 4.18.0+
* OS's supported on: Windows
* License: GNU General Public License Version 3

## Features
1. Analyse Registry hives based on bookmarks provided by <a href="https://github.com/EricZimmerman/RegistryExplorerBookmarks">EricZimmerman</a>
2. Ability to analyze registry hives independently without the need to load a full disk image
3. Categorize Keys according to their usage
4. Transaction logs analysis and determine wether the Registry Hive is dirty or not.

## Screenshot
<img src="https://raw.githubusercontent.com/0xHasanM/Autopsy-Registry-Explorer/main/screenshot.png" alt="Hash-Extension-Bruter Usage" width="800" height="440">

## Installation  
1. ```git clone https://github.com/0xMohammed/Autopsy-Registry-Explorer.git```  
2. ```copy Module folder to 'C:\Users\{Username}\AppData\Roaming\autopsy\python_modules'```

## Refrences  
[Autopsy discussion group](https://sleuthkit.discourse.group/t/creating-new-custom-artifact/2367)  
[Transaction logs analysis](https://github.com/EricZimmerman/RECmd/blob/7ea93bc53166d1c73386d9fe31aafc20759ac190/rla/Program.cs)    
[Sleuthkit API Reference](http://www.sleuthkit.org/sleuthkit/docs/api-docs/4.3/index.html)  
[Python Registry Parser](https://github.com/williballenthin/python-registry)
