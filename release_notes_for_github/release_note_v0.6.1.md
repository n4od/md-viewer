# Release v0.6.1 — Drag & Drop, Actually

A one-bug release. Dropping a Markdown file onto the window did nothing at all: the drop
handler was listening for an event the app never receives, so every drop was discarded in
silence. It is fixed, and the full-window overlay that was supposed to announce a drop
shows up now too.

## 🐛 Fixed

### 🖱️ Dropping a File Opened Nothing

- **The drop was delivered, and then ignored.** Tauri hands a drop to the app as a
  *webview* event only when the webview is a separate child of its window. This window's
  webview **is** its whole content, so the runtime synthesizes a *window* event for it
  instead — and the handler was registered on the webview. Nothing was listening where the
  drop actually arrived: no overlay, no document, no error message.
- **The handler now sits where the drop lands.** It runs on the same window-event path as
  the close request, so dragging a file over the window shows the drop overlay, and
  releasing it opens the document — for a single file, for a multi-file selection, and for
  every Markdown extension the app already knows.
- **Each drop costs what opening a document always cost.** No polling, no timers, no new
  threads: a few one-field messages while a file hovers, and one read-and-render when it is
  released. Idle and startup behaviour are unchanged.

### ⏳ Broken Since the First Release

- The v0.1.0 notes listed drag & drop as a way to open a file. That was never true: the
  handler could not fire, and nothing else in the app noticed because `Ctrl`+`O`, the
  welcome screen, the recent list and Explorer all open files by other routes. If you
  tried to drop a file and concluded the feature did not exist, you were right.

## 📅 Recent Version History

- **v0.6.1**: Drag & drop works.
- **v0.6.0**: A frameless window whose toolbar is the title bar, and a reading surface that scrolls.
- **v0.5.0**: One band of chrome, Ctrl+wheel zoom, and the heading `#` removed.
- **v0.4.0**: Keyboard-driven tabs, session restore, front-end tests and a fmt/clippy gate.
- **v0.3.4**: Markdown files get a document icon of their own.
- **v0.3.3**: Settings panel, two-button toolbar, scroll and shift fixes.

---

**Full Changelog**: [CHANGELOG.md](https://github.com/n4od/md-viewer/blob/main/CHANGELOG.md)
