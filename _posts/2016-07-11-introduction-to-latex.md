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

Additionally, certain things like references (to figures, sections,
etc.), figure/table numbers, and citations are simpler and easier to
manage (i.e. they don't ever change and you can be certain they're
up-to-date).

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

Writing LaTeX is for the most part the same as writing regular text
because that is all it is. Where things change a little bit is when
you want to enter mathematical equations, references, or citations.

#### Converting from Markdown into LaTeX

### Compiling LaTeX into PDF

### Installing Packages

### How to include references

Anywhere you have a `\label{label_name}`, e.g.

```latex
\label{simple_label}
\label{sec:fluid_dynamics}
\label{fig:fluid_flow}
\label{table:experimental_results}
```

in any of your documents, you can easily reference them using that
same label, e.g.

```latex
Object \ref{simple_label}
Section \ref{sec:fluid_dynamics}
Figure \ref{fig:fluid_flow}
Table \ref{table:experimental_results}
```

### How to include citations

Assuming you have a bibliography file (`.bib`), that has references of
the form:

```latex
@article{fractionated_spacecraft,
	tite={Assessing the fractionated spacecraft concept},
	author={Mathieu, Charlotte and Wiegel, AL},
	journal={AIAA Paper},
	volume={7212},
	year={2006}
}
@book{fundamentals_astrodynamics,
	title={Fundamentals of Astrodynamics},
	author={Bate, R.R. and Mueller, D.D. and White, J.E.},
	isbn={9780486600611},
	lccn={73157430},
	series={Dover Books on Aeronautical Engineering Series},
	year={1971},
	publisher={Dover Publications}
}
```

then you can cite them anywhere in your documents as:

```latex
\cite{fundamentals_astrodynamics} talks about the fundamentals of
astrodynamics. More recently, \cite{fractionated_spacecraft} discussed
and analyzed the concepts of fractionated spacecraft flying in
formation.
```

## Going from here

## Further references

* [LaTeX Wiki 1](http://en.wikibooks.org/wiki/LaTeX)
* [LaTeX Wiki 2](http://latex.wikia.com/wiki/Main_Page)
