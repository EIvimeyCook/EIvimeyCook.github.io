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
├── Ed.JPG        # the original, full-resolution photograph
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
| 08 | Tools & apps | `#tools` | R packages and Shiny apps |
| 09 | Contact | `#contact` | Email, profile links |

## The two maps

After the record loads, one request goes to
[Crossref](https://api.crossref.org/), filtered to the DOIs the record already
holds, and returns the author list for each work.

- **Map of papers** puts one node per work and joins two works that share an author.
  Node size is the number of authors, colour is the primary theme, and works of the
  same theme attract one another so the map groups by subject. **Preprints are their
  own group** whatever their subject, and are drawn hollow. The
  button under the panel takes them out of the layout and puts them back; hidden
  nodes are skipped by the forces, the edges and the hit test alike, so the rest
  redistributes into the space instead of leaving holes.
- **Map of co-authors** puts one node per person and joins two people who appear on
  the same work. Node size is the number of works shared. **Clicking a person lists
  what the two of them wrote**, and clicking any title in that list hands off to the
  record below, which finds and flashes the row. Escape, the close button, or a
  press on the canvas dismisses the list.

## The background

The curve behind the page is a Gompertz survivorship simulation tied to scroll
position. A cohort of individuals with sampled lifespans thins out as you descend,
and l(x) is traced across the viewport. It is schematic, with arbitrary parameters,
and is not fitted to anything. The constants sit at the top of that module: `A`,
`B`, `XMAX` and `N`.

## Citation

A machine-readable [`CITATION.cff`](CITATION.cff) is included, so GitHub's
"Cite this repository" button gives formatted APA and BibTeX.

## Contact

Edward R. Ivimey-Cook, <e.ivimeycook@gmail.com>,
[ORCID 0000-0003-4910-0443](https://orcid.org/0000-0003-4910-0443)

## License

Released under the [MIT License](LICENSE.md).
