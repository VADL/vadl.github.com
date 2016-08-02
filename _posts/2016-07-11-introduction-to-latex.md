---
title: Introduction to LaTeX
date: '2016-07-11 09:18:00'
layout: post
---
## What is LaTeX?

LaTeX is a tool for writing text without specifying how the text
should be laid out on a page and formatted. Normally, when text is
entered into a word processing document and saved, the layout of the
text, the style of the text, and the formatting of any tables,
figures, headings, etc. are all stored with the text and are
inseparable from the text itself.

So where is the formatting stored? The anser is the style file
(`.sty`) and its associated class files. These files are text
independent (i.e. they contain no text themselves that will wind up in
the final document. By swapping out the style/class files, you can
take the same LaTeX files and render a completely differently looking
document (e.g. presentation, book chapter, two column paper, thesis,
etc.)

Since you may want to re-use text between documents (and
figures/tables), you do not want to tightly couple the formatting of
the text with the text itself. Additionally (and most importantly) the
formatting and layout are not touched at all when editing the text, so
users are free to write and let the style file take care of all the
formatting.

Finally, LaTeX is also useful because you can include one file inside
other files. This allows you to break each section or chapter of a
long document up into separate files. This feature becomes especially
useful when collaborating with others on the same document since you
can more easily handle simultaneous working (since generally two
people will not be writing in the same text at the same time). Again,
even if there is a conflict and two people need to merge their text,
the text is _all_ they need to merge; there will be no formatting
conflicts at all.

## How to install LaTeX

### Windows

### Linux

### OSX

## How to use LaTeX

### Writing LaTeX

#### Converting from Markdown into LaTeX

### Compiling LaTeX into PDF

### Installing Packages

### How to include references

## Going from here

## Further references
