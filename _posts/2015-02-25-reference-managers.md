---
layout: post
title: (Another) Best reference manager for academics
comments: true
---
## Best reference manager for academics (Includes a recommendation)

While there is no shortage of articles about what is the best reference manager, I wanted to know what is the recommended option for scholars. I started with a comparison chart in [wikipedia](http://www.wikiwand.com/en/Comparison_of_reference_management_software) and then wandered the web in search for advice. 

I think [this one](http://book.openingscience.org/tools/reference_management.html) is one of the best articles in the web about reference managers. However, all articles suggest that you should pick a reference manager that suits your own individual needs, or more accurately your work-flow. 

I tried to narrow down my focus to the context of the academia and soon enough I found [this question](https://www.researchgate.net/post/Which_is_the_best_Reference_Management_Software_for_research_scholars/1) in Research Gate from which I compiled a [database](https://dl.dropboxusercontent.com/u/128600/posts/RGRefManPref.csv) to further analyze.

## The analysis

I used the database to explore the preferences of the participants in the forum. I used **R** for all my analyses, here is some code. 

	library(RCurl)  
	library(dplyr)  
	library(ggplot2)

	#read the csv file
	x <- getURL("https://dl.dropboxusercontent.com/u/128600/posts/RGRefManPref.csv")    
	data <- tbl_df(read.csv(text = x))  #import as data table  

After I imported the database I made, I wandered if there was any association between the recommendation and the user reputation (i.e. Research Gate Score and Impact Points), let's check out. 

	# plot the data
	qplot(data = data, 
	      x = RG.Score, 
	      y = Recommend, 
	      ylab = "Recommended Reference Manager",
	      geom = "point", 
	      size = Impact.Points)

The code above results in this plot:
![ref manager recommendation by Research Gate Score](https://dl.dropboxusercontent.com/u/128600/posts/RGRefManPref.png)

It may be more clear if we get the average RG Score by reference manager, let's tabulate this.

	data %>%
	  select(Recommend, RG.Score) %>%
	  group_by(Recommend) %>%
	  summarise(Score = mean(RG.Score)) %>%
	  arrange(desc(Score))

|    Recommended Reference Manager |  Average RG Score |
|----|----|
| endnote + refworks | 72.41 |
| suggested a list   | 58.46 |
| paperpile          | 28.61 |
| reference manager  | 25.87 |
| papers             | 24.41 |
| pasteref           | 23.20 |
| endnote            | 19.13 |
| zotero             | 12.05 |
| docear             | 10.89 |
| mendeley           |  9.19 |
| refworks           |  7.64 |
| jabref             |  6.39 |
| wizfolio           |  2.08 |
| citavi             |  0.75 |

It looks like the higher the score the less popular is the suggestion, or more probably, the less common suggestions have only one or two backers with high RG Score. This may also mean (and the graph suggest it is that way) that the most common suggestions have a wide spread in reputation.

Finally let's take a look at what are the most common suggestions by making a ranking of the recommendations that had more than one supporter.

	# table: most recommended
	data %>% 
	  select(Recommend) %>% 
	  group_by(Recommend) %>% 
	  tally() %>% 
	  filter(n > 1) %>% 
	  arrange(desc(n))

the code above resulted in the following table:


 |Recommended Reference Manager | N° of Recommendations  |
 |----|----|
 | EndNote  | 11 |
 | Mendeley | 10 |
 | Zotero   | 10 |
 | Citavi   | 2  |
 | Docear   | 2  |
 | Papers   | 2  |


This ranking somehow resembles the one found in [this post](http://www.docear.org/2013/11/11/on-the-popularity-of-reference-managers-and-their-rise-and-fall/) made with a completely different methodology (i.e. estimation of popularity using the number of search events for a specific term).

Here is one nice graph from that article.  
![average ranking](http://www.docear.org/wp-content/uploads/2013/11/img_527cf2747f936.png)


I would say that all of the above share a set of basic features that should satisfy the average (academic) user, that is:

- Search and import citations
- Gather metadata from PDF files
- Organize your citations
- Annotate citations and files
- Share your database with collaborators
- Export database in standard formats (e.g. RIS, BibTeX)
- Format citations according to various journal styles
- Integrate with word processing software

So let's focus on the unique features of each one of them.

### [EndNote](http://endnote.com/)

EndNote is a paid software, it cost $249 USD but student discounts (via a third-party retailer) are available. While this price tag may look expensive, consider that the storage capacity for files is *unlimited*. You can also try it for 30 days or use the free online version (with a 2 GB limit for files).

It works on Windows, Mac and Ipad, it also offers a web version. Apart from the standard features, it does offer some additional features like generating sections in your bibliography or multiple bibliographies in a single file (you can have multiple documents to overcome this limitation in other software). A detailed list of features can be found [here](http://endnote.com/product-details) but to get into the real detail you have to check the manuals. Notice that the ["little" How-to book](http://dewey.thomsonreuters.com/training/Little%20Book/Little_EndNote_How-To_Book.pdf) (which is NOT the manual) has more than a hundred pages.

### [Mendeley](http://www.mendeley.com/)

Mendeley works in Windows, Mac, Linux and iOS as well as a web version. It is free, but **requires** an account, even if you use it in your desktop, you have to log in. You only have to pay if you exceed th 5 GB sync limit, there are personal ($14.99 USD/month for unlimited) and team plans ($49 for 5 collaborator up to $249 USD for 50).

It is very good at managing an existing *pdf* collection. One of its unique features is the ability to export **and** sync to a single bibtex file or separated files by collection or document.

### [Zotero](https://www.zotero.org/)

This program is free and open-source, it works on Windows, Mac, Linux and online. You **can** set up an account but it is not mandatory. The *pdf* indexing requires a plug-in but you can find an easy way of installing it in the preferences.

It is good at managing various kind of file types, specially good at capturing online sources. You can can have *Saved searches* that can automatically populate a collection based of a search pattern. You can also generate a time-line of references and highlight interesting articles with search filters.

You can sync your library database files only or the totality of your files. Up to 300 MB are for free, but you can pay for different storage plans ($10 USD/ month for unlimited storage). 

### [Citavi](https://www.citavi.com/)

Citavi is the self proclaimed leader in the German-speaking universities, despite it is Windows only. The free edition limits the references per project to 100.

There is a complete list of features in their website that you can [explore](https://www.citavi.com/sub/manual4/en/feature_overview.html) but I think that the features that set this one apart are the *Task planner* and the *Knowledge organizer*, this last feature let you organize your citations according to an outline of your project.
![screenshot](https://www.citavi.com/images/screenshots/Citavi4-Screen2-en-840.png)

If you have online resources you may find useful its ability to save webpages as *pdf* files. Apart from the standard integration with MS Word it offers an add-on for the most popular LaTeX editors. 

### [Docear](http://www.docear.org/)

Managing references it's only one aspect of this Mac, Windows and Linux software. 
Free and open-source, Docear is an interesting idea, it works more like a project manager using mind-mapping concepts and letting you draft your article directly within the application. 
![docear screenshot](http://www.docear.org/wp-content/uploads/2013/08/img_521cbea60cd22.png)

The program uses your own *pdf* reader to annotate the file and then imports the resulting metadata back to the program. It is integrated with [JabRef](http://jabref.sourceforge.net/) for reference and Bibtex file management.

The learning curve may be a little steep but if you fancy organizing your ideas through mind-maps (and you don't mind the 80's look) this is the software for you. 

### [Papers](http://www.papersapp.com/)

Papers  was originally a Mac aplication that it is currently implemented also in Windows and Online. It costs $79 USD but you can get a 40% discount if you are a student (you have to show a picture of your student ID).

One of its most notable features is the existence of *Magic Citations* that enable this program to place citations and bibliographies in a lot of different programs, including plain-text editors.

The sync options are offered via Dropbox.

## My Recommendation

After all of this research I would say that you should pick from Mendeley or Zotero, unless you have very specific needs that are not met by these. They are free and offer great support.

Further, if you have an existing collection of *pdf* files I would suggest go with Mendeley, it is better than Zotero at "curating" your database. Also, if you are a LaTeX user, pick this one because of the ability to synchronize your library with a `.bib` file. If most of your references come from scrapping the web and your files are not only *pdf* but all kind of sources go for Zotero. This is also your option if you are an advocate of open-source.

As a more detailed recommendation, my own set up is Mendeley (sync of files turned off) synchronizing a master `.bib` file that I use to insert citations in LaTeX files via the Latextools package for SublimeText 3.

Finally, the guys at Docear have also made [very detailed](http://www.docear.org/2014/01/15/comprehensive-comparison-of-reference-managers-mendeley-vs-zotero-vs-docear/) comparison between their product and their competitors Zotero and Mendeley, it worths taking some time to read it (including the comments). 

Thanks for reading. :fish:
