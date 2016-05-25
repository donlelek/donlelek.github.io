---
layout: post
title: How to use Markdown and Pandoc in Sublime Text 3
comments: true
---

# A follow up on *"How to: Achieve a text-only work-flow"*

In the previous [post](http://donlelek.github.io/2015-03-09-text-only-workflow/) I described the minimum requirements to be able to achieve a text-only workflow using [**Markdown**](http://johnmacfarlane.net/pandoc/demo/example9/pandocs-markdown.html) and [**Pandoc**](http://johnmacfarlane.net/pandoc/). 

I mentioned that there is a *"Super easy setup"* option using [**Rstudio**](http://rmarkdown.rstudio.com/) as your only tool to achieve this (specially useful if you use R), but I also promised to show a more general approach using [Sublime Text 3](http://www.sublimetext.com/3) along with some extensions to automatize the process.

# What do you need?

- Pandoc
- TeX distribution (only for PDF)
- Sublime Text 3 (ST3)

and some Extensions for ST3 which can be installed following the links below, but I strongly recommend installing everything in ST3 using [Package control](https://packagecontrol.io/installation)

- [Markdown Extended](https://packagecontrol.io/packages/Markdown%20Extended)
- [CiteBibtex](https://packagecontrol.io/packages/CiteBibtex)
- [Pandoc](https://packagecontrol.io/packages/Pandoc)

These Markdown extensions should work out-of-the-box, but Pandoc and CiteBibtex need some configuration. 

# Setup

I assume you have already installed *Pandoc* and *TeX* in your computer, if you haven't go to my previous [post](http://donlelek.github.io/2015/03/09/text-only-workflow/) and do it.

## Sublime Text 3

Now install ST3, visit http://www.sublimetext.com/3 and download the version that matches your OS. After installation proceed to activate package control following [this instructions](https://packagecontrol.io/installation).

## ST3 Extensions

When package control is installed you just invoke the *Command pallete* with the key combination `Shift + Ctrl + P` (`Shift + Command + P` in OS X), now start writing "insta..." and from the drop-down list select the Command **"Package Control: Install Package"**. It will take a second to show you the available packages, once it does, start typing the name of the package and use the arrows to select the one that you want and press `Enter` to proceed with the installation.

### Markdown Extended

> Markdown syntax highlighter for Sublime Text, with extended support for GFM fenced code blocks, with language-specific syntax highlighting. YAML Front Matter.

This is currently the most complete Markdown syntax highlighter for ST3 since it supports almost all Markdown specific syntax (with the exception of `<!--html comments-->` and `~~strikethrough~~` text) along with other languages when they are included in fenced blocks, like the following screenshot:

![Example](https://dl.dropboxusercontent.com/u/128600/posts/Screenshot%202015-03-24%2015.55.33.png) 

### CiteBibtex

> Effortlessly insert citations from BibTeX into texts written in Pandoc or LaTeX

This extension has two functions. When you press `F10` you will be able to search and insert a citation from a central BibTeX file.

![Screenshot](https://dl.dropboxusercontent.com/u/128600/posts/Screenshot%202015-03-25%2009.32.37.png)

Before that you have to configure the  `CiteBibtex.sublime-settings` file with the path to your BibTeX file. You access this file through the menu `Preferences > Package settings > CiteBibtex > Settings - User`.

``` json

	{
	    	"bibtex_file": "path/to/bibtex.bib",
	    	"autodetect_syntaxes": {"Markdown Extended": "pandoc",
		},
	}
```	

The second feature of this extension is the ability to extract the citations in your current document and generate a new BibTeX file with the same name as your document within the same folder. You do this by invoking the *Command pallete* and choosing **"CiteBibtex: extract citations in current file"**.

### Pandoc

> A Sublime Text plugin that uses Pandoc to convert text from one markup format into another. Pandoc can convert documents in markdown, reStructuredText, textile, HTML, DocBook, LaTeX, MediaWiki markup, OPML, or Haddock markup to XHTML, HTML5, HTML slide shows using Slidy, reveal.js, Slideous, S5, or DZSlides, Microsoft Word docx, OpenOffice/LibreOffice ODT, OpenDocument XML, EPUB version 2 or 3, FictionBook2, DocBook, GNU TexInfo, Groff man pages, Haddock markup, OPML, LaTeX, ConTeXt, LaTeX Beamer slides, PDF via LaTeX, Markdown, reStructuredText, AsciiDoc, MediaWiki markup, Emacs Org-Mode, Textile, or custom writers can be written in lua.

Finally, we arrived to the most useful extension. After you finish the configuration you will be able to summon Pandoc from the *Command pallete* and choose what kind of document you want to convert your markdown file to.

![Pandoc](https://dl.dropboxusercontent.com/u/128600/posts/Screenshot%202015-03-25%2009.50.19.png)

You access the configuration file through the menu `Preferences > Package settings > Pandoc > Settings - User`. I recommend the following configuration to be able to export to HTML, PDF and DOCX, it works perfectly on a Mac and fine in my Debian Laptop. You have to tweak the paths to make it work on windows.

``` r

	{
	  "default": {

	       "pandoc-path": "/usr/local/bin/pandoc",

	    "transformations": {

	      "HTML 5": {
	        "scope": {
	          "text.html.markdown": "markdown"
	        },
	        "syntax_file": "Packages/HTML/HTML.tmLanguage",
	        "pandoc-arguments": [
	          "-t", "html",
	          "--filter", "/usr/local/bin/pandoc-citeproc",
	          "--to=html5",
	          "--no-highlight", 
	        ]
	      },

	      "PDF": {
	        "scope": {
	          "text.html": "html",
	          "text.html.markdown": "markdown"
	        },
	        "pandoc-arguments": [
	          "-t", "pdf", 
	           
	          "--latex-engine=/usr/texbin/pdflatex",
	          "--filter", "/usr/local/bin/pandoc-citeproc"
	        ]
	      },

	      "Microsoft Word": {
	        "scope": {
	          "text.html": "html",
	          "text.html.markdown": "markdown"
	        },
	        "pandoc-arguments": [
	          "-t", "docx",  
	          "--filter", "/usr/local/bin/pandoc-citeproc"
	        ]
	      },

	    },
	    "pandoc-format-file": ["docx", "epub", "pdf", "odt", "html"]

	  }

	}

```	

Note that both   `"--latex-engine=/usr/texbin/pdflatex"` and `"--filter", "/usr/local/bin/pandoc-citeproc"` have to be specified with the full path.

# Wrap-up

This post shows you how to work without leaving ST3 for the preparation of manuscripts, the setup takes some time but after you are done, everything will be so much faster.

I hope you enjoy it.


