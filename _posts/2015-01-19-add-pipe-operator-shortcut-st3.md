---
layout: post
title: R users: add a pipe operator shortcut to SublimeText
comments: true
---

## Tired of typing %>% in R?

If you are using the [`dplyr`](http://cran.rstudio.com/web/packages/dplyr/vignettes/introduction.html) or [`tidyr`](http://cran.r-project.org/web/packages/tidyr/vignettes/tidy-data.html) packages in R, you're probably getting very familiar with the ``%>%`` symbol, known as the `pipe operator`. Rstudio (Version 0.98.1091) provides a keyboard shortcut to insert it, so I wanted to do the same in SublimeText.  

You can add a custom keybinding by opening sublime *Preferences* menu and selecting *Key Bindings - User*, this will open the file  ``Default (your_OS).sublime-keymap`` where `your_OS` is either OSX, Windows or Linux.  

Just paste the following code in the above mentioned file and save it.  

```  
[  
	{ "keys": ["super+shift+m"], "command": "insert_snippet", "args": {"contents": "%>%"}  
}  
]
```

This will insert the ``%>%`` string each time the ``Command + Shift + M`` key combination is pressed. I'm working on making it available only when editing R scripts but haven't found a solution yet... maybe you can help!

