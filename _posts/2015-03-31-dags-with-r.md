---
layout: post 
title: Drawing DAGs with R
comments: True
---

# Directed acyclic diagrams (DAG) with R

In preparation for my comprehensive exam I have been looking for easy and fast ways to draw causal diagrams. In LaTeX this can be achieved with some effort using the *TikZ* package, some examples are [this](https://uponthepeople.wordpress.com/2012/03/16/coding-dags-in-latex/) and [this](http://www.konstantinkashin.com/blog/2013/06/19/dags-in-latex/).

I am planning to write the report for the data analysis portion of my exam using [Rmarkdown](http://rmarkdown.rstudio.com/), so I want to draw the causal diagram directly from R. Enter the **DiagrammeR** package.

> "With the DiagrammeR package, you can create graph diagrams and flowcharts using R. Markdown-like text is used to describe a diagram and, by doing this in R, we can also add some R code into the mix and integrate these diagrams in the R console, through R Markdown, and in shiny web apps."

# A short Example using DiagrammeR

The [DiagrammeR](https://github.com/rich-iannone/DiagrammeR) package let us access the drawing capacities of [mermaid](http://knsv.github.io/mermaid) and [graphViz](http://www.graphviz.org).

For this example I will be reproducing Example 1.4 "A causal diagram of factors affecting fertility in cows", from the textbook by Dohoo, I., W. Martin, and H. Stryhn. [*"Veterinary epidemiologic research."*](http://www.amazon.ca/Veterinary-Epidemiologic-Research-Ian-Dohoo/dp/B009YW4ITO) (2009).

Let's start by using *mermaid*:

``` r	

	library(DiagrammeR)

	mermaid("
	        graph LR
	        A(Age)-->F(Fertility)
	        A-->O(Cistic ovarian <br> disease)
	        A-->R(Retained <br> placenta)
	        R-->O
	        R-->M(Metritis)
	        M-->O
	        O-->F
	        M-->F
	        ")
```	

![Imgur](http://i.imgur.com/gUp57Hr.png)

That looks good but since I am trying to reproduce what is in the book, I want to get rid of the boxes, for this we define a style for each node (white fill and white border).

``` r	

	        mermaid("
	        graph LR
	        A(Age)-->F(Fertility)
	        A-->O(Cistic ovarian <br> disease)
	        A-->R(Retained <br> placenta)
	        R-->O
	        R-->M(Metritis)
	        M-->O
	        O-->F
	        M-->F
	
	        style A fill:#FFFFFF, stroke-width:0px
	        style R fill:#FFFFFF, stroke-width:0px
	        style M fill:#FFFFFF, stroke-width:0px
	        style O fill:#FFFFFF, stroke-width:0px
	        style F fill:#FFFFFF, stroke-width:0px
	        ")

```

![Imgur](http://i.imgur.com/JRSOR2V.png)

The same can be achieved with the flexible and nicer but more complicated graphViz:

``` r

	grViz("
	digraph causal {
	
	  # Nodes
	  node [shape = plaintext]
	  A [label = 'Age']
	  R [label = 'Retained\n Placenta']
	  M [label = 'Metritis']
	  O [label = 'Cistic ovarian\n disease']
	  F [label = 'Fertility']
	  
	  # Edges
	  edge [color = black,
	        arrowhead = vee]
	  rankdir = LR
	  A->F
	  A->O
	  A->R
	  R->O
	  R->M
	  M->O
	  O->F
	  M->F
	  
	  # Graph
	  graph [overlap = true, fontsize = 10]
	}")

```

![Imgur](http://i.imgur.com/EYU3pcg.png)

I got a very similar, yet a reflection of the original figure in the book.

# Conclusion

While *graphViz* allows for a lot of customization I think I will stick with the simple syntax of *mermaid* for the report. For publication however, I think *graphViz* is a serious contender for *TikZ*. Just visit the DiagrammeR [docs page](http://rich-iannone.github.io/DiagrammeR/docs.html) and learn how to draw very fancy diagrams without leaving the R environment.


Thanks for reading!
