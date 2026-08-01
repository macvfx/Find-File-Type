# Changelog

All notable changes to **Find File Type** are documented here. The project follows roughly [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) conventions; user-facing changes first, then schema / build housekeeping.

## [1.6] — 2026-07-31 (build 1)

Protect retained scan history when storage is unavailable or unexpectedly
incomplete, make same-named watched folders distinguishable in Insights, and
turn unknown file types into useful, contextual CSV evidence.

### Added

- Safety confirmation before a zero-file scan or a scan with a major file/folder-count drop can replace retained catalog data.
- Preflight-first **Rebuild Current Data** workflow: every watched folder must scan successfully before existing scan records and snapshots are removed.
- Uncategorized CSV and clipboard exports containing file counts, formatted and byte sizes, and up to three representative full paths.
- Default classifications contributed for `.bom`, `.ics`, `.fmp12`, `.graffle`, `.sns`, `.swift`, `.py`, `.conf`, `.pl`, and RED `.rmd` metadata.
- Focused tests covering scan-drop safeguards, volume aliases, uncategorized CSV quoting, sample paths, and contributed defaults.
- Reusable signed/notarized DMG validation and dry-run-first GitHub release tooling.

### Changed

- Insights folder choices show the folder name plus volume/location alias and expose the complete path as hover help.
- GitHubUpdateChecker now resolves from the remote `1.0.2` Swift package with a committed revision pin instead of requiring a sibling checkout.
- User, workflow, and release documentation now describe retained-data safety, exports, update channels, and the approval-gated release process.

### Fixed

- Uncategorized file type CSV export and clipboard copy now operate on the displayed filtered/sorted rows and report save/copy failures.

### Build

- `CFBundleShortVersionString = 1.6`, `CFBundleVersion = 1`.
- Release tag: `1.6+1`.

---

## [1.5] — 2026-06-02 (build 2)

Classify unknown file types directly in the app, track your coverage percentage, and contribute your classifications back to the community.

### Added (build 2)

- **Apple Archive (.aar)** — added to Archives group as a compressed archive format using Apple's AppleArchive framework.
- **Sony camera metadata (.bim)** — added to Projects/Ingest group as camera catalog metadata found in Sony XDRoot card structures alongside MXF and XML files; essential for syncing and managing footage metadata in complex editing scenarios.

### Changed (build 2)

- **Archiware P5 stubs** — `.p5c` (P5 Companion) is no longer marked as legacy; `.p5a` (P5 Archive) remains legacy. Both now carry notes explaining they are placeholder stubs left on disk after files are archived to tape.

### Added

- **Uncategorized sidebar tab** — lists every unclassified file extension across all watched folders, sorted by file count so the most impactful types appear first. Includes a search field and sort controls (by file count, total size, or extension name).
- **Inline classification form** — click Classify on any extension to expand a form with Group picker, Category autocomplete from existing types, optional Stage and Role fields. The row stays highlighted while editing. Color is auto-assigned based on group.
- **Completion percentage** — a progress bar in the Uncategorized header tracks what percentage of your files are categorized, with color-coded thresholds (red < 60%, orange 60–90%, green 90%+).
- **Ignore junk extensions** — click the x button to hide extensions you don't want to classify (generated suffixes, tool-specific scratch files). They stop counting against your completion percentage and can be restored from the Show Ignored toggle.
- **NEW badges** — extensions discovered since the last scan are flagged with a blue "NEW" badge.
- **Contribution bar** — a fixed bottom bar shows your user-added classification count with Export CSV and Copy to Clipboard buttons for sharing contributions via GitHub.
- **Export Contributions CSV** — exports only user-added types (not the built-in defaults) as a standard 9-column category CSV.
- **Copy Contributions to Clipboard** — copies user-added types in CSV format, ready to paste into a GitHub issue.
- **Save confirmation toast** — after classifying an extension, a green checkmark toast confirms where it was saved (e.g. "Saved .pl as Projects → shell script").

### Changed

- **Help window** — added a section covering the Uncategorized tab, classification workflow, and contribution process.
- **User Guide** — new Uncategorized section with full documentation of the classify, ignore, and contribute workflows.

