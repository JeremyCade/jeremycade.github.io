---
layout: post
title:  "LaTeX on Ubuntu 14.10"
date:   2014-12-15
comments: true
categories: latex ubuntu 14.10
---

Writting a thesis for a years worth of research is down right fun. Even more so when trying to compile a large document with LaTeX. Normally compiling LaTeX is not an issue for me. The sizeable [MacTeX](https://tug.org/mactex/) packages generally contain all the packages I require. Though recently, I decided to dust off a old [MSI Wind12 U200 Netbook](http://www.msi.com/product/nb/U200.html#hero-overview) and install [Ubuntu 14.10](http://www.ubuntu.com/download/desktop) to use as my writing machine. My initial thought was that it was going to be a simple case of apt-get install and I would be off and running with a working LaTeX install. 


## General Document Layouts and LaTeX Packages
My standard LaTeX document makes use of the following packages:

- geometry
- fancyhdr
- inputenc
- glossaries
- graphicx
- biblatex


sudo apt-get install --yes texlive-full

glossaries.sty not found

sudo apt-get install --yes texlive-latex-extra

bibtex.sty not found

sudo apt-get install --yes texlive-bibtex-extra

