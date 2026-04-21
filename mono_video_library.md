> [!IMPORTANT]
> ### Under Construction
> Mono Video Library is changing quickly. You probably won't recognise this repo tomorrow (nor will I).

# Mono, a distraction-free video library

![Demo Screenshot](src/assets/screenshots/demo.png)

Mono Video Library is a distraction-free desktop video library for organising and watching videos stored on your own machine. It is built with Tauri 2, vanilla HTML/CSS/JavaScript, Rust, and SQLite. Import source folders, build a virtual library without moving files on disk, annotate videos with descriptions and tags, search across files/folders/tags, and play videos in-app without sending your library to a cloud service.

## Current State

Mono Video Library is a working pre-release Tauri desktop app, currently focused on Windows/WebView2. The app stores library metadata, virtual organisation, descriptions, description drafts, tags, pin order, hidden-file state, blacklisted-file state, missing-file state, durations, resume positions, and playback history in a local SQLite database. Media streams from disk through Tauri's asset protocol.

The UI is custom HTML/CSS with a frameless window, custom title bar, sidebar, player controls, tooltips, dialogs, settings, search tray, context menus, and keyboard navigation. Expect implementation details and database migrations to continue moving while the app is in early access.

## Features

### Sources

- Import one or more local source folders with the folder picker.
- Drag folders from the operating system file explorer into the sidebar to import them.
- Choose whether imported folders keep their folder structure in the Library or place videos in `(Unsorted)`.
- Remember preserved-structure imports so newly discovered files land under the same virtual root on refresh.
- Skip duplicate and nested source imports when the selected folder is already covered.
- Refresh sources to rescan folders, add new videos, restore found files, mark missing files, fill durations, and skip blacklisted files.
- Remove source folders from Mono Video Library without deleting files from disk.
- Rename source display names and reveal source folders in the OS file explorer.
- Select multiple source folders with Ctrl/Cmd-click, Shift-click, or Ctrl/Cmd+A, then remove them together.

### Library

- Browse sources, virtual folders, subfolders, and videos in a collapsible tree.
- Keep real disk paths separate from virtual organisation: `path` is the absolute disk path, while `virtual_path` controls where a video appears in Mono Video Library.
- Create top-level virtual folders and nested subfolders without moving files on disk.
- Use `(Unsorted)` for imported videos that have not been organised.
- Drag videos and folders into virtual folders; move nested virtual folders back to the Library root.
- Keep videos inside folders: dropping videos directly onto the Library root is blocked.
- Sort unpinned rows by name, file type, or length, with ascending and descending direction.
- Keep `(Unsorted)` and pinned folders above normal sorted rows.
- Pin folders and subfolders, then reorder pinned peers by dragging the pin handle.
- Display available, missing, and hidden counts for folders.
- Display video durations after metadata has been read, then persist durations for sorting.
- Backfill missing durations in the background after import, refresh, or startup.
- Select multiple Library rows with Ctrl/Cmd-click, Shift-click, or Ctrl/Cmd+A.
- Rename videos, virtual folders, subfolders, and source display names.
- Remove videos, folders, virtual folders, and subfolders from Mono Video Library without deleting files from disk.
- Remove selected videos or folders in bulk.
- Remove virtual folders while moving their videos back to `(Unsorted)`.
- Hide videos from the normal Library view and optionally show hidden files from Settings.
- Clear all missing video entries from the Library in bulk.

### Search

- Search video filenames with a debounced search bar.
- Search results include video matches, folder suggestions, and tag suggestions.
- Prefix a query with `/` to search folders.
- Prefix a query with `#` to search tags.
- Click a folder or tag suggestion to add it as a filter.
- Combine filename search with one or more folder and tag filters.
- Match any selected tag or require every selected tag.
- Open the filter tray from the compact filter pill and remove individual filters.
- Remove virtual folders or tags directly from search results when supported.
- Use Up/Down to move through results and Enter to open the selected result.
- Use Ctrl/Cmd+K, or `/` outside text fields, to focus search.
- Use Escape to clear the current search UI.

### Playback

- Click a video in Library, Search, or History to play it in the main panel.
- Use browser-style back and forward buttons for the current playback session.
- Use custom play/pause, mute, volume, seek, timestamp, zoom, and fullscreen controls.
- Cycle video zoom between Fit, Crop, and Stretch.
- Use keyboard playback controls: Space/K for play or pause, M for mute, F for fullscreen, arrow keys for seek and volume.
- Rename the current video from the player title.
- Treat video renames as virtual display renames: Mono Video Library updates the database and matching history display names but does not rename the file on disk.
- Persist duplicate display-name suffixes so rows remain distinguishable.
- Save video durations after metadata loads.
- Optionally resume videos from the last saved playback position.
- Mark missing files when detected and offer removal of stale library entries.
- Fade player controls and cursor during fullscreen playback, then restore them on interaction.

### Descriptions And Tags

- Add a description to each video.
- Autosave in-progress description drafts separately from the saved description.
- Save descriptions with Ctrl/Cmd+Enter.
- Cancel description edits to return to the saved text.
- Add and remove tags from the player tag row.
- Manage tags for one video or selected Library videos from the context menu.
- Reuse existing tags from tag dialog suggestions.
- Remove a tag globally from search results; Mono Video Library removes it from matching videos.

### History

- Log played videos to the History section.
- Group entries by Today, Yesterday, or full date labels.
- Collapse and expand date groups.
- Play videos directly from History.
- Remove individual history entries or whole date groups without changing the Library.
- Respect the hidden-files setting when showing history entries.

