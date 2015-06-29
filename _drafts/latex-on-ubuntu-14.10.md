---
layout: post
title:  "LaTeX on Ubuntu 14.10"
comments: true
categories: latex ubuntu 14.10
---

Writing a thesis for a years worth of research is down right fun. Even more so when trying to compile a large document with LaTeX. Normally compiling LaTeX is not an issue for me. The sizable [MacTeX](https://tug.org/mactex/) packages generally contain all the packages I require. Though recently, I decided to dust off a old [MSI Wind12 U200 Netbook](http://www.msi.com/product/nb/U200.html#hero-overview) and install [Ubuntu 14.10](http://www.ubuntu.com/download/desktop) to use as my writing machine. My initial thought was that it was going to be a simple case of apt-get install and I would be off and running with a working LaTeX install and all of my required packages.

My standard LaTeX document makes use of the following packages:

- biblatex
- fancyhdr
- geometry
- glossaries
- graphicx
- inputenc

Nothing out of the ordinary, or packages that you wouldn't expect to be installed as part of a standard [TeX Live](https://www.tug.org/texlive/) install.

To ensure everything plays nice, I make use of the following Makefile:

    # LaTeX Makefile
    FILE=main

    all: $(FILE).pdf

    .PHONY: clean
    clean:
	    rm -f *.aux *.blg *.out *.bbl *.log
	    rm -f *.toc *.bcf *.xml *.pdf
	    rm -f $(FILE)-blx.bib $(FILE).glo $(FILE).ist
	    rm -f $(FILE).glg $(FILE).gls $(FILE).glsdefs

    $(FILE).pdf: $(FILE).tex
	    pdflatex $(FILE)
	    bibtex $(FILE)
	    pdflatex $(FILE)
	    makeglossaries $(FILE)
	    pdflatex $(FILE)

## Installing TexLive

Ubuntu's package repository contains a number of Tex Live packages. My initial approach was to install the texlive-full package.

    sudo apt-get install --yes texlive-full

A quick **make** resulted in LaTeX throwing an error

    pdflatex main
    make: pdflatex: Command not found
    make: *** [main.pdf] Error 127



    sudo apt-get install --yes texlive-full

    glossaries.sty not found

    sudo apt-get install --yes texlive-latex-extra

    bibtex.sty not found

    sudo apt-get install --yes texlive-bibtex-extra
