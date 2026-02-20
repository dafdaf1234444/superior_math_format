# Superior Math Format

Interactive viewer for Oxford undergraduate mathematics lecture notes with dependency graphs, cross-reference navigation, and MathJax rendering.

## Quick Start

**No build step. No dependencies to install. No compilation.** Each course is a single self-contained HTML file.

```bash
git clone https://github.com/dafdaf1234444/superior_math_format.git
cd superior_math_format
```

Then open `index.html` in any modern browser:

| Platform | Command |
|----------|---------|
| **Windows** | `start Part_B\Michaelmas\B2.1_Introduction_to_Representation_Theory\index.html` |
| **macOS** | `open Part_B/Michaelmas/B2.1_Introduction_to_Representation_Theory/index.html` |
| **Linux** | `xdg-open Part_B/Michaelmas/B2.1_Introduction_to_Representation_Theory/index.html` |
| **WSL** | `cmd.exe /c start "" "$(wslpath -w Part_B/Michaelmas/B2.1_Introduction_to_Representation_Theory/index.html)"` |

That's it. MathJax loads from CDN (requires internet on first load, then cached by browser).

## Available Courses

| Course | Term | Path |
|--------|------|------|
| B2.1 Introduction to Representation Theory | Michaelmas | `Part_B/Michaelmas/B2.1_Introduction_to_Representation_Theory/index.html` |

## Features

### Three-Panel Layout

```
+------------+--------------------+---------------+
|  SIDEBAR   |   LECTURE NOTES    |    CONTEXT    |
|            |                    |               |
| Contents   |  [scrollable       | Dep graph     |
| (TOC)      |   main content     | Prerequisites |
|            |   with LaTeX]      | Referenced by |
| ────────── |                    |               |
| Nomencla-  |                    |               |
| ture       |                    |               |
| (glossary) |                    |               |
+------------+--------------------+---------------+
```

- **Left sidebar (resizable)**: Split into two panels
  - **Top -- Table of Contents**: Collapsible sections, auto-expands to current scroll position, highlights active item
  - **Bottom -- Nomenclature**: All items grouped by type (Def/Thm/Lem/Prop/Cor/Ex/Rmk) with filter tabs, hover for peek preview, click to navigate
- **Center -- Lecture notes**: Scrollable content with Computer Modern Serif font, collapsible proofs
- **Right panel (resizable)**: Context for the selected item
  - Per-item dependency graph (SVG DAG, 2 levels deep)
  - Collapsible prerequisites and "referenced by" lists with expand/collapse all
  - Back/forward navigation history
  - Close button to hide panel for full-width reading

### Dependency Minimap

Always-visible overview at the top of the right panel. Every item in the document is a small colored square arranged in section rows:

- **Color = type**: Blue=Definition, Red=Theorem, Green=Lemma, Purple=Proposition, Orange=Corollary, Gray=Example/Remark
- **Scroll tracking**: Current reading position highlighted with dark border
- **Hover**: Dependencies glow blue, dependents glow red, unrelated items dim
- **Click**: Navigate to any item

### Peek Popover

Hover any cross-reference link in the main text to see a floating preview card positioned near the reference. Scrollable, stays open when you mouse into it.

### Typography

LaTeX paper aesthetic: Computer Modern Serif font, monochrome environment styling (thin gray left border, no colored boxes), booktabs-style tables (horizontal rules only).

## For AI / LLMs -- Content Conventions

If you are an AI assistant working with these files, here is the markup structure:

### HTML Structure

```html
<!-- Section heading -->
<h2 class="section-heading" id="sec1">1. Section Title</h2>

<!-- Theorem-like environment -->
<div class="env definition" id="def-1.2">
  <div class="env-head">Definition 1.2.</div>
  Content with \(inline math\) and \[display math\]
</div>

<!-- Cross-reference to another item -->
<a class="ref" data-ref="thm-1.21">Maschke's Theorem</a>

<!-- Proof (collapsible) -->
<div class="proof" id="proof-1.21">
  <div class="proof-head">Proof.</div>
  <div class="proof-body">Proof content...</div>
  <div class="qed">&#9633;</div>
</div>
```

### Environment Types

| Type | CSS Class | ID Prefix | Example ID |
|------|-----------|-----------|------------|
| Definition | `env definition` | `def-` | `def-1.2` |
| Theorem | `env theorem` | `thm-` | `thm-1.21` |
| Lemma | `env lemma` | `lem-` | `lem-3.8` |
| Proposition | `env proposition` | `prop-` | `prop-2.5` |
| Corollary | `env corollary` | `cor-` | `cor-1.24` |
| Example | `env example` | `ex-` | `ex-1.3` |
| Remark | `env remark` | `rmk-` | `rmk-1.10` |
| Notation | `env notation` | `not-` | `not-1.1` |
| Warning | `env warning` | `warn-` | `warn-X.Y` |

### ID Format

`{type-prefix}-{section}.{number}` -- e.g. `def-1.2` is Definition 1.2 in Section 1.

### Dependency Graph

The viewer auto-builds a dependency graph at page load by scanning:
1. All `<a class="ref" data-ref="target-id">` links inside each `.env` div
2. All refs inside the immediately following `.proof` div (if present)

This produces `graph[id] = { uses: [...], usedBy: [...] }` for every item.

### Adding a New Course

1. Create `Part_X/Term/Course_Name/index.html`
2. Follow the HTML conventions above
3. The inline JS will auto-build the dependency graph from the markup
4. Update the "Available Courses" table in this README

## License

Notes based on Oxford University lecture courses. Viewer code is MIT licensed.