### Schema

- Migrates from `schema_version = 3` → `4`. Adds `ignored_extensions` table for tracking junk extensions.

### Build

- `CFBundleShortVersionString = 1.5`, `CFBundleVersion = 2`.
  - Build 1 → 2: added `.aar` (Apple Archive) and `.bim` (Sony camera metadata) file types; corrected `.p5c` legacy flag and added Archiware context notes to both P5 stub types.

---

## [1.4] — 2026-05-29 (build 1)

Interactive folder drill-down, major startup performance improvement, Storage Facts, Help window, and a polished loading experience.

### Added

- **Folder drill-down** — clicking a subfolder in the By Folder view now drills into that directory, showing its immediate children (subfolders and files) with the same size breakdown. Works to any depth.
- **Breadcrumb navigation** — a clickable breadcrumb trail (Root > FolderA > SubfolderB) appears above the chart and list when drilled in. Click any segment to jump back to that level.
- **Loose file listing** — files at the current folder level are shown individually in a "Files" section beneath the subfolder list, with filename, extension, and size. Replaces the old "(files at this level)" placeholder.
- **Clickable chart bars** — tapping a bar in the horizontal bar chart drills into that subfolder, matching the list behavior.
- **Storage Facts popover** — a lightbulb button in the Overview toolbar shows a popover with fun facts about your storage: total files cataloged, largest category, most common types, biggest extension by size, unique type count, and scan history.
- **Loading highlights** — the startup loading screen now shows a rotating highlight reel of storage facts and usage tips instead of a blank spinner. Facts are fetched from pre-computed snapshot data (milliseconds) and display immediately while heavier queries load in the background.
- **Help window** — Help menu → "Find File Type Help" opens a dedicated help window covering Getting Started, Browse by Type, Browse by Folder, Track Trends, Search, Schedule Scans, Customize File Types, Menu Bar, and keyboard shortcuts.
- **About panel link** — the system About panel now shows a clickable link to code.matx.ca below the copyright.
- **Auto-select first folder on launch** — the app preloads the first watched folder's full detail data during startup and auto-selects it in the sidebar, eliminating the second loading screen when clicking into a folder.

### Changed

- **Folder breakdown list** — switched from Table to List with distinct sections for subfolders (blue, clickable) and loose files (document icon, sorted by size descending).
- **Summary footer** — now shows separate subfolder and loose file counts alongside the combined total file count and size.
- **Loading indicator** — the "Loading folder data…" spinner now only appears on the initial load of a folder. Subsequent data refreshes (post-scan, settings changes) update in place without blanking the view.
- **Chart styling** — loose-file entries ("." rows) render in gray to visually distinguish them from drillable subfolder bars.

### Fixed

- **Startup performance ~20x faster** — removed unnecessary `LOWER()` wrappers from all category JOIN queries. Extensions are already stored lowercase, so the `LOWER()` calls were forcing full table scans on 831K+ rows. The menu bar summary query dropped from ~21 seconds to ~1 second; per-folder type breakdown from ~10 seconds to ~0.7 seconds.
- **View flash on background refresh** — drilling into a subfolder no longer briefly shows a "Loading folder data…" screen when the app refreshes data in the background (e.g. after a scan completes). Data loads are now scoped to the current drill path and stale results from concurrent queries are discarded.
- **"No data — Scan to see folder sizes" flash during drill-down** — replaced with a brief spinner while the subfolder query runs.

### Build

- `CFBundleShortVersionString = 1.4`, `CFBundleVersion = 1`.
- Added `Credits.html` to the app bundle for the system About panel.

---

## [1.3] — 2026-05-26 (build 1)

Scanning no longer blocks the app, folder management is more discoverable, and junk extensions are filtered more aggressively.

### Added

