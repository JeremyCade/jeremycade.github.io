---
layout: page
title: Debian / Ubuntu Snippets
---

A collection of Snippets, commands and other useful things for Debian / Ubuntu that I often forget, and need to lookup. 

# apt

### Listing available packages

Searching via `apt`

    apt-cache search packagename

### List installed packages

Via `apt` 

    apt list --installed

Alternative `dpkg`
List packages that have been installed locally. 

    dpkg --get-selections | grep -v deinstall

Search for a specific packages in the installed list

    dpkg --get-selections | grep packagename

Simple `dpkg` alternative

    dpkg -l

