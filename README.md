<div align="center">
 <h1>EIvimeyCook.github.io</h1>
</div>

<!-- badges: start -->
[![Website](https://img.shields.io/badge/website-eivimeycook.github.io-brightgreen)](https://eivimeycook.github.io/)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE.md)
<!-- badges: end -->

Source for [eivimeycook.github.io](https://eivimeycook.github.io/): a single-page
academic site laid out as an instrument panel, with a publication record that keeps
itself up to date.

## Structure

```text
EIvimeyCook.github.io/
├── index.html    # the whole site: markup, styles and scripts
├── ed-1600.jpg   # photograph used in the header
├── ed-800.jpg    # smaller variant, served to narrow viewports
├── Ed.JPG        # the original, full-resolution photograph
└── README.md
```

No build step and no dependencies beyond two Google Fonts stylesheets. Editing
`index.html` and pushing is the whole workflow.

## Sections

| No. | Section | Anchor | Contents |
| --: | :------ | :----- | :------- |
| 01 | About | `#about` | Name, roles, links, photograph, biography, timeline |
| 02 | Map of papers | `#papers` | Network of works, joined by shared authorship |
| 03 | Map of co-authors | `#people` | Network of collaborators |
| 04 | Research | `#research` | The three research strands |
| 05 | Output | `#output` | Works per year, and the spread across themes |
| 06 | Publications | `#record` | The full filterable record |
| 07 | Open science | `#open` | SORTEE roles, editing, and an auto-filled paper list |
| 08 | Tools & apps | `#tools` | R packages and Shiny apps |
| 09 | Contact | `#contact` | Email, profile links, colophon |

## The publication record

`index.html` fetches the public ORCID record for
[0000-0003-4910-0443](https://orcid.org/0000-0003-4910-0443) on page load, so new
work appears without a rebuild. Corrigenda and errata are filtered out, DOIs are
resolved to links, and publication status (preprint, in review, software) is
inferred from the ORCID work type and the DOI prefix.

If ORCID cannot be reached, the page falls back to a hand-kept array in the same
script and says so beneath the record. **That array is the thing to edit when a
paper lands and ORCID is slow to catch up**: search for `FALLBACK` in `index.html`.

The **Open science** section builds its own list by filtering the record for the
`open` theme, so it needs no maintenance of its own.

### Where a preprint says it was posted

ORCID usually leaves `journal-title` empty for a preprint, so the source has to be
recovered from the DOI. **The prefix is the only part that identifies the server.**
The Center for Open Science hosts EdArXiv, MetaArXiv, PsyArXiv, SocArXiv and OSF
Preprints alike, and every one of their DOIs contains `osf.io` in the *suffix*:

```text
10.35542/osf.io/…   EdArXiv
10.31222/osf.io/…   MetaArXiv
10.31219/osf.io/…   OSF Preprints
```

An earlier version tested for the substring `osf.io` and so labelled all of them
"OSF". The table to edit is `SERVERS`, keyed by prefix and matched anchored.

That table is only a first guess. The same Crossref request that builds the maps
also asks for `institution` and `group-title`, which carry the name the server
itself deposited, and those replace the guess when they arrive — so a server the
table has never heard of still ends up correctly named, and a guess that is simply
wrong is corrected. A journal title that came from ORCID is never overwritten.
`publisher` is deliberately not consulted: it says "Center for Open Science" for
all five of the servers above.

A preprint that matches nothing and is corrected by nothing reads "Preprint"
rather than being left blank or guessed at.

**`statusOf()` recognises a preprint the same way**: by the same `SERVERS`
table, not a separate hand-picked list of prefixes. A `FALLBACK` entry for a
preprint on a server `SERVERS` already knows — Research Square, SSRN,
ChemRxiv, any of them — is detected automatically even if you forget to set
`s:"Preprint"` by hand. Add a new server to `SERVERS` and both the label and
the status pick it up together.

### DOIs

The maps look papers up by DOI, and that lookup breaks on a DOI that has
publisher URL cruft attached to it. `doiOf()` extracts the DOI once,
strips trailing `/full`, `/abstract` and the like, and everything else uses the
result — a bare trailing number is deliberately left alone, because real DOIs end
in one (`10.1086/699654`).

That is not enough for every publisher: OUP's URLs put their own article id after
the DOI (`…/doi/10.1093/evolut/qpac045/6916890`) and Nature's contain no DOI at
all. Those `FALLBACK` entries carry an explicit `d:` field, which wins over
anything parsed from the URL. **If you add a fallback entry whose URL is not a
plain `doi.org` link, check what `doiOf()` makes of it and add `d:` if it is
wrong.** Records from the live ORCID feed always carry a clean DOI and need none
of this.

## Classifying a paper

Themes come from keyword rules near the top of the record script. Search for
`RULES`.

```js
['pedagogy', ['pedagog*', 'teaching', 'taught', 'educat*', 'curricul*', 'student*', ...]],
```

Two things to know before editing:

- **Keywords match on word boundaries.** `aging` will not fire inside *engaging*
  or *managing*, and `soma` will not fire inside *somatic*. This matters: an
  earlier version matched bare substrings and quietly filed teaching papers under
  Ageing.
- **A trailing `*` marks a stem.** `educat*` matches *education* and *educational*;
  `teaching` matches only the whole word. Stems are worth preferring for anything
  with plural or adjectival forms: `meta-analy*` reaches *meta-analysis*,
  *meta-analyses*, *meta-analyst*, *meta-analysts* and *meta-analytic*, where
  listing the noun alone would miss most of them.

A work may carry several themes. A work matching nothing is tagged **Other**, which
appears as its own filter and in the distribution chart, so unclassified work is
visible rather than absorbed into a research theme. **Clicking the Other filter is
the quickest way to audit the classifier**: if it is empty, everything has landed
somewhere deliberate.

### Forcing a classification

Some titles carry no usable keyword. "Meta-analysts must lead by example" is about
open science, but says so nowhere in its title. For those, use `OVERRIDES`, just
above the rules:

```js
var OVERRIDES = [
  { match: 'lead by example', themes: ['open', 'meta'] }
];
```

`match` is tested, in lower case, against both the DOI and the title, so either a
DOI or a distinctive phrase will do. Whatever is listed replaces what the rules
decided. Reach for this rather than bending a keyword that would then misfire on
other work.

Two named exceptions also sit just below the rules: a couple of cross-species
syntheses whose titles never say "meta-analysis" are tagged as such by title.

## The two maps

After the record loads, one request goes to
[Crossref](https://api.crossref.org/), filtered to the DOIs the record already
holds, and returns the author list for each work.

- **Map of papers** puts one node per work and joins two works that share an author.
  Node size is the number of authors, colour is the primary theme, and works of the
  same theme attract one another so the map groups by subject. **Preprints are their
  own group** whatever their subject, and are drawn hollow: what has been posted but
  not published is a question about the record rather than about the research. The
  button under the panel takes them out of the layout and puts them back; hidden
  nodes are skipped by the forces, the edges and the hit test alike, so the rest
  redistributes into the space instead of leaving holes.
- **Map of co-authors** puts one node per person and joins two people who appear on
  the same work. Node size is the number of works shared. **Clicking a person lists
  what the two of them wrote**, and clicking any title in that list hands off to the
  record below, which finds and flashes the row. Escape, the close button, or a
  press on the canvas dismisses the list.

Both run a force-directed layout to rest off screen and show a loading state until
it settles, so nothing thrashes about on arrival. Nodes can be dragged.

These maps are only as complete as the author lists publishers have deposited with
Crossref. If the request fails, both panels say so and draw nothing: there is no
synthetic fallback, because invented co-authors would be worse than an empty panel.

## The background

The curve behind the page is a Gompertz survivorship simulation tied to scroll
position. A cohort of individuals with sampled lifespans thins out as you descend,
and l(x) is traced across the viewport. It is schematic, with arbitrary parameters,
and is not fitted to anything. The constants sit at the top of that module: `A`,
`B`, `XMAX` and `N`.

## Design notes

Set in [Archivo](https://fonts.google.com/specimen/Archivo) and
[DM Mono](https://fonts.google.com/specimen/DM+Mono). Dark by default; the toggle
stores an explicit choice under `eic-theme` in `localStorage`.

Colour and spacing are declared once as custom properties at the top of the
stylesheet, and the light theme redefines only those tokens. The page is a lattice
of hairline-bordered panels on one repeating grid, with corner ticks borrowed from
instrument faces.

Everything degrades: with JavaScript off the whole document is still there, without
the webfonts it falls back to a system sans and mono, and `prefers-reduced-motion`
disables every animation while leaving all content visible. Verified for horizontal
overflow from 320px to 1920px.

Text meets WCAG AA against its background in both themes. Two tokens needed
adjusting to get there and should not be lightened again: `--ink-3`, and the light
theme's `--signal`, which at its original `#d8331c` was 3.81:1 on the light ground
and so failed for the small mono text it is used for.

## Verified

The page has no framework and no test runner, so this is checked with a
headless Chromium script against mocked ORCID / Crossref payloads (the live
hosts aren't reachable from where this was built, so that mocking is the
ceiling of what could be automated — a look at the real feeds after it's
pushed is still worth doing):

- No horizontal overflow at 320, 375, 390, 768, 1024, 1280, 1600 or 1920px, in
  both themes, after visiting every section.
- Zero console or page errors through the same walk.
- `prefers-reduced-motion`: every animation off, nothing hidden that shouldn't
  be, all content still present.
- JavaScript off: the record and every section heading are still there.
- WCAG AA contrast on `--ink-3` and `--signal` against their grounds, both
  themes (see above).
- The preprint source table (`SERVERS`) checked prefix-by-prefix against a
  live example of each server — EdArXiv, MetaArXiv, PsyArXiv, SocArXiv,
  EarthArXiv, OSF Preprints, ChemRxiv, Preprints.org, Research Square, SSRN —
  rather than taken on memory.
- The DOI extractor (`doiOf`) against every URL shape actually in `FALLBACK`,
  including the ones that needed an explicit `d:` (Nature, OUP).
- Crossref returning institution/group-title, Crossref rejecting that
  `select=`, and Crossref unreachable — each falls back correctly, and the
  network panels say so honestly rather than drawing nothing silently.
- The co-author popup and the preprint toggle, clicked through in both themes.

## Citation

A machine-readable [`CITATION.cff`](CITATION.cff) is included, so GitHub's
"Cite this repository" button gives formatted APA and BibTeX.

## Contact

Edward R. Ivimey-Cook, <e.ivimeycook@gmail.com>,
[ORCID 0000-0003-4910-0443](https://orcid.org/0000-0003-4910-0443)

## License

Released under the [MIT License](LICENSE.md).
