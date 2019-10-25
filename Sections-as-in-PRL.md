## Sections as in PRL

To section a document as in _Physical Review Letters_ you can add this snippet in the preamble of your REVTeX 4.x document. Remember to load the `prl` option.

```latex
\makeatletter
\renewcommand{\paragraph}{%
  \@startsection{paragraph}{4}%
  {\z@}{1ex}{-1em}%
  {\normalfont\normalsize}{}%
}%
\renewcommand{\section}[1]{\paragraph{\itshape#1.---}}
\makeatother
```

And then use the standard sectioning commands

```latex
\section{Introduction}%
Lorem ipsum dolor sit amet...
```

To get

_Introduction._—Lorem ipsum dolor sit amet...

**Note:** The `%` symbol is important to avoid an unwanted extra space between the emdash and the first letter of the section.
