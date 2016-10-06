---
layout: post
title: "All you need is ggplot"
comments: true
---

I recently stumbled into [this thread in stackoverflow](http://stackoverflow.com/questions/39870405/plotting-equations-in-r) and got inspired to design my own android wallpaper. 

![love_android.png](https://dl.dropboxusercontent.com/u/128600/posts/love_android.png)

I love dark wallpapers, they are both nice and save energy, [specially if your phone has an AMOLED screen](https://www.androidpit.com/how-black-wallpaper-can-save-your-battery). I used [Axeman's answer](http://stackoverflow.com/a/39871049/4654047) but the other answer was great too. The first part is just generating the relevant data frames.

``` r

    library(ggplot2)
    library(dplyr)

    # generate data frames based on functions 
    L <- data_frame(x = 1:100,
                    y = 1 / x)
    O <- data_frame(t = seq(-pi, pi, l = 100),
                    x = 3 * cos(t),
                    y = 3 * sin(t))
    V <- data_frame(x = -50:50,
                    y = abs(-2 * x))
    E <- data_frame(y = seq(-pi, pi, l = 100),
                    x = -3 * abs(sin(y)))
    # put all together and add equations 
    pd <- bind_rows(L, O, V, E, .id = "letter") %>%
      select(-t) %>% 
      mutate(letter = factor(letter, labels = c("y == 1/x",
                                                "x^2 + y^2 == 9",
                                                "y == abs(-2*x)",
                                                "x == -3*abs(sin(y))")))
    ```

The second part is interesting and I learned a lot trying to achieve the look I wanted. While trying to put all letters in one row using `facet_grid` I found some limitations on how you specify "free" scales, using a grid means a common scale has to be used for each row and column, the solution is using `facet_wrap` with `nrow = 1` instead. This works because in `facet_wrap` each plot is independent. Another thing I learned is how you can use the option `labeller = label_parsed` to use formulas in the facet title. 

``` r

    ggplot(pd, aes(x, y)) +
      # vline and hline add the "fake" axes
      geom_vline(xintercept = 0, color = "white") + 
      geom_hline(yintercept = 0, color = "white") +
      # the letters
      geom_path(size = 1.5, col = 'red') +
      # give each letter it's own plot area
      facet_wrap(~letter, 
                 # allow different x and y scales
                 scales = 'free',
                 # this allows facet titles to be formulas
                 labeller = label_parsed,
                 # put in one line
                 nrow = 1, 
                 # move facet titles to the bottom
                 switch = "x") +
      ggtitle("ALL YOU NEED IS") +
      # several tweaks to achieve final look
      theme_minimal() +
      theme(text = element_text(size = 30, color = "white"),
            plot.background = element_rect(fill = "black", color = "black"),
            axis.text = element_blank(), 
            axis.title = element_blank(),
            panel.grid = element_blank(),
            strip.text = element_text(color = "white"),
            title = element_text(face = "bold"))
    # saving as a 1920 x 1080 to match my phone's screen resolution
    ggsave("~/love_android.png", width = 19.20, height = 10.80, dpi = 100)
```

Hope you liked it.
Cheers!