### Settings

- Open Settings from the sidebar footer.
- Choose light, dark, or system theme.
- Set UI zoom to Small, Default, or Large.
- Turn automatic source refresh on startup on or off.
- Turn resume playback on or off.
- Include or hide hidden files in the Library and History views.
- Unhide individual videos or clear the hidden list.
- Remove individual blacklisted files or clear the blacklist.
- Persist theme, zoom, hidden-file display, auto-scan, resume playback, sidebar width, video zoom, and sidebar section expansion in `localStorage`.

### Context Menus And Removal

- Right-click Library rows, Sources, and History entries.
- Available actions are filtered by item type.
- Supported actions include Pin/Unpin, Rename, Reveal in File Explorer, Tag file(s), Hide/Unhide, Add Folder, Remove from Library, Remove Source, Remove Folder, Remove from History, and Remove History.
- Bulk actions work with selected source folders, Library folders, and Library videos.
- Library removal updates Mono Video Library's records; it does not delete media files from disk.
- Video removal can optionally blacklist paths so the files are not re-imported on future scans.
- System folders such as `(Unsorted)` cannot be renamed, removed, or manually unpinned.

### Window, Sidebar, And Help

- Use a custom title bar with minimize, maximize, close, and an About dialog on the Mono Video Library logo.
- Drag the window from non-interactive title-bar space.
- Resize the sidebar with the vertical handle; width is remembered.
- Collapse and expand Sources, Library, and History; section state is remembered.
- Native window minimums and CSS layout minimums protect the sidebar and main content from being resized below usable sizes.
- Open the Help menu for an in-app guide to importing, organising, searching, playback, notes, tags, history, context menus, settings, and window controls.
- Close dialogs, menus, edits, and search states with Escape.

## Supported Video Formats

`mp4`, `mkv`, `avi`, `mov`, `webm`, `flv`, `wmv`, `m4v`, `ts`, `m2ts`, `mpg`, `mpeg`

## Stack

| Layer | Technology |
|---|---|
| App shell | Tauri 2 |
| Frontend | Vanilla HTML, CSS, JavaScript modules |
| Backend | Rust |
| Database | SQLite via `rusqlite` |
| File scanning | `walkdir` |
| Native plugins | Tauri dialog and opener plugins |

## Project Layout

| Path | Purpose |
|---|---|
| `src/index.html` | App shell markup, dialogs, sidebar, player, settings, and help panel |
| `src/script.js` | Frontend entry point and module wiring |
| `src/modules/` | Feature modules for playback, search, settings, sidebar, tree rendering, drag/drop, dialogs, context menus, tooltips, title bar behavior, accessibility, and the help menu |
| `src/styles/` | CSS split by feature ownership |
| `src/assets/` | Icons, branding, fonts, and screenshots |
| `src-tauri/src/` | Rust application bootstrap, commands, database layer, models, and utilities |
| `src-tauri/tauri.conf.json` | Tauri window, bundle, asset protocol, and build configuration |
| `architecture.md` | Source of truth for module boundaries, data flow, state ownership, and refactoring rules |

## Development

### Prerequisites

- Rust
- Node.js
- Platform requirements for Tauri 2 development

### Install

```bash
npm install
```

### Run

```bash
npm run tauri dev
```

### Build

```bash
npm run tauri build
```

### Useful Checks

```bash
node --check src/script.js
node --check src/modules/player.js
node --check src/modules/search.js
cargo check --manifest-path src-tauri/Cargo.toml
cargo test --manifest-path src-tauri/Cargo.toml
```

## Architecture Notes

- The frontend is split into ES modules under `src/modules`.
- Modules follow the layering and ownership rules in `architecture.md`.
- `script.js` only imports modules and performs startup callback wiring.
- Frontend code calls Rust through wrappers in `src/modules/tauri.js`.
- Runtime UI state lives in `AppState` in `src/modules/state.js`.
- User preferences that do not need database storage live in `localStorage`.
- The Rust backend owns SQLite, filesystem scans, virtual path mutations, pin persistence, descriptions, drafts, tags, hidden-file state, blacklists, missing-file state, video duration persistence, resume positions, history persistence, and Library refresh behavior.
- SQLite schema changes are managed by numbered migrations in `src-tauri/src/database/schema.rs`.
- OS folder drops use Tauri's WebView drag/drop API because HTML5 drag/drop is unreliable in WebView2.

## Data Model Summary

Mono Video Library stores these main kinds of data:

- `folders`: top-level real source folders, display names, hidden state, and preserved-structure import roots.
- `videos`: scanned video files, including absolute disk `path`, mutable `virtual_path`, display `filename`, duplicate-name suffix metadata, saved descriptions, autosaved description drafts, missing-file state, hidden state, stored `duration_secs`, and stored `resume_position_secs`.
- `virtual_folders`: user-created organisational folders.
- `history`: watched video entries.
- `video_tags`: normalised tags assigned to videos for search and filtering.
- `blacklisted_files`: paths excluded from future scans and re-imports.
- `library_pins`: persistent pin order for Library rows.

## Credits

- UI icons from [Iconoir](https://iconoir.com)

## License

Source available for viewing and educational purposes only. Copying, forking, distribution, and derivative works are not permitted under the current pre-release license. See `LICENSE.md`.

## Recommended IDE Setup

VS Code with the Tauri extension and rust-analyzer.
