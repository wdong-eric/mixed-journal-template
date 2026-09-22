# Mixed-journal LaTeX template

An unofficial writing template for manuscripts, research notes and preprints, combining a Nature Astronomy-inspired page layout with APS/REVTeX-style headings and a bundled AAS bibliography style. It uses the standard `article` class and is not an official journal submission template.

## Files

- [`nature-astronomy-like.tex`](nature-astronomy-like.tex) — working example with title, abstract, equations, figure and table placeholders, references and end matter.
- [`natureastro-custom.sty`](natureastro-custom.sty) — page layout, typography, headings, citations and custom commands.
- [`aasjournalv7.bst`](aasjournalv7.bst) — optional BibTeX style; included but not active in the example.
- [`.gitignore`](.gitignore) — excludes generated PDFs and LaTeX build files.

## Current setup

```latex
\documentclass[10pt,a4paper,twocolumn]{article}
\usepackage[revtexheadings]{natureastro-custom}
```

- A4, two columns, 17 mm top/side margins, 18 mm bottom margin and a 5.2 mm column gap.
- Palatino-style serif text and matching mathematics through `newpxtext` and `newpxmath`. The style sets body text to 8.25 pt with 10.05 pt leading, overriding the class's nominal 10 pt size.
- A title/author/abstract block above both columns, with its contents occupying 74% of the text width. The journal masthead is disabled.
- REVTeX-inspired headings: Roman-numbered, centred uppercase sections; lettered, centred bold subsections; and Arabic-numbered, centred italic subsubsections.
- Superscript numerical citations through `natbib`, with sorting and compression within citation lists, plus coloured hyperlinks.
- Compact captions, an optional running header at the upper left and page numbers at the lower right.

To use the alternative Nature-inspired headings without displayed numbers, replace the package line with:

```latex
\usepackage[natureheadings]{natureastro-custom}
```

This is also the default when no heading option is supplied.

## Build

The local build uses pdfLaTeX from TeX Live 2024 on macOS. Keep the `.tex`, `.sty` and optional `.bst` files together. From this directory, run:

```bash
latexmk -pdf -synctex=1 -interaction=nonstopmode -halt-on-error nature-astronomy-like.tex
```

This produces `nature-astronomy-like.pdf`. Alternatively, the self-contained example can be built with two pdfLaTeX passes:

```bash
pdflatex -halt-on-error nature-astronomy-like.tex
pdflatex -halt-on-error nature-astronomy-like.tex
```

## Manuscript metadata and end matter

Set `\title{...}` and `\author{...}`, then place the `abstract` environment before `\begin{document}`, as in the example. The custom `\maketitle` prints the stored abstract with the title and authors.

- `\runninghead{...}` sets the optional short running header; it is commented out in the example.
- `\affiliations{...}` and `\correspondence{...}` store metadata for `\makeendmatter`, which is called after the references.
- `\methods`, `\dataavailability` and `\codeavailability` create unnumbered sections.
- `\acknowledgements`, `\authorcontributions` and `\competinginterests` create additional unnumbered sections.

Remove any optional sections you do not need.

## Mathematics, figures and tables

The style loads `amsmath`, `amssymb`, `bm` and `upgreek`, and defines these math-mode shortcuts:

| Command | Convention |
| --- | --- |
| `\vect{x}` | Bold italic vector via `\bm` |
| `\mat{A}` | Bold upright matrix via `\mathbf` |
| `\tens{T}` | Bold upright tensor via `\mathbf` |

Use `widetext` for equations that span both columns:

```latex
\begin{widetext}
\begin{equation}
  \vect{y} = \mat{A}\vect{x}
\end{equation}
\end{widetext}
```

This environment uses `cuted`. Use standard `figure` and `table` environments for one column, or `figure*` and `table*` for floats spanning both columns. The style already loads `graphicx`, `booktabs`, `tabularx` and `array`.

## References

The example uses a self-contained `thebibliography` block, so no `.bib` file or BibTeX run is required. Cite entries with `\cite{key}`.

To use the bundled AAS BibTeX style, create `references.bib` and replace the entire `thebibliography` block and the commented `\bibliographystyle` line with:

```latex
\bibliographystyle{aasjournalv7}
\bibliography{references}
```

Keep `\makeendmatter` after the bibliography. The `latexmk` command above handles the required BibTeX and LaTeX passes automatically.
