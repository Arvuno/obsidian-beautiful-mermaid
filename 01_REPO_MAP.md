# 01 REPO MAP — obsidian-beautiful-mermaid

## What It Does
An **Obsidian plugin** that renders Mermaid diagrams using the `beautiful-mermaid` library, producing polished SVG output with support for Obsidian theme variables (CSS custom properties). It supports Reading view, Live Preview, a full-screen modal preview with zoom/pan, and four code block language aliases.

## Tech Stack
- **Language:** TypeScript (strict mode)
- **Bundler:** esbuild (single-file bundle → `main.js`)
- **Runtime:** Obsidian plugin API (`obsidian` v1.8.7+)
- **Editor integration:** CodeMirror 6 (`@codemirror/state`, `@codemirror/view`) for Live Preview rendering
- **Mermaid rendering:** `beautiful-mermaid` v1.1.3 (renders Mermaid diagram source to SVG)
- **Package manager:** Bun
- **Build target:** ES2018 (CJS format)

## Project Structure
```
obsidian-beautiful-mermaid/
├── main.ts              # Plugin entry point + all logic (~765 lines)
├── styles.css            # All plugin styles (~255 lines)
├── manifest.json         # Obsidian plugin manifest (version 0.2.3)
├── package.json          # NPM package definition
├── tsconfig.json         # TypeScript config (strict, ESNext, DOM lib)
├── esbuild.config.mjs    # Build script (dev watch or production bundle)
├── bun.lock              # Bun lockfile
├── versions.json         # Obsidian version manifest
├── LICENSE               # Apache-2.0
├── README.md             # User-facing docs
├── .github/
│   └── workflows/
│       └── release.yml   # CI: builds on tag, validates version alignment,
│                         #   creates GitHub release with attestation
├── assets/               # Gallery SVG previews (flow, XY charts)
└── scripts/
    └── generate-assets.ts # Generates gallery SVG assets from Mermaid source
```

## Key Files

### `main.ts` (765 lines)
- `BeautifulMermaidPlugin` — Obsidian plugin class; `onload()` registers settings tab, code block processors, and CodeMirror editor extension
- `BeautifulMermaidSettingTab` — Settings UI (languages, theme, fit-to-width, min-readable-height)
- `MermaidPreviewModal` — Full-screen modal with zoom/pan (pointer events, pinch-to-zoom)
- `MermaidEditorWidget` — CodeMirror block widget for Live Preview rendering
- `createMermaidEditorExtension` — StateField that decorates mermaid code blocks in the editor
- `fillBeautifulMermaidBlock` — Core rendering function; creates container, SVG host, action buttons (Edit, Open, Copy)
- `fitRenderedDiagram` — ResizeObserver-based diagram scaling (fit-to-width or min-readable-height)
- `renderBeautifulMermaid` — Calls `beautiful-mermaid`'s `renderMermaidSVG`
- Theme support: maps Obsidian CSS variables to beautiful-mermaid color tokens, or uses built-in themes (zinc-dark, tokyo-night, catppuccin-mocha, nord, dracula, github-light, github-dark, solarized-light, solarized-dark, one-dark)
- Custom fence detection (`findMermaidFences`) — handles triple/quad backticks and tildes
- Error handling with styled fallback block

### `styles.css` (255 lines)
- CSS custom properties for theming (`--beautiful-mermaid-*`)
- `.beautiful-mermaid-block` — main container
- `.beautiful-mermaid-scroll` — overflow container with fade mask edges
- `.beautiful-mermaid-modal` — full-screen preview modal
- Export-mode overrides (`.export-image-root`) — removes masks and actions for image export
- Print media query — resets transforms, hides actions

### `manifest.json`
- Plugin ID: `beautiful-mermaid-renderer`
- Min app version: `1.5.0`
- Version: `0.2.3`

### `.github/workflows/release.yml`
- Triggers on git tags or manual `workflow_dispatch`
- Validates: tag == manifest.json version == package.json version; versions.json entry exists
- Builds with `bun run build`
- Attests build provenance (SLSA)
- Creates/updates GitHub release with `main.js`, `manifest.json`, `styles.css`

## Build Commands
| Command | Action |
|---------|--------|
| `bun install` | Install dependencies |
| `bun run build` | Type-check + esbuild production bundle → `main.js` |
| `bun run dev` | esbuild watch mode |
| `bun run typecheck` | `tsc --noEmit --skipLibCheck` |
| `bun run assets` | Generate gallery SVG assets |

## Supported Code Block Aliases
- `mermaid`
- `mermaid-beautiful`
- `beautiful-mermaid`
- `bmmd`

## Settings
1. **Code block languages** — comma-separated custom fence languages
2. **Diagram theme** — "Obsidian colors" (CSS vars) or 13 built-in beautiful-mermaid themes
3. **Fit diagrams to width** — scale inline diagrams to fit container width
4. **Minimum readable height** — slider (160–420px), used when fit-to-width is off

## Risk-Sensitive Areas
- **SVG injection** (`appendSvg`): parses untrusted Mermaid output via `DOMParser`; validates `svgElement.nodeName === 'svg'` before appending — minimal XSS risk
- **CodeMirror editor extension**: modifies editor state via `Decoration.widget` and `Decoration.replace` — safe as it only replaces known fence blocks
- **Clipboard write** (`navigator.clipboard.writeText`): only writes pre-stripped mermaid source, not user-controlled content
- **ResizeObserver**: used for responsive diagram scaling; memory cleaned up via `onunload`
- **Pointer/pinch events** in modal: all events have `preventDefault()` with `{ passive: false }` only where needed
- **No `eval()` or dynamic content injection** beyond SVG

## Dependency Versions (from package.json)
- `obsidian`: `^1.8.7`
- `beautiful-mermaid`: `^1.1.3`
- `@codemirror/state`: `^6.5.2`
- `@codemirror/view`: `^6.38.8`
- `esbuild`: `^0.24.0`
- `typescript`: `^5.7.0`
- `@types/node`: `^20.11.0`