- **Toolbar minus (−) button** — remove the currently selected watched folder directly from the sidebar toolbar, alongside the existing + button. Disabled when no folder is selected.
- **Remove confirmation dialog** — both the toolbar − button and the right-click "Remove" action now show a confirmation alert before deleting a watched folder and its scan history.
- **Maximum extension length filter** — new setting in Settings → Exclusions (default: 12 characters). Extensions longer than this limit are ignored during scanning unless they are already in the File Types catalog. Eliminates thousands of Xcode build artifact and restore-fragment suffixes from polluting uncategorized results.
- **Hash-suffix junk pattern** — extensions matching the pattern `prefix-randomhash` (e.g. `.h-2lj1v1oj8gf7r`, `.pcm-2do7c9yyfyqof`) are now filtered out during scanning.
- **22 new file types** from uncategorized scan data: `sh` (shell script), `fp7` (FileMaker Pro 7), `m4r` (audio ringtone), `tsv` (tab-separated values — common in Archiware P5 exports), `qt` (QuickTime clip), `aifc` (AIFF-C audio), `pcm` (raw PCM audio), `aup3` (Audacity project), `thm` (camera thumbnail), `pict` (legacy PICT image), `vcf` (vCard contact), `htm` (HTML page), `pps` (PowerPoint Show), `emlx` (Apple Mail message), `pkg` (macOS installer), `bz2` (bzip2 archive), `ipa` (iOS app archive), `sit` (StuffIt archive), `jar` (Java archive), `scpt` (AppleScript), plus `lrf`, `otf`, `acr`, `rtn`, `drt`, `ttml`, `dwg`, `dxf` from the previous build.

### Changed

- **Non-blocking scan indicator** — scanning a folder no longer covers the entire app with a modal overlay. A compact bottom banner shows scan progress, folder name, and a cancel button while you continue browsing other folders, insights, and search results.
- **Sortable type breakdown table** — all five columns (Type, Category, Group, Files, Size) in the By Type detail view are now sortable by clicking the column header. Default sort is by size descending. A totals footer shows type count, total files, and total size.
- **Total default file types** now ~150 (up from ~130).

### Fixed

- **App launch hang** — initial data loading (categories, menu bar summary, schedules) now runs off the main thread so the window appears immediately instead of blocking for several seconds on large databases.
- **Folder click hang** — selecting a watched folder in the sidebar no longer freezes the UI while aggregate queries run. Type breakdown, folder breakdown, and dead symlink queries now load asynchronously.
- **Slow aggregate queries** — added composite indexes on `files(folder_id, extension)`, `files(folder_id, size)`, and `file_type_categories(extension)` to speed up the type/folder breakdown JOINs.

### Build

- `CFBundleShortVersionString = 1.3`, `CFBundleVersion = 1`.

---

## [1.2] — 2026-05-15 (build 4)

Focused polish on scan control and real-world extension recognition.

### Added

- **Cancelable scans** — dropping a folder, adding one from the picker, or manually starting a scan now shows a prominent in-progress overlay with a `Cancel Scan` button so long-running local or network scans can be aborted cleanly.
- **More prominent scan status UI** — the previous small bottom-corner activity pill is now a centered progress card with clearer status text, file counts, and a visible cancelling state.
- **Expanded file-type recognition** from fresh real-world scans: `mpeg`, `mts`, `m2ts`, `lrv`, `lrf`, `flv`, `swf`, `img`, `sparseimage`, `disc`, `eml`, `yml`, `yaml`, `url`, `webarchive`, `textclipping`, `omf`, `aaf`, `lrprev`, `lprev`, `otf`, `acr`, `rtn`, `drt`, `dwg`, `dxf`, and `ttml`.
- **Offline warning controls** — when a watched folder is unavailable, the app now shows a passive banner with `Dismiss`, `1 Hour`, `1 Day`, and `Always` options instead of interrupting your current work with a modal error.
- **Offline warning override list in Settings → Alerts** — folders muted with `Always` are now listed in one place so you can re-enable their offline warnings later.

### Documentation

- Updated the README and User Guide to reflect the current `1.2 (build 4)` release and the latest real-world file-type recognition set.

### Changed

- **Junk-extension filtering** is more aggressive for generated restore / scratch suffixes, while still allowing legitimate digit-led extensions like `3gp` and `7z`.

### Fixed

