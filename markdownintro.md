name: inverse
layout: true
class: flamingo
---

## How using markdown can free you from LaTeX, Word, Powerpoint (and Dropbox, too, if you use git with github or bitbucket)

.footnote[.left[.red[\*]Clemens Messerschmidt]]
---
## markdown is a markup language. (got it?)
As are .red[.html] and .red[.tex] (actually, tex is more than that.) 

## However, markdown is much simpler to write and __read__.

http://cpsievert.github.io/slides/markdown/#/6

.footnote[http://daringfireball.net/projects/markdown/syntax]


---

## pandoc is your translator

.footnote[http://johnmacfarlane.net/pandoc/diagram.png]

It will convert almost any text format into any other. Almost.

---

## knitr is a transparent engine for dynamic report generation with R

If you did something related ~5 years ago, you would have used Sweave. knitr is better and integrated with Rstudio.

.footnote[http://yihui.name/knitr/]

---

## Why?
Because of all the cool stuff you can do!

* write papers, reports, ...
* prepare your presentations
* supplement your R analyses with comments
	* Reproducible research!!
* generate .doc, .tex, .pdf, .html

One starting point, x products.

---

## Example 1: Writing a paper
Text is easy to write in markdown.

* Benefits over Word 
    * version control
        * merging conflicts
    * it's not Word
* Benefits over LaTeX
    * easier to write
    * your beautiful .tex is not going to be used anyway
    * easier collaboration
    * nicer with version control
    
### pandoc's markdown can handle your bibtex citations, too.
---

## Example 2: Presentations

* the same points apply as for text documents
* again, you get version control
* no clicking and dragging 
* no immediate visual feedback

### Gripes
* placing images can be a $@#%^ in the neck

---

## Example 3: R reports
### This is where it gets exciting.

__markdown + knitr => pdf/html__

If you work in R, this is the closest thing to fully reproducible research you will find.

.footnote[http://kieranhealy.org/files/misc/workflow-rmd-md.png]

---

## Example 4: Writing documentation
markdown makes it easy to write documentation AND serve it.

### Mkdocs

Automatic generation and push to your gh-pages branch.

My github pages: http://messersc.github.io/docs/

.footnote[https://pages.github.com/]

---
## Disadvantages

* markdown is NOT standardized
    * different flavors
	* github-flavored markdown
	* pandoc's extended markdown
	* remark.js markdown additions
	* rmarkdown
    * presentation frameworks handle new slides, images differently
    * efforts: http://commonmark.org/  
* presentations: Powerpoint/LaTeX will give you much more control, e.g. figure placing
* pandoc's md to pdf will not break code lines by default (via LaTeX, so that is expected)

---

## The End

This presentation.red[\*]  was written in markdown and rendered using remark.js

### Highly recommended reads and references:

http://kieranhealy.org/blog/archives/2014/01/23/plain-text

http://programminghistorian.org/lessons/sustainable-authorship-in-plain-text-using-pandoc-and-markdown.html

http://galahad.well.ox.ac.uk/repro/

https://nicercode.github.io/guides/reports/

http://cpsievert.github.io/slides/markdown/#/

http://zombietetris.de/blog/181/

http://remarkjs.com/#1


.footnote[.red[ * ] https://github.com/messersc/talks/blob/master/markdownintro.md]
