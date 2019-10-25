## Improved version of the BibTeX style file `iopart-num.bst`

This BibTeX style file adds support to arXiv and DOI links to the very good bst file by [Mark Caprio](https://github.com/mark-caprio). [[github](https://github.com/mark-caprio/iopart-num)] [[ctan](https://www.ctan.org/tex-archive/biblio/bibtex/contrib/iopart-num)]

There exists another (deprecated) improvement by [Leo C. Stein](https://github.com/duetosymmetry/iopart-num.bst) but this version is closer to the IOP journals formatting style.

### Differences

- Added entries `doi`, `volumetitle`, `transjournal`, `transsection`, `transvolume`, `transnumber`, `transissue`, `transpages`, `transyear`.

- Added the `doilink` function

```
FUNCTION {doilink}
{ duplicate$ empty$
{ pop$ "" }
{ doi empty$
    { skip$ }
    { "\href{https://doi.org/" doi * "}{" * swap$ * "}" * }
  if$
}
if$
}
```

- Modified the `format.eprint` function

```
FUNCTION {format.eprint}
{ eprint duplicate$ empty$
    'skip$
    { 
      doi empty$
    { "{\em Preprint} " }
    { "({\em Preprint} "}
  if$
      swap$
       "\eprint"
      archive empty$
        'skip$
        { "[" * archive * "]" * }
      if$
      "{" * swap$ * "}" *
      *
      doi empty$
    { "" }
    { ")"}
  if$
      *
    }
  if$
}
```

- Modified the `format.bvolume` function

```
FUNCTION {format.bvolume}
{ volume empty$
    % no volume: return blank
    { "" }
    {
      series "series" bibinfo.check
      empty$ 
        % no series: must be multivolume book
        {
          bbl.volume volume tie.or.space.prefix "volume" bibinfo.check * *
          volumetitle empty$
            'skip$
            % volumetitle: book volume has title
            { " " volumetitle "volumetitle" bibinfo.check emphasize * * }
          if$
        }
        % series: format as volume in series
        { 
          "(" 
          series "series" bibinfo.check emphasize *
          " " * bbl.volume * 
          volume tie.or.space.prefix "volume" bibinfo.check * 
          ")" * *
        }
      if$
      "volume and number" number either.or.check
    }
  if$
}
```

- Added `doilink` at the end of the `format.vol.num.pages` function

- Added the line `format.title "title" output.check` in the `incollection` and `inproceedings` functions

- Changed the line `"\providecommand{\eprint}[2][]{\url{#2}}"` into `"\providecommand{\eprint}[2][]{\href{http://arxiv.org/abs/#2}{\ttfamily #2}}"` in the `begin.bib` function