- **Cancelling a scan now stops directory enumeration itself** instead of only waiting for the current scan to finish in the background. Cancelled scans exit quietly without writing partial snapshots or showing a false "volume may not be mounted" error.
- **Schedule preferred-time pickers now reflect the selected hour and minute** instead of leaving the value visually unclear after selection.
- **Alert rules now distinguish file count from total size** and can be scoped to all watched folders or a specific watched folder, so thresholds like `100` are read back clearly as `100 files` or `100 GB total size`.
- **Watched folders with the same visible name are easier to tell apart** thanks to a storage/location alias badge in the sidebar plus full-path hover help.
- **Unavailable watched folders no longer steal focus with a modal error alert** when you already know the volume is offline and just want to browse existing results or open Settings.

### Build

- `CFBundleShortVersionString = 1.2`, `CFBundleVersion = 4`.

## [1.1] — 2026-05-12 (build 6)

The "Categories Overhaul" release. Adds layered Stage and Role classification on top of the existing Group / Category axis, expands the default file-type seed to ~102 entries, fixes several v1.0 miscategorizations, and excludes zero-byte non-symlink files from breakdowns by default.

Ships with the first **app icon**.

### Added

- **App icon** — full macOS icon set (16×16 through 512×512 @1x/@2x) wired through `Assets.xcassets/AppIcon.appiconset`. Replaces the generic placeholder that v1.0 shipped with.
- **Four-axis classification** — every file type now carries Group / Category / **Stage** / **Role** instead of just Group + Category. Stage covers the post-production lifecycle (Preproduction → Acquisition → Ingest → Editorial → Graphics → Finishing → Delivery → Archive → Admin). Role describes the job the file plays (`camera original`, `editorial media`, `timeline interchange`, `color pipeline`, …).
- **Legacy / niche flags** captured on rare or deprecated formats — `fcpproject`, `drp`, `p5a`, `p5c` flagged legacy; `mhl`, `motn`, `motr`, `fcpevent`, `xoundset`, `ctg`, `mif` flagged niche. Data only in 1.1 — round-trips through CSV but is not surfaced in the UI. A user-editable tags column that supersedes these flags is queued for v1.2.
- **27 new default file types**: `caf`, `exr`, `dpx`, `ttf`, `fdx`, `mif`, `html`, `fcpevent`, `xmp`, `edl`, `ale`, `otio`, `cube`, `cdl`, `motn`, `motr`, `log`, `p5a`, `p5c`, `dat`, `css`, `js`, `plist`, `xoundset`, `ctg`. Total default seed is now 102 rows (up from ~78).
- **Real-world catalog expansion** — added recognition for `m2v`, `vob`, `ac3`, `par`, `fcp`, `exe`, `tgz`, `eps`, `db`, `bmp`, `moti`, `tga`, `smi`, `pct`, `mocha`, and `ico` from live archive / restore samples.
- **Inline-editable category table** — every cell (Extension, Category, Group, Stage, Role, Color) is editable in place. Built-in rows are editable too.
- **Reset to default** ↺ button per row, visible only when the row differs from the v1.1 seed. **Reset All to Defaults** in the section toolbar restores the full seed.
- **CSV import / export** for the category schema, in three modes:
  - Export CSV — writes the canonical 9-column form (Extension, Category, Group, Stage, Role, Color, IsLegacy, IsNiche, Notes).
  - Import CSV (merge) — adds new extensions, updates existing rows by extension, leaves un-mentioned rows alone.
  - Import CSV (replace) — wipes the table and imports exactly what's in the CSV.
  Tolerant of v1.0-format 4-column CSVs (Extension, Category, Group, Color) — missing columns default sensibly in merge mode.
- **Stage filter** in Settings → File Types, alongside the existing Group filter.
- **Include zero-byte files in breakdowns** toggle under Settings → Exclusions. Off by default.
- **Rebuild Current Data** action in Settings → Exclusions — deletes all scan-derived file records, snapshots, and last-scan dates, then rescans every watched folder while preserving watched folders, categories, exclusions, alerts, and other settings.
- **CSV exports** of breakdowns from the main UI: type breakdown, folder breakdown, dead symlinks, search results, and scan history.
- `settings` key-value table for app-level prefs (`schema_version`, `include_zero_byte_files`). Cleaner than `PRAGMA user_version` for the prefs we'll keep adding.
- **Menu bar footer controls** — added a visible app version/build readout in the header plus `Quit` alongside `Open App`.

