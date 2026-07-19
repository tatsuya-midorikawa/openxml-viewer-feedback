# OpenXML Viewer

A Visual Studio Code extension that lets you **preview Office Open XML files (`.xlsx` / `.docx` / `.pptx`) directly inside Visual Studio Code**.
Just double-click a file to quickly inspect its contents—without launching Excel, Word, or PowerPoint.

> [!NOTE]
> This extension is intended for **viewing (read-only)**. It does not provide editing or saving capabilities.

---

## Table of Contents

- [OpenXML Viewer](#openxml-viewer)
  - [Table of Contents](#table-of-contents)
  - [Key Features](#key-features)
  - [Supported File Formats](#supported-file-formats)
  - [Requirements](#requirements)
  - [Installation](#installation)
    - [From the Visual Studio Code Marketplace](#from-the-visual-studio-code-marketplace)
    - [From a VSIX File](#from-a-vsix-file)
  - [Usage](#usage)
  - [Commands](#commands)
  - [Settings](#settings)
  - [Limitations](#limitations)
  - [Roadmap](#roadmap)
  - [Contributing](#contributing)
  - [Sponsors](#sponsors)
  - [License](#license)

---

## Key Features

- 📊 **View Excel workbooks (`.xlsx`)**
  - Display cell contents in a table, sheet by sheet
  - Switch between multiple sheets with tabs
  - Display merged cells and virtualize rows/columns for responsive scrolling
  - Preserve hidden/styled cells, hyperlinks, themes and style inheritance, ISO dates, scientific/fraction/elapsed-time and conditional number formats
  - Show comments, common conditional formatting/data bars/icon sets, table banding, and cached worksheet charts
  - Keep frozen rows/columns visible, draw numeric line/column/win-loss sparklines, and render cached combination-chart series
  - Reflow two-cell anchored images after row/column resizing while preserving drawing order
  - Resize column widths and row heights (drag borders, double-click to auto-fit, reset per sheet); adjustments persist across reopen
- 📝 **View Word documents (`.docx`)**
  - Display paragraphs, inherited text styles, inline images, headings, and tables
  - Preserve hyperlinks, bookmarks, content controls, tracked insertions/deletions, fields, numbering overrides, table layout/borders/shading/merges, and page sections
  - Render complex field results/fallbacks, page/column breaks, and multi-column section hints
  - Approximate pages around explicit page breaks and display tracked-change author/date metadata
  - Show headers, footers, footnotes, endnotes, and comments as linked auxiliary content
- 📑 **View PowerPoint presentations (`.pptx`)**
  - Display text, shapes, and images for each slide
  - Apply per-master themes, nested group rotation/flip transforms, shape/image flips, table merges/styles/borders, fields, and speaker notes
  - Render connectors, cached charts, SmartArt nodes/edges, numeric/formula custom geometry paths, and common effects as bounded SVG/static content
  - Navigate with the slide list or compact previous/next controls
- 🔍 **In-viewer text search**
  - Run a full-text search over the displayed content and highlight matches
  - Jump to the previous/next match (moving across sheets and slides)
- ♿ **Accessible, localized controls**
  - English/Japanese UI, a roving ARIA grid with selection state, modal focus management, keyboard navigation/resizing, focus restoration, high-contrast and reduced-motion support
  - Copy focused cells, paragraphs, and slide text with Ctrl/Cmd+C
- ⚡ **Manifest-first loading**
  - Request worksheet row windows, document chunks, slides, searches, and binary assets from a persistent parser worker only when needed
  - Share one URI-scoped parser worker and bounded caches across split views; file changes invalidate the shared generation once
  - Coalesce equivalent split-view requests, reference-count cancellation, and enforce panel/session/waiter/time limits
  - Patch arriving row windows/document chunks/active slides without rebuilding the whole Webview
- 📤 **Static export**
  - Export a sanitized self-contained HTML snapshot with no scripts or external resource fetches
  - Assemble sheets, document chunks, and slides cooperatively with cancellation/progress, then export a bounded PDF (up to 100 pages / 100 MiB of validated JPEG data)
- 🩺 **Local diagnostics**
  - Inspect broker/Worker/Webview cache, index, DOM, Blob, and pending-request counts in an Output channel
  - Diagnostics stay local, contain no document text, and send no telemetry
- 🖱️ **Seamless operation**
  - Launch the preview simply by opening a file in the Explorer (custom editor)
  - No need to launch external applications
- 🔒 **Safe viewing**
  - Read-only, so accidental edits cannot corrupt the file
  - The Webview's Content Security Policy (CSP) restricts loading external resources
  - Supports Restricted Mode and virtual workspaces; files are read only through the VS Code file-system API and no external document resources are fetched

---

## Supported File Formats

| Type | Extension | Format | Status |
| --- | --- | --- | --- |
| Excel workbook | `.xlsx` | SpreadsheetML | ✅ View |
| Word document | `.docx` | WordprocessingML | ✅ View |
| PowerPoint | `.pptx` | PresentationML | ✅ View |

> [!IMPORTANT]
> The target is **Office Open XML (OOXML)** files.
> Legacy binary formats (`.xls` / `.doc` / `.ppt`) and macro-enabled formats (such as `.xlsm`) are not supported.

See [OOXML Compatibility Matrix](./OOXML-COMPATIBILITY.md) for feature-level static rendering and fallback behavior.

---

## Requirements

- Visual Studio Code `1.90.0` or later
- No additional runtime or external application (such as Microsoft Office) is **required**

---

## Installation

### From the Visual Studio Code Marketplace

1. Open the **Extensions** view from the VS Code sidebar (`Ctrl+Shift+X` / `⌘+Shift+X`)
2. Search for `OpenXML Viewer`
3. Click **Install**

### From a VSIX File

```bash
code --install-extension openxml-viewer-<version>.vsix
```

Alternatively, open the Command Palette (`Ctrl+Shift+P` / `⌘+Shift+P`), run
**"Extensions: Install from VSIX..."**, and select the `.vsix` file.

---

## Usage

1. Open a `.xlsx` / `.docx` / `.pptx` file in the VS Code Explorer.
2. This extension's custom editor launches automatically and shows a preview of the contents.
3. If you want to open the file in the default text editor or another editor, right-click the file and switch the way it opens via
   **"Open With..."**.

> [!TIP]
> If you want to change the default way a file opens, you can set the default editor per extension via
> **"Open With..." → "Configure default editor for '*.xlsx'..."**.

Type a keyword into the search bar at the top of the preview to run a full-text search over the displayed document.
Press `Enter` / `Shift+Enter` to move to the next / previous match, and `Esc` to clear the search.
For spreadsheets and presentations, the viewer automatically switches to the matching sheet / slide.

---

## Commands

These are available from the Command Palette (`Ctrl+Shift+P` / `⌘+Shift+P`).

| Command | Command ID | Description |
| --- | --- | --- |
| OpenXML Viewer: Open Preview | `openxml-viewer.openPreview` | Open the active file in the preview |
| OpenXML Viewer: Reload | `openxml-viewer.reload` | Reload the current preview |
| OpenXML Viewer: Export Self-Contained HTML | `openxml-viewer.exportHtml` | Export bounded static content as script-free HTML |
| OpenXML Viewer: Export Static PDF | `openxml-viewer.exportPdf` | Export bounded static viewer pages as PDF |
| OpenXML Viewer: Show Keyboard Shortcuts | `openxml-viewer.showShortcutHelp` | Show in-viewer keyboard help |
| OpenXML Viewer: Show Local Diagnostics | `openxml-viewer.showDiagnostics` | Show count-only local performance/resource diagnostics |

---

## Settings

You can configure these in `settings.json` (user settings / workspace settings).

| Setting Key | Type | Default | Description |
| --- | --- | --- | --- |
| `openxmlViewer.spreadsheet.maxRows` | `integer` | `1000` | Maximum spreadsheet rows parsed and virtualized (`1`–`10000`) |
| `openxmlViewer.spreadsheet.showGridlines` | `boolean` | `true` | Whether to show cell gridlines |
| `openxmlViewer.document.showImages` | `boolean` | `true` | Whether to parse and show images in Word documents |
| `openxmlViewer.presentation.thumbnails` | `boolean` | `true` | Whether to show the slide thumbnail list |
| `openxmlViewer.theme` | `string` | `"auto"` | Preview color scheme (`auto` / `light` / `dark`) |

Example configuration:

```jsonc
{
  "openxmlViewer.spreadsheet.maxRows": 5000,
  "openxmlViewer.theme": "dark"
}
```

---

## Limitations

> [!WARNING]
> This extension is intended to provide a **simple preview**. It does not guarantee rendering that is fully compatible with Excel, Word, or PowerPoint.
> The layout, formatting, colors, and so on may differ from how the file appears in the original Office application.

- Editing or saving Office content is not supported (view-only). HTML/PDF export creates a static viewer representation and does not modify the source.
- Charts, SmartArt, custom geometry, conditional formatting, and other advanced objects may use a simplified static fallback rather than Office-identical rendering.
- Cached chart series (including common combination charts), legends/axes/data labels, simple stacked charts, SmartArt data edges, and common custom-geometry formulas/arcs are rendered; complex effects/layout rules and exact Word pagination remain approximations.
- Frozen spreadsheet panes and numeric line/column/win-loss sparklines are rendered. Split panes, filters, data validation, print metadata, Word first/even/default section references, anchored-object metadata, text boxes, and richer field fallbacks remain static hints/approximations.
- PDF output is intentionally viewer-static and is not guaranteed to match Microsoft Office pagination or fonts.
- Hyperlinks are opened only after user interaction and only for `http`, `https`, or `mailto`; embedded external resources are never fetched by the preview.
- Embedded images are limited to safe raster MIME types (PNG, JPEG, GIF, BMP, WebP); SVG/vector/unknown image payloads are not expanded or embedded in exports.
- Raster signatures and decoded dimensions/pixel counts are validated, while ZIP, asset, search, and warm-session caches are byte bounded.
- Password-protected or encrypted files cannot be opened.
- Content beyond the parser's safety budgets (rows, blocks, slides, objects, or table cells) is omitted with an in-viewer notice.
- Formulas display their cached result values (no recalculation is performed).
- Legacy binary formats (`.xls` / `.doc` / `.ppt`) are not supported.

---

## Roadmap

- [x] `.xlsx` viewer (cells, multiple sheets)
- [x] `.docx` viewer (body, tables, images)
- [x] `.pptx` viewer (slides, thumbnails)
- [x] In-viewer text search (match highlighting, previous/next navigation)
- [x] Resizable spreadsheet column widths and row heights (drag, auto-fit, persistence)
- [x] Keyboard text/cell copy support
- [x] Bounded cached chart, SmartArt, connector, and custom-geometry rendering
- [x] Shared parser sessions and incremental lazy rendering
- [x] Sanitized self-contained HTML and bounded static PDF export
- [x] Shared request QoS, cooperative search/export, count-only local diagnostics, and reproducible release metadata
- [x] Clean-checkout builds, Webview epochs/Worker recovery, full-vs-lazy parity, byte/pixel budgets, internal workbook/slide navigation, and installed-VSIX validation

---

## Sponsors

If you would like to support the development of this project, contributions via [GitHub Sponsors](https://github.com/sponsors/tatsuya-midorikawa) are welcome.
Your support will be used to improve features and maintain the project over time.
