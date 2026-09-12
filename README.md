# Reflexive Thematic Analysis — presentation package

Files:

| File | What it is |
|---|---|
| `rta_presentation.tex` | The Beamer source. Single self-contained file. |
| `rta_presentation.pdf` | Compiled deck — 18 logical slides, 57 overlay pages. |
| `SCRIPT.md` | Timed speaking script with click cues for all three presenters. |

---

## Compiling

```
pdflatex rta_presentation.tex
pdflatex rta_presentation.tex        # second pass: the footer progress bar
```

Run it **twice**. The progress bar in the footer needs
`\inserttotalframenumber`, which is only correct on the second pass.

### Fonts

The file detects the engine automatically:

- **pdflatex** — uses Charter (a Baskerville-like serif) for headings and
  Helvetica for body text. No extra fonts needed. This is what you should use on
  the lab computer.
- **xelatex / lualatex** — if Cormorant Garamond, Source Sans 3 and IBM Plex
  Mono are installed it uses them; otherwise it falls back silently. Use this on
  Overleaf for the exact editorial look.

Either engine produces the same layout. Nothing breaks if the fonts are missing.

### Packages used

`beamer, tikz, booktabs, array, amsmath, microtype`. All standard. The only
slightly unusual TikZ library is `overlay-beamer-styles`, which ships with both
TeX Live and Overleaf and provides the `visible on=<...>` and `alt=<...>{}{}`
keys used for the highlight-and-dim animations.

**Upload the PDF to the lab computer before class** — not the `.tex`. Losing
marks to a missing package on someone else's machine would be a waste.

---

## How the deck maps to the rubric

The rubric marks all five criteria **individually**, so the animated set pieces
are spread across the three segments rather than concentrated in one.

**Slide Quality — Technical (8 marks).** Each presenter owns at least one
non-trivial TikZ build:

- *Arafat* — the three-family spectrum, which uses TikZ's `alt` key to bring the
  active card to full opacity while the others drop to 45%; and the layered
  three-circle intersection with a conceptual equation.
- *Meherab* — the six-phase loop, revealed one node at a time with a commentary
  panel that swaps text via `\only`, then dashed curved feedback arrows for
  recursion; and the progressive transformation slide, where a transcript
  extract becomes codes, then a braced cluster, then a theme.
- *Dipto* — four continua sliders generated from a single `\foreach` loop, and a
  before-to-after transformation turning a topic summary into a real theme.

Also present: a fully custom Beamer theme (colours, frametitle rule, speaker
footer, TikZ progress bar), custom commands (`\memberslide`, `\keyidea`,
`\term`, `\hl`), a custom `pullquote` environment with oversized quotation
marks, reusable TikZ styles, `booktabs` tables, multi-column layouts, math
notation and a references frame.

**Slide Quality — General (5 marks).** One idea per slide, generous whitespace,
nothing below about 8 pt, thin borders rather than heavy boxes, and a five-colour
palette used consistently: deep blue for structure, ochre for emphasis and
progress, rose for warnings and problems, sage for good practice, plum for
themes.

**Slide Contents (5 marks).** Drawn from all four papers — Braun & Clarke 2006,
2019 and 2020, and Byrne 2022. The through-line is: what TA is → the three
variants → why reflexivity matters → the six phases → a worked example → the
design decisions → the documented mistakes → quality criteria.

**Presentation (10 marks).** This is the largest single block of marks and the
deck can't earn it for you. See the rehearsal notes at the end of `SCRIPT.md`.

**WoW Factor (2 marks).** The intended high point is the transcript-to-theme
transformation on slide 10 — one visual evolving through four stages instead of
four separate slides — backed up by the recursive six-phase loop and the
highlight-and-dim comparison.

---

## Notes on the content

The transcript extract on slide 10 is an illustrative line written in our own
words, modelled on the kind of data item in Byrne's study rather than copied
from it. Direct quotations across the deck are kept short and attributed.

If you want to switch the references frame to BibTeX/BibLaTeX for the written
report later, the four sources are already formatted in the `thebibliography`
environment at the end of the `.tex` and transfer over directly.

---

## Things you may want to adjust

- **Names and IDs** appear in three places: the title slide, the three
  `\memberslide` calls, and the closing slide. Search for the ID numbers to find
  them all.
- **Speaker footer** — the `\renewcommand{\currentspeaker}{...}` lines mark the
  start of each segment. Move one and the footer follows.
- **If you need to cut time**, the roadmap slide and the final overlay of the
  quality slide are the safest things to drop. Don't cut overlay steps out of
  the animated builds — the build is the explanation.