### Changed

- **`dng` group moved from Video to Images** — camera raw stills now live with the rest of the still imagery.
- **`mxf`** reclassified from "camera raw" → **professional container** (Stage: Acquisition, Role: camera original).
- **`mov`** reclassified from "editing file" → **video container** (Stage: Editorial, Role: editorial media).
- **`fcpxml`** → **project interchange** (Role: timeline interchange).
- **`prproj`** → **project file** (Role: editorial project).
- **`aep`** → **motion graphics project** (Stage: Graphics).
- **`drp`** → **project archive** (color project, legacy).
- **Delivery video formats** (`mp4`, `m4v`, `webm`, `mkv`, `avi`, `wmv`, `mpg`) → **delivery video** (Stage: Delivery).
- **`ts`** → **transport stream** (broadcast or stream delivery).
- **Lossless audio** (`wav`, `aif`, `aiff`, `flac`) → **lossless audio**, with per-row Stage/Role.
- **Lossy audio** (`mp3`, `aac`, `m4a`, `ogg`, `wma`) → **lossy audio** (Stage: Delivery, Role: review audio).
- **`ai`** → **vector design** (Group: Images, Stage: Graphics).
- **`indd`** → **page layout** (Group: Images).
- **`tif` / `tiff`** → **raster image** (Stage: Finishing, Role: high-quality still master).
- **`svg`** → **vector image** (Stage: Graphics, Role: scalable graphic asset).
- **Document family** (`doc`, `docx`, `pages`) → **document** (Stage: Admin, Role: business document).
- **Spreadsheet family** (`xls`, `xlsx`, `numbers`) → **spreadsheet**.
- **`csv`** → **tabular data**.
- **`pdf`** → **fixed-layout document**.
- **Presentation family** (`ppt`, `pptx`, `key`) → **presentation**.
- **`txt`** → **plain text**, **`rtf`** → **rich text**, **`md`** → **markup text** (Role: documentation).
- **Disk images** (`dmg`, `iso`) → **disk image** (Stage: Admin, Role: software package / disk image).
- **`xml` / `json`** → **structured data** (Group: Projects, Role: structured metadata).
- **`mhl`** → **media hash list** (Stage: Archive, Role: verification manifest).
- **`fcpbundle`** → **library bundle** (Group: Projects, Role: editorial library).
- **`fcpproject`** → **project file** (Group: Projects, flagged legacy).
- Zero-byte non-symlink files are now skipped both at scan time (excluded from `snapshot_type_summary` aggregation) and at query time (filtered from breakdown queries). The `files` table still records them so they remain searchable. **Dead symlinks** (size 0, `is_symlink = 1`) are intentionally still counted in breakdowns and listed in the Dead Symlinks tab.
- Disposable or malformed extensions are now ignored during scanning so they do not pollute totals, including numeric-only suffixes like `.01` / `.02` / `.03`, generated `sb-...` suffixes, and explicit junk patterns like `.0psd`, `.autosave`, and `.tmp`.
- **Overview's watched-folder list** now reads as a scan-status panel with explicit click-through into folder detail, instead of feeling like a second non-functional sidebar.
- README's "70+ file types" claim updated to "100+ file types".

### Fixed

