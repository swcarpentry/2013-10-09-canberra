# Literate programming in R

It's easy to generate reports dynamically in R. The paradigm come from Donald Knuth (Knuth, 1984).

**Basic idea:** Write **data** + **software** + **documentation** (or in this case manuscripts, reports) together.

Analysis code is divided into text and code "chunks".

This allows us to extract the code for machine readable documents (technically called `tangle`) or produce a human-readable document (called `weave`).

Literate programming involves with three main steps:  

1. Parse the source document and separate code from narratives.
2. Execute source code and return results.
3. Mix results from the source code with the original narratives.

## Why this is important?
Results from scientific research have to be reproducible to be trustworthy. We do not want a finding to be merely due to an isolated occurrence, e.g. only one specific lab can produce the results on one specific day, but no one else can.

Reproducible research (RR) is one possible by-product of dynamic report generation, but it does not absolutely guarantee RR. 

## Installing Knitr

```coffee
install.packages("knitr", dependencies = TRUE)
install.packages("pander") # useful for formatting tables.
```

Knitr supports a variety of documentation formats including markdown, `html` and `LaTeX`. Exports to `PDF` and `HTML`.

## What is markdown?

An incredibly simple semantic file format, not too dissimilar from .doc, .rtf or .txt. Markdown makes it easy for even those without a web-publishing background to write any sort of text (including with links, lists, bullets, etc.) and have it displayed in a variety of formats. 

