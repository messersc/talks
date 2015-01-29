# Clemens Messerschmidt
## How using markdown can free you from LaTex, Word, Powerpoint
## (and dropbox, if you use git with github or bitbucket)

---

# What is markdown

## markdown is a markup language. (got it?)
As are .html and .tex (actually, tex is more than that.) 

However, markdown is much simpler to write.

---
# markdown examples



---

# What is pandoc

## pandoc is a translator.

http://johnmacfarlane.net/pandoc/diagram.png

It will convert almost any text format into any other. Almost.

---

# What is knitr

## knitr is a transparent engine for dynamic report generation with R

If you did something related ~5 years ago, you would have used Sweave. knitr is better and integrated with Rstudio.

---

# Why?
Because of all the cool stuff you can do!

* write papers, reports, ...
* prepare your presentations
* supplement your R analyses with comments
* generate .doc, .tex, .pdf, .html

One starting point, x products.

---

# Example 1: Writing a paper
Text is easy to write in markdown.

Benefits over Word: 
    * version control
        * merging conflicts
    * it's not Word

Benefits over LaTeX:
    * easier to write
    * your beautiful .tex is not going to be used anyway
---

# Example 2: Presentations
    * the same point apply, however Powerpoint is superior in same cases
    * again, you get version control
    * no clicking and dragging 

---

# Example 3: R reports
## This is where it gets exiting.

__markdown + knitr => pdf/html__

If you work in R, this is the closest thing to fully reproducible research you will find.

---
# Rstudio Demo

---

# Example 4: Writing documentation
markdown makes it easy to write documentation AND serve it.

Frameworks: Mkdocs

Automatic generation and push to your gh-pages branch.

Github pages: http://messersc.github.io/docs/

---
# Disadvantages

* markdown is NOT standardized
    * different flavors, e.g. github-flavored markdown
    * presentation frameworks handle new slides, images differently
    * efforts: http://commonmark.org/  
* presentations: Powerpoint will give you much more control, e.g. figure placing

---

# End

This presentation was written in markdown and rendered using remark.js

### Highly recommended read: http://kieranhealy.org/blog/archives/2014/01/23/plain-text/
 