- **Icon not displaying when running the app** — `Info.plist` was missing `CFBundleIconFile` and `CFBundleIconName`. With `GENERATE_INFOPLIST_FILE = NO`, Xcode does not auto-inject those keys from `ASSETCATALOG_COMPILER_APPICON_NAME`. Added both keys (set to `AppIcon`) to `Sources/Info.plist` and to `project.yml` so future `xcodegen` regenerations don't revert the fix.
- **Settings window shrinks when switching to Alerts or About, and won't grow back when returning to File Types** — macOS `TabView` sized the window to the active tab's intrinsic content. Pinned every Settings tab to a shared minimum frame (860×540 — enough for the File Types row to lay out without clipping) so the window keeps a stable floor across tabs.
- **Inline `legacy` / `niche` capsules clipped the extension text in the Ext column.** Capsules removed; the flag data is preserved in the model, database, and CSV export. A proper user-editable tags column is queued for v1.2.
- **Settings panes felt cramped around explanatory copy and controls** — added shared interior padding so sections have more breathing room and read more like distinct panes.
- **Menu bar `Open App` could fail to visibly raise the main window** — switched the main scene to a named singleton window and now explicitly activates, deminiaturizes, and brings forward the app window from the menu bar action.

### Schema

Migrates from `schema_version = 1` → `2` on first launch. The migration is idempotent and forward-only.

- New table: `settings (key TEXT PRIMARY KEY, value TEXT)`.
- `file_type_categories` gains: `stage_name TEXT`, `role_name TEXT`, `notes TEXT`, `is_legacy INTEGER DEFAULT 0`, `is_niche INTEGER DEFAULT 0`.
- `snapshot_type_summary` gains: `stage_name TEXT`, `role_name TEXT`. Populated for all scans from v1.1 onward; pre-v1.1 snapshot rows stay `NULL` and will render as an "Unknown" series once Stage/Role trend charts ship in v1.2.
- Content migration: for each row whose `category_name` still matches its v1.0 default, overwrite category / group / stage / role with the v1.1 seed values. User-edited rows are preserved by virtue of no longer matching the v1.0 default. Colors are preserved during migration; the per-row reset button restores the seed color.

A v1.0 binary **cannot** open a v1.1-migrated database cleanly because of the added columns and the `settings` table. To roll back you'd need to restore a pre-v1.1 backup of `~/Library/Application Support/FindFileTypes/findfiletypes.sqlite`.

### Build

- `CFBundleShortVersionString = 1.1`, `CFBundleVersion = 6`.
  - Build 2 → 3: app icon wired up; `CFBundleIconFile` + `CFBundleIconName` added to Info.plist.
  - Build 3 → 4: Settings window shrink fix (shared 860×540 pane floor); inline legacy / niche capsules removed from the categories row.
  - Build 4 → 5: added the Settings rebuild-scan-data action, expanded the shipped file-type catalog from real storage samples, and filtered disposable numeric/junk extensions out of scan totals.
  - Build 5 → 6: clarified the Overview scan-status panel, added more Settings pane whitespace, improved menu bar window-raising behavior, and added version/quit controls to the menu bar.
- Archived `FindFileType_Categories_reviewed.csv` and `FindFileType_Categories_role_first_pass.csv` into `FindFileTypes/archive/`. The shipped `FindFileType_Categories.csv` is rewritten with the new 9-column header.

### Known limitations / deferred

- Stage/Role surfacing in Insights, folder detail, search, and the menu bar lands in **v1.2** — the data is captured starting from this release but no view exposes it yet.
- `cpf` and `locked` remain intentionally unclassified pending real-world samples.
- macOS notification delivery for alert rules is still visual-only.

---

## [1.0] — 2026

Initial release.

- Drag-and-drop folder watching with recursive scanning and symlink handling.
- SQLite database (`~/Library/Application Support/FindFileTypes/findfiletypes.sqlite`) with historical snapshots, WAL journal mode.
- File-type categories (group + workflow category, ~78 defaults).
- Bundle exclusions (`.fcpbundle`, `.app`, `.photoslibrary`, `.fcpproject` by default).
- Scheduled scanning — hourly / daily / weekly with preferred time and busy-window deferral.
- NavigationSplitView with sidebar, Overview, Insights, Search.
- Pie chart by file type, horizontal bar chart by subfolder, Swift Charts trend lines (total / by extension / by group).
- Dead symlink detection and dedicated listing.
- Search by name, extension, size range.
- Menu bar dropdown with colored pills and stacked-bar visualization.
- Settings: File Types, Exclusions, Schedules, Alerts, About.
- Offline mode — unmounted volumes show last-known data.
- GitHubUpdateChecker integration (`macvfx/Find-File-Type`).
