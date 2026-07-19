# OOXML Compatibility Matrix

This matrix describes the static-preview behavior of the current implementation. “Supported” means the feature is parsed into a bounded typed model and rendered without executing document content. It does not imply pixel-identical Microsoft Office layout.

| Format | Feature | Behavior |
| --- | --- | --- |
| SpreadsheetML | Relationships, themes, shared strings, indexed/custom colors, `cellStyleXfs`/`cellXfs` | Supported |
| SpreadsheetML | Implicit row/cell references, styled blanks, hidden rows/columns, merged cells, hyperlinks | Supported |
| SpreadsheetML | 1900/1904 dates, `t="d"`, conditional/multi-section/literal/scientific/fraction/elapsed-time formats | Supported |
| SpreadsheetML | One/two-cell image anchors, rotation/flip, resize reflow, drawing order | Supported |
| SpreadsheetML | Comments, common conditional-format rules/differential styles/data bars/icon sets, table bands | Supported as bounded static formatting |
| SpreadsheetML | Cached worksheet charts | Common line/column/pie/area/scatter charts render as bounded SVG |
| SpreadsheetML | Frozen and split panes | Frozen rows/columns remain sticky; split views render up to four independently virtualized viewports |
| SpreadsheetML | Filters/sort state, data validation operator/flags, print area/titles/setup | Parsed and exposed as bounded static metadata/hints |
| SpreadsheetML | Sparklines | Numeric line/column/win-loss ranges, including bounded cross-sheet sources, render as SVG |
| SpreadsheetML | Chart legends/axis titles/data labels/stacking/combination/literal/scatter/pie series | Common cached forms render as bounded SVG approximations |
| SpreadsheetML | Formula evaluation | Typed formula/shared/array/cache provenance and cached values only; formulas are not recalculated |
| WordprocessingML | Defaults, theme colors/fonts, paragraph/character/table style inheritance | Supported |
| WordprocessingML | Ordered runs/images, hyperlinks, bookmarks, SDT, inserted/deleted content, field results/fallbacks | Supported |
| WordprocessingML | Direct/style-derived numbering, start and level overrides, common number formats | Supported |
| WordprocessingML | Table grid/width/alignment, horizontal/vertical merge, shading, vertical alignment, borders | Supported |
| WordprocessingML | Paragraph spacing/indentation/tab metadata/shading/borders, hidden runs, style-derived pagination flags | Supported as normalized static formatting |
| WordprocessingML | Table row height/header/split rules, fixed layout, table fill/indent/spacing, cell margins | Supported |
| WordprocessingML | Table pagination | Oversized non-merged tables split at row boundaries and repeat authored header rows; vertical-merge groups remain unsplit |
| WordprocessingML | Multiple formatted paragraphs inside table cells | Paragraph alignment/spacing/indentation/runs/links/fields are retained; nested tables remain a bounded fallback |
| WordprocessingML | Scoped sections, inherited first/even/default headers/footers, footnotes/endnotes/comments | Headers/footers render on authored page cards; notes/comments remain linked auxiliary content |
| WordprocessingML | Nested PAGE/NUMPAGES/SECTIONPAGES/PAGEREF fields, TOC tab leaders, conditional table styles, page/column breaks, section numbering/geometry/borders | Common page fields are recomputed from the bounded composed page map; unsupported fields use cached static results |
| WordprocessingML | Floating image anchors/wrap | Page/margin alignment, effective relative dimensions and z-order are supported; complex wrap polygons remain approximated |
| WordprocessingML | Common anchored DrawingML and VML rectangles/text boxes, page/margin alignment, fill/line/insets/z-order | Supported as layered static page objects; uncommon VML formulas/groups remain approximated |
| WordprocessingML | Word 2010 relative shape sizing/position | `wp14:sizeRelH`, `sizeRelV`, and percentage position offsets are retained through MCE and applied to page geometry |
| WordprocessingML | Paragraph formatting inside DrawingML text boxes | Style-derived runs, paragraph alignment, spacing and indentation are retained |
| WordprocessingML | Wrap polygons and richer fields | Bounded static approximation |
| WordprocessingML | Page-break-aware Letter/section-sized page cards and tracked-change author/date | Authored/style/saved hints are honored and overlapping hints are deduplicated; browser line reflow remains approximate |
| WordprocessingML | Oversized paragraph pagination | Browser line boxes split non-`keepLines` paragraphs at bounded text offsets; complex inline objects remain an unsplit fallback |
| PresentationML | Slide→layout→master→theme resolution, `clrMap` overrides, common `fmtScheme` styles and theme fonts/colors | Supported |
| PresentationML | Nested group scale/rotation/flip, shape/image flip, master→layout→slide decorations, picture fills | Parent affine transforms are applied once; general affine shear is approximated |
| PresentationML | Text paragraphs, hyperlinks, fields, tables with merge/style/borders | Supported |
| PresentationML | Speaker notes and connectors | Supported |
| PresentationML | Common cached charts, SmartArt data nodes, numeric multi-path custom geometry and responsive outlines | Bounded native SVG/static rendering; unsupported formulas fall back |
| PresentationML | Chart axes/legends/data labels/stacking/literal/scatter/pie caches, SmartArt edges/layout hints, simple geometry formulas/arcs and common preset paths | Bounded SVG approximation |
| PresentationML | Common outer/inner shadows, glow, soft edge, reflection | CSS static approximation |
| PresentationML | Affine group transforms, transition/timing/media metadata | Full affine matrix retained; behavior/media are represented statically and never executed |
| PresentationML | Animation, transition playback, embedded media playback | Not executed |
| All formats | Embedded images | PNG/JPEG/GIF/BMP/WebP with signature/dimension/pixel/animation validation; SVG is converted to a bounded inert primitive allowlist; EMF/WMF/EPS/unknown MIME use omission fallback |

## Deliberate exclusions

- Editing/saving Office content, macro execution, formula recalculation, and animation playback
- Password decryption or encrypted packages
- Fetching external document resources
- Legacy binary `.xls`, `.doc`, and `.ppt` formats

All parsers enforce ZIP/XML and format-specific semantic budgets. Content omitted by a budget is reported in the viewer instead of being processed without limits.

The delivery layer is manifest-first: worksheet cell subtrees are retained by row window, Word body subtrees by block chunk, slides and binary assets on demand, and search runs inside the persistent parser worker.
Split views share a URI-scoped worker/session; equivalent requests are coalesced and bounded by panel/session/waiter/time limits. Chunk arrivals patch only the active surface.
Search uses validated forward indexes. Worker export yields between row windows, Word chunks, and slides; Webview HTML/PDF assembly yields between bounded chunks/pages and forwards normalized progress. HTML export is script-free/self-contained with a raster MIME allowlist; PDF accepts validated JPEG data URIs and remains a bounded viewer-static representation rather than Office-identical pagination.
