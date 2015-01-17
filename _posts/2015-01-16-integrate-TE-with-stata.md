---
layout: post
title: Integrate your text editor with Stata
comments: true
---

## How to integrate your text editor with [Stata](www.stata.com)  

I've been using Stata for almost a year now and I like it a lot, but the integrated do-file editor is somehow limited; also, I love the advanced features that text editors have to offer (multiple cursors, block selection, regex search/replace, etc), and of course the possibility of using my text editor to do other things like run **R** code or compile documents written in **Latex** or **Markdown**. 


### Choose a text editor

I will be focusing on [Sublime Text](www.sublimetext.com/3) (free and cross-platform) and [Notepad++](http://notepad-plus-plus.org) (free and open-source, Windows only) but there is plenty of [alternatives](http://lifehacker.com/five-best-text-editors-1564907215).


### Notepad++

**Download and install Notepad:** note that these instructions are for Windows only.

**Enable Stata syntax-highlighting:** go to the [user defined languages](http://sourceforge.net/apps/mediawiki/notepad-plus/index.php?title=User_Defined_Language_Files) page, look for 'Stata' and download the corresponding ``xml`` file, save it in your *downloads* folder. Open Notepad++ and in the *Language* menu select *Define your language*, then press the *Import* button, select your source file and you are done.  
(*Update* the provided link is not working, but you can get a copy [here](https://code.google.com/p/notepad-stats-integration/source/browse/userdefineLang_stata.xml)).

**Install and configure Rundolines:** for this implementation we will be using a small program called ``rundolines``,  you can find the program and general instructions on how to install it in [Friedrich Huebler's page](http://huebler.blogspot.ca/2008/04/stata.html)

The short version is:  
- Download the program from [here](https://www.dropbox.com/s/58jiwvol59y619e/rundolines41.zip)  
- Unzip it in your ADO personal directory (usualy ``C:\ado\personal`` )   
- Edit the the ``INI`` files:  
`` stata "C:\Program Files\Stata13\StataSE.exe" `` should match your path  
`` statawin "Stata/SE 13.1" `` should match your version

In my experience this is enough to make it work with Stata 12 and 13, but some other options are available and may be needed to make the script work properly, visit the developer page to explore those options.

**'Connect' Notepad++ with ``rundolines``:** in Notepad++ go to the *Run* menu and select *run*, browse to where you saved ``rudolines.exe`` and select it. You have to add quotes to the path, so before clicking save it should look like `` "C:\ado\personal\rundolines.exe" ``.  
Once you save it you can assign a name and shortcut, avoid combinations with **CTRL** or **SHIFT** (**F9** is recommended by the developer).

### Sublime Text

**Download and install Sublime Text:** select and download the binaries that correspond to your operating system in the [download page](www.sublimetext.com/3).

**Install package control:** go to [this page](https://packagecontrol.io/installation) and proceed with the installation as instructed.

**Install and configure Stata Enhanced package:** once package control has been installed you can invoke the command prompt by pressing `` Ctrl + Shift + P `` (Windows) or `` Command + Shift + P `` in OS X, start writing 'install' and Select *Package Control: Install Package* from the list. Search 'Stata Enhanced' and press `` Enter ``.  
If your version of Stata is Stata 13/SE you don't need any additional steps, if it's not your case just go to the [Stata Enhanced page](https://github.com/andrewheiss/SublimeStataEnhanced) and follow the instructions for your operating system.

Easy peasy, lemon squeezy! 

### My 2 cents

Notepad++ may be easier to use and personalize; however, personally I prefer Sublime text, I think it looks better and the overall experience feels more polished, plus I can use it in OS X and Windows and Linux (to some extent).  

Enjoy!!

