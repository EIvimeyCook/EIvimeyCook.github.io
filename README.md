<div align="center">
 <h1>EIvimeyCook.github.io</h1>
</div>

<!-- badges: start -->
[![Website](https://img.shields.io/badge/website-eivimeycook.github.io-brightgreen)](https://eivimeycook.github.io/)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE.md)
<!-- badges: end -->

Source for [eivimeycook.github.io](https://eivimeycook.github.io/): a single-page
academic site.

## Structure

```text
EIvimeyCook.github.io/
├── index.html    # the whole site: markup, styles and scripts
├── ed-1600.jpg   # photograph used in the header
├── ed-800.jpg    # smaller variant, served to narrow viewports
├── CITATION.cff  # citation metadata for GitHub's "Cite this repository"
├── LICENSE.md    # MIT licence
└── README.md
```

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
| 08 | Tools & apps | `#tools` | Shiny and web apps |
| 09 | Contact | `#contact` | Email, profile links |

## The two maps

After the record loads, one request goes to
[Crossref](https://api.crossref.org/), filtered to the DOIs the record already
holds, and returns the author list for each work.

- **Map of papers** puts one node per work and joins two works that share at least one co-author besides me.
  Node size is the number of authors, colour is the primary theme, and works of the
  same theme attract one another so the map groups by subject. **Preprints are their
  own group** whatever their subject, and are drawn hollow. The
  button under the panel takes them out of the layout and puts them back; hidden
  nodes are skipped by the forces, the edges and the hit test alike, so the rest
  redistributes into the space instead of leaving holes.
- **Map of co-authors** puts one node per person and joins two people who appear on
  the same work. Node size is the number of works shared. **Clicking a person lists
  what we've written together**, and clicking any title in that list hands off to the
  Publications list below, which finds and flashes the row. Escape, the close button, or a
  press on the canvas dismisses the list.

## The background

The curve behind the page is a survivorship simulation tied to scroll position — one of five textbook mortality models, picked at random each time the page loads: Gompertz, Gompertz–Makeham, Weibull, Exponential, and Siler. A cohort of individuals, their lifespans sampled from whichever model was picked, thins out as you descend, and l(x) is traced across the viewport. The colophon at the foot of the page names and describes the model this load landed on. Each model is also rescaled at load time so its own "95% dead" point lands at the foot of the page — otherwise the Gompertz-shaped models finish early and Exponential barely starts by the time you've actually scrolled to the bottom. 

## Citation

A machine-readable [`CITATION.cff`](CITATION.cff) is included, so GitHub's
"Cite this repository" button gives formatted APA and BibTeX.

## Contact

Edward R. Ivimey-Cook, <e.ivimeycook@gmail.com>,
[ORCID 0000-0003-4910-0443](https://orcid.org/0000-0003-4910-0443)

## License

Released under the [MIT License](LICENSE.md).