* [Markdown cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
* [Original markdown reference](http://daringfireball.net/projects/markdown/basics)


## When to implement reproducibility via literate programming?

* Anytime but easiest to do at the beginning of a project.
* Best used alongside version control (keeps track of everything as * you go along)
* Use software like R where instructions are coded (no GUIs unless they generate code)
* Don't save the output (only raw dataset with pre-processing code. Don't save the cleaned datasets except for temporary use).
and save the data in a non-proporietary format (e.g. `csv` over `xls`).


---

**Good and Bad Practices**

See best practices document in the 01-R-basics folder.


## Creating a basic knitr document

In RStudio, choose new R Markdown file (easiest way)
or create a new file and save it with extension `.Rmd`.

A basic code chunk

<pre><code>
```{r}
# some R code
```
</code></pre>
---

Either knit using the knit button or do it programmatically.

```
library(knitr)
knit("file.Rmd")
```

**What just happened?**

knitr reads the Rmd file, finds and runs the code chunks identified by the backticks, and replaces it with the output of the functions. 

Add names to code chunk

e.g.
<pre><code>
```{r some_name}
```
</code></pre>

Do not edit the output files because they are automatically generated. When reprocessing the original document all changes will be lost.

**Other options you can add to the tag**

**echo** = : TRUE or FALSE to show or hide code.  
**eval** = : TRUE or FALSE to run or skip the code.  
**warning** = : TRUE or FALSE to show or hide function warnings.  
**message** = :TRUE or FALSE to show or hide function messages.  
**results** = "hide": will hide results.  
**fig.height**: Height of figure  
**fig.width**: width of figure  


**Write sentences in text with inline output**

<pre><code>
Include some text `r mean(1:5)`. 
</code></pre>

**Summarizing output from models.**

<pre><code>
```{r fit_model}
library(datasets)
data(airquality)
fit = lm(Ozone ~ Wind + Temp + Solar.R, data = airquality)
```


```{r showtable, results="asis", echo = FALSE, message = FALSE, warning = FALSE}
library(pandoc)
pander(fit)
```
</code></pre>

## Global options

Global options are shared across all the following chunks after the location in which the options are set, and local options in the chunk header can override global options.

<pre><code>
```{r setoptions, echo = FALSE}
options(width = 60, show.signif.stars = FALSE)
opts_chunk$set(echo = FALSE, 
            results = "asis", 
            warning = FALSE, 
            message = FALSE, 
            fig.width = 5, 
            fig.height = 4, 
            tidy = TRUE, 
            fig.align='center')
```</code></pre>


## Other Options
**Deailing with long running process**

add `cache=TRUE` to a code block definition. After the first run, results are cached. We'll discuss better ways to acheive the same thing using `Make` in the next section.


## Quick reporting

If a user only has basic knowledge about R but knows nothing about knitr, or one does not want to write anything else other than an R script, it is also possible to generate a quick report from this R script using the `stitch()` function.

The basic idea of `stitch()` is that knitr provides a template of the source document with some default settings, so that the user only needs to feed this template with an R script (as one code chunk), then knitr will compile the template to a report. Currently it has built-in templates for LATEX, HTML and Markdown. The usage is like this:

```coffee
library(knitr) 
stitch("your-script.R")
```
## Additional chunk options

As mentioned in Chapter 3, we can write chunk options in the chunk header. The syntax for chunk options is almost exactly the same as the syntax for function arguments in R. 

```
{r chunk_mame, eval=if (value < 5) TRUE else FALSE}
```

Suppose value is a numeric variable created in the source document before this chunk.We can pass an expression if (value < 5) TRUE else FALSE to the option eval, which makes the option eval depends on the value of value, and the consequence is we evaluate this chunk based on the value of value (if it is greater than 5, the chunk will not be evaluated), i.e. we are able to selectively evaluate certain chunks. 


## Chunk Label
The only possible exception is the chunk label, which does not have to follow the syntax rule. In other words, it can be invalid R code. This is for both historical reasons (Sweave convention) and laziness (avoid typing quotes). Strictly speaking, the chunk label, as a part of chunk options, should take a character value, hence it should be quoted, but in most cases, knitr can take care of the unquoted labels and quote them internally, even if the “objects” used in the label expression do not exist. Below are all valid ways to write chunk labels:

```
{r, label_name}
```

Chunk labels are supposed to be unique id’s in a document.  
knitr will throw an error if two chunks have the same name.  
If no chunk names are given, knitr will simply increment from chunk 1, 2,3 etc.



## Error handling

It may be very surprising to knitr users that knitr does not stop on errors! As we can see from the previous example, 1 + ’a’ should have stopped R because that is not a valid addition operation in R (a number + a string). The default behavior of knitr is to act as if the code were pasted into an R console: if you paste 1 + ’a’ to the R console, you will see an error message, but that does not halt R – you can continue to type or paste more code. To completely stop knitr when errors occur, set this option in advance:

```coffee
opts_knit$set(stop_on_error = 2L)
```


The meaning of the integer code for stop on error is as follows
(from the evaluate package; see the documentation for evaluate() there):
* **0L** do not stop on errors, just like the code was pasted into R console
* **1L** when an error occurs, return the results up to this point and ignore the rest of code in the chunk but do not throw the error either
* **2L** a full stop on errors


## Working with graphics in knitr

```coffee
library(ggplot2)
p <- qplot(carat, price, data = diamonds) + geom_hex()
p 
# no need to print(p)
```

---

## Code Externalization
It can be more convenient to write R code chunks in a separate R script, rather than mixing them into a source document; Using the format:


```
## @knitr chunk_label

```

Be sure to leave a blank line between chunks.


In the source document, we can first read the script using the func- tion read chunk():

```
read_chunk("shared_code.R")
```


This is usually done in an early chunk such as the first chunk of a document, and we can use the chunk Q1 later in the source document:


<pre></code>```
{r, chunk_name}
```
</code></pre>

## Pandoc
Pandoc (http://johnmacfarlane.net/pandoc) is a universal document converter. In particular, Pandoc can convert Markdown to many other document formats, including LATEX, HTML, Rich Text Format (*.rtf), E- Book (*.epub), Microsoft Word (*.docx) and OpenDocument Text (*.odt), etc.
Pandoc is a command line tool. Linux users should be fine with it; for Windows users, the command window can be accessed via the Start menu, then Run cmd. Once we have opened a command window (or terminal), we can type commands like this to convert a Markdown file, say, test.md, to other formats:

```
pandoc test.md -o test.html
pandoc test.md -o test.odt
pandoc test.md -o test.rtf
pandoc test.md -o test.docx
pandoc test.md -o test.pdf
```

Add margins using (in this case 1 inch):

```
pandoc -V geometry:margin=1in test.md -o test.pdf
```

