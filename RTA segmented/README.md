# Reflexive Thematic Analysis — presentation package

Files:

| File | What it is |
|---|---|
| `rta_presentation.tex` | The full deck as one file. Kept for reference / final archive — **this is the one that was timing out on Overleaf**. |
| `rta_presentation.pdf` | Compiled from the file above — 18 logical slides, 57 overlay pages. |
| `preamble.tex` | Shared theme, colours, fonts, custom commands. All three parts below `\input` this. |
| `part1_arafat.tex` | Title, roadmap, and Arafat's segment. Compiles to 20 pages on its own. |
| `part2_meherab.tex` | Meherab's segment. Compiles to 18 pages on its own. |
| `part3_dipto.tex` | Dipto's segment, references, and the closing slide. Compiles to 19 pages on its own. |
| `rta_presentation_merged.pdf` | The three parts merged back into one 57-page PDF — identical output to `rta_presentation.pdf`. |
| `SCRIPT.md` | Timed speaking script with click cues for all three presenters. |

---

## Editing on Overleaf without hitting the compile-time limit

Each presenter should open and edit **their own `partN_*.tex` file** —
`preamble.tex` needs to be in the same Overleaf project folder, but you don't
open or compile it directly. Overleaf compiles whichever `.tex` file is set as
the project's main file (or whichever one you have open with "Recompile" —
on the free plan, set each person's file as the main file in turn, or use
three separate Overleaf projects sharing a copy of `preamble.tex`).

Editing `preamble.tex` — colours, fonts, the custom commands, the footer —
affects all three parts, since they all `\input` it. If one of you needs a
new color or command, add it there and tell the other two, or just retype it
locally in your own part if it's a one-off.

**Do not edit `rta_presentation.tex` and expect it to update the split
files, or vice versa.** They are two independent copies of the same content.
Treat the three `partN` files as the source of truth going forward, and only
regenerate `rta_presentation.tex` right before your final submission (see
"Merging back into one PDF" below).

## Compiling

**Each part, independently** (this is the fast one — use this while editing):

```
pdflatex part1_arafat.tex
pdflatex part1_arafat.tex        # second pass: page numbers + progress bar
```

Swap in `part2_meherab.tex` / `part3_dipto.tex` as needed. Each is a complete,
compilable document — title through references are all handled by whichever
part needs them, so you never need the other two files to see your own slides.

**The full deck in one file** (slower — only do this for a final check):

```
pdflatex rta_presentation.tex
pdflatex rta_presentation.tex
```

Run every file **twice**. The footer progress bar needs a second pass to read
the correct frame count from the `.aux` file.

### Merging the three parts back into one PDF

Once everyone is happy with their part, compile all three (twice each), then
merge the resulting PDFs — no LaTeX needed for this step:

```bash
pdfunite part1_arafat.pdf part2_meherab.pdf part3_dipto.pdf rta_presentation_merged.pdf
```

If `pdfunite` isn't available, `pdftk` or `qpdf` do the same job:

```bash
pdftk part1_arafat.pdf part2_meherab.pdf part3_dipto.pdf cat output rta_presentation_merged.pdf
# or
qpdf --empty --pages part1_arafat.pdf part2_meherab.pdf part3_dipto.pdf -- rta_presentation_merged.pdf
```

All three are free command-line tools; if none are installed, any "merge PDF"
web tool works just as well for a 57-page academic deck — there's nothing
sensitive in it.

### Why the page numbers and progress bar are not automatic across the merge

Beamer only knows about frames inside the file it's currently compiling. Left
alone, `part2_meherab.tex` would number its own slides 1 through 4 (it has
4 logical frames, not counting overlay pages), which would look wrong once
merged after Part I's slides 1–7.

To fix this, each part's preamble sets a **starting offset**:

- `part1_arafat.tex` — no offset, starts at 1 (7 logical frames total)
- `part2_meherab.tex` — `\setcounter{framenumber}{7}`, so it starts at 8
- `part3_dipto.tex` — `\setcounter{framenumber}{11}`, so it starts at 12

`preamble.tex` also defines `\decktotalframes` (currently `18`, the total
logical frames across all three parts) so the footer progress bar fills
proportionally to the *whole* deck, not just whichever part is compiling.

**If you add or remove a slide**, these numbers drift. Recount logical frames
(each `\begin{frame}` and each `\memberslide{...}` counts as one — overlay
`<->` steps within a frame do not) in the part you changed and in every part
after it, and update:

1. The `\setcounter{framenumber}{...}` line in the *next* part(s).
2. `\decktotalframes` in `preamble.tex` if the total changed.

This is the only piece of bookkeeping the split costs you. Everything else
about editing is identical to working in the single file.

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
