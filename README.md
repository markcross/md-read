# md·read — Local Markdown Reader
https://github.com/markcross/md-read

A single self-contained HTML file for reading Markdown documents locally in any browser. No installation, no server, no dependencies beyond a CDN connection on first load.

---

## Getting Started

Save `markdown-reader.html` anywhere on your machine and open it in a browser. That's it.

**CDN note:** On first load the page fetches [marked.js](https://cdnjs.cloudflare.com/ajax/libs/marked/9.1.6/marked.min.js) and [highlight.js](https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/highlight.min.js) from cdnjs. Both are then cached by the browser, so subsequent loads work offline.

---

## Opening a Document

There are five ways to load a Markdown file:

- **Click the drop zone** — the dashed bar in the header opens a file picker. Accepts `.md`, `.markdown`, and `.txt` files.
- **Drag and drop** — drag a file from your file manager onto anywhere in the window.
- **Keyboard shortcut** — `Ctrl+O` (or `Cmd+O` on Mac) opens the file picker directly.
- **Paste** — click the **Paste** button to read raw Markdown from the clipboard. If clipboard access is blocked, a prompt dialog appears as fallback.
- **Recent files** — hover over **md·read** in the top-left corner to reveal a dropdown of the last 12 uniquely viewed files. See [Recent Files](#recent-files) below.

The filename is displayed in the header after a file is loaded. The drop zone shrinks to accommodate long filenames.

---

## Navigation

### Scrolling

| Control | Action |
|---|---|
| `↑` button | Scroll up one page (~95% of the visible height) |
| `↓` button | Scroll down one page (~95% of the visible height) |
| `PgUp` key | Same as ↑ button |
| `PgDn` key | Same as ↓ button |

### Table of Contents

Click **TOC** in the toolbar to open a sidebar listing all headings (`h1`–`h4`) found in the document. The TOC only appears when the document contains two or more headings.

- Click any entry to jump to that heading, placing it at the **top of the viewport**.
- The active heading is highlighted as you scroll.
- TOC open/closed state is remembered between sessions.

### Document Map

A live miniature preview of the entire document is shown in the right-hand panel — similar to the Document Map in Notepad++.

- **Blue viewport indicator** shows your current reading position proportionally.
- **Click anywhere** on the map to jump instantly to that position.
- **Click and drag** to scrub freely through the document.
- The map re-renders automatically after loading a file and on window resize.
- Headings appear as bright bars, body text as faint line stubs, code blocks with a green edge, and blockquotes with a blue left rule.

### Reading Timer

A `HH:MM:SS` elapsed timer sits at the right end of the toolbar and starts automatically when a document is opened. The timer resets whenever a new document is loaded.

| Interaction | Action |
|---|---|
| Single click | Pause the clock (turns amber with a ⏸ indicator) |
| Single click (while paused) | Resume the clock |
| Double-click | Reset to `00:00:00` and restart |

---

## Recent Files

Hover over the **md·read** logo in the top-left corner to reveal a dropdown list of the last 12 uniquely opened files, showing the full filename.

- **Click any entry** — copies the filename to the clipboard and shows a brief toast notification. You can then paste it into the file picker's filename field to navigate to it. Note: browsers do not expose the full file path when opening local files — only the filename is available.
- The scroll position for each file is tracked individually. When you reopen a file via the file picker, the reader restores your last position in that file automatically.
- **Clear history** — a button at the bottom of the dropdown removes all recent entries.
- Recent file history is stored in `localStorage` and persists across sessions.

---

## Display Settings

All display settings are saved to `localStorage` and restored automatically on next open.

### Font Size & Line Spacing

| Control | Action |
|---|---|
| `A−` button | Decrease font size by 1px (minimum 12px) |
| `A+` button | Increase font size by 1px (maximum 28px) |
| `Ctrl+−` | Same as A− |
| `Ctrl+=` | Same as A+ |

Line height adjusts proportionally with font size (range 1.3–2.2).

### Column Width

| Control | Action |
|---|---|
| `←` button | Narrow the text column by 80px |
| `→` button | Widen the text column by 80px |
| `Ctrl+←` | Same as ← button |
| `Ctrl+→` | Same as → button |

The text column remains centred. Range is 400px–1400px (default 720px).

### Font Face

A dropdown selector in the toolbar offers nine font choices:

**Serif** — Charter *(default)*, Georgia, Palatino  
**Sans-serif** — IBM Plex Sans, Inter, System UI  
**Monospace** — JetBrains Mono, Fira Code, Courier New

All fonts are system fonts — no web fonts are loaded. If a font is not installed, the browser falls back gracefully to the next in the stack.

### Colours

Two colour swatches sit in the toolbar:

- **BG** — click to open the native colour picker for the page background.
- **FG** — click to open the native colour picker for the body text.

The surface and border colours are automatically derived from the background colour to keep the chrome coherent across light and dark themes.

**⇅ Flip button** — single click swaps foreground and background colours. **Double-click** (within 300ms) resets both to the default dark theme (`#0f1117` background, `#e6edf3` text).

---

## Markdown Support

Rendered via [marked.js](https://marked.js.org/) with GitHub Flavoured Markdown (GFM) enabled. Supported elements include:

- Headings `h1`–`h6`
- Paragraphs, bold, italic, strikethrough
- Ordered and unordered lists
- Task lists (checkboxes)
- Blockquotes
- Fenced code blocks with syntax highlighting (via highlight.js, github-dark theme)
- Inline code
- Tables
- Horizontal rules
- Images
- Links

---

## Session Persistence

Settings and the last viewed document are stored in the browser's `localStorage` under the key prefix `mdr_`. The following are persisted:

| Setting | Storage key |
|---|---|
| Last document (content + filename) | `mdr_doc` |
| Scroll position of last document | Updated within `mdr_doc` on every scroll event |
| Font size & line height | `mdr_settings` |
| Column width | `mdr_settings` |
| Background colour | `mdr_settings` |
| Foreground colour | `mdr_settings` |
| Font choice | `mdr_settings` |
| TOC open/closed | `mdr_settings` |
| Recent files history (up to 12 entries, each with scroll position) | `mdr_recent` |

### Chrome on Local Files

Chrome blocks `localStorage` for `file://` URLs by default. If this is the case a warning banner appears in the header with the fix. All settings will still apply for the current session — only persistence across reloads is affected.

**Fix:** Close all Chrome windows, then relaunch with the flag:

```bash
google-chrome --allow-file-access-from-files
```

Or for Chromium:

```bash
chromium-browser --allow-file-access-from-files
```

To make this permanent, add an alias to `~/.bashrc`:

```bash
alias mdread='google-chrome --allow-file-access-from-files /path/to/markdown-reader.html'
```

**Firefox** works out of the box with no flags required.

The warning banner only appears when localStorage actually fails a test write — if the flag is already set, or if the browser permits it by default, the banner stays hidden.

---

## Keyboard Shortcuts Summary

| Shortcut | Action |
|---|---|
| `Ctrl+O` | Open file picker |
| `PgUp` | Scroll up one page |
| `PgDn` | Scroll down one page |
| `Ctrl+−` | Decrease font size |
| `Ctrl+=` | Increase font size |
| `Ctrl+←` | Narrow text column |
| `Ctrl+→` | Widen text column |

---

## Known Limitations

- **Full file path unavailable** — browsers expose only the filename when opening local files via `<input type="file">`, not the full path. The recent files list therefore stores and copies only the filename. This is a browser security restriction and cannot be worked around from within the page.
- **File re-open from recent list** — clicking a recent entry copies the filename to the clipboard. You must manually navigate to the file using the file picker. Only the most recently stored document can be reopened directly from the recent list.

---

## Technical Notes

- Pure HTML/CSS/JS — no build step, no framework, no npm.
- All state is held in memory and `localStorage`; nothing is written to disk or sent anywhere.
- The document map uses an HTML5 `<canvas>` element rendered by walking the live DOM — not a screenshot or iframe.
- The native browser scrollbar is hidden; all scrolling is handled by the inner `#content-wrap` container.
- Syntax highlighting uses the `github-dark` theme from highlight.js and can be replaced by swapping the CDN stylesheet link.
- The reading timer is a simple `setInterval` counter; it does not account for time spent with the tab in the background.
