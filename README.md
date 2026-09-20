# Find File Types

A native macOS app that catalogs and monitors file storage across watched folders — NAS volumes, LucidLink filespaces, SANs, local RAIDs, and USB SSDs.

Drop a folder to watch and the app scans it recursively, recording every file by type, size, modification date, and symbolic link status into a local SQLite database. A compact bottom banner shows scan progress without blocking the UI, with a cancel button in case you change your mind on a very large local or network volume. Scheduled rescans build historical snapshots so you can track how storage grows over time and answer questions like "how many MXF camera originals are on the cloud drive?" or "is proxy usage growing?"

## What's New in 1.7.1 (build 2)

- **Large folders no longer run out of memory** — a scan used to hold every file it found in memory until it finished, costing around 1 KB per file. Files are now written to the database as they are found, so peak memory is set by the batch size rather than the size of the folder. Watching the top level of a multi-million-file share is now no more expensive than watching a small local one.
- **The app stays responsive while a scan is saved** — the catalog write runs off the main thread.
- **Search matches names containing quotes** — a filename with an apostrophe previously returned nothing; `%` and `_` in a search are now matched literally.
- **Accurate paths in mirrored trees** — a file whose path repeated the watched folder's full path (a backup or proxy tree mirroring the volume layout) had that repeat stripped from its recorded path.
- **Bounded file lists** — the dead-symlink and file lists show the first 1,000 rows and report the true total; CSV export still writes the complete list.
- **No more flashing scan banner** (build 2) — a folder on an unmounted volume retried its scheduled scan every 15 seconds, showing and hiding the scanning banner each time. The retry is now counted from when the scan started.
- **The View label no longer wraps** (build 2) — it shared its width with the segmented control beside it and broke mid-word.

### Since 1.6

- **Clearer Help and steadier layout** — bold accent-colored Help headings, explicit width constraints on the sidebar and detail panes, and a 980-point minimum window width.
- **More file types** — defaults for Audacity, playlist, TV database, SQLite, checksum and tag files, plus camera-card metadata under Projects / Ingest. Existing custom classifications are preserved.

For the full release history, see [CHANGELOG.md](CHANGELOG.md).

## Features

- **Drag-and-drop** folder watching — or use the +/− toolbar buttons
- **Cancelable scans** — every manual scan shows a non-blocking bottom banner with a `Cancel` button
- **Four-axis classification** — Group (Video, Audio, Images, Documents, Archives, Projects), Category (structural file-type class), Stage (Preproduction → Archive workflow lifecycle), Role (what the file is used for)
- **150+ file types** pre-configured with sensible defaults for media/post-production workflows, including camera raw, ProRes containers, color pipeline files (CDL, CUBE, OTIO), and editorial metadata (MHL, ALE, XMP, EDL)
- **Inline-editable categories** — edit any cell, reset a row to its default, bulk import/export the category schema as CSV
- **Scheduled scanning** — hourly, daily, or weekly with preferred time and busy-window support (same scheduling model as BackupTrust)
- **Symbolic link awareness** — symlinks counted at 0 bytes, dead symlinks flagged in a dedicated view
- **Zero-byte filter** — files that don't use storage are excluded from breakdowns by default (toggle in Settings)
- **Bundle exclusions** — skip `.fcpbundle`, `.app`, `.photoslibrary` and other bundles (configurable)
- **Junk-extension filter** — disposable numeric-only suffixes, generated restore fragments, and known junk extensions are ignored during scanning so they do not pollute totals
- **Historical snapshots** — indefinite retention, trend charts via Swift Charts
- **Guarded rescans** — zero-file and major file/folder drops retain the prior catalog until you explicitly approve replacement
- **Volume-aware Insights** — folders with the same name include their storage alias, with the complete path available as hover help
- **Uncategorized evidence export** — export or copy the displayed unknown extensions with counts, sizes, and up to three representative paths
- **Menu bar dropdown** — colored pills showing storage breakdown by group, per-folder or all folders
- **Search** — find files by name, extension, size range across all watched folders
- **Offline support** — unmounted volumes show last-known data from the database
- **Alert rules** — define visual thresholds per extension, category, or group by total size or file count, across all watched folders or one folder

## Requirements

macOS 14.0 or later. Universal binary — Apple silicon and Intel.

## Download

Signed and notarized builds are on the
[releases page](https://github.com/macvfx/Find-File-Type/releases/latest). Each
release ships the disk image and a `.sha256.txt` checksum. The app checks for
updates on its own and will tell you when a newer version is published.

## Database

Stored in `~/Library/Application Support/FindFileTypes/findfiletypes.sqlite`. Contains:

| Table | Purpose |
|-------|---------|
| `watched_folders` | Registered folder paths, schedules, last scan date |
| `files` | Current file inventory (upserted each scan) |
| `files_staging` | Rows written during a scan in progress, merged into `files` only once the scan is accepted |
| `scan_snapshots` | Aggregate totals per scan for historical tracking |
| `snapshot_type_summary` | Per-extension breakdown per snapshot |
| `snapshot_folder_summary` | Per-subfolder breakdown per snapshot |
| `file_type_categories` | Extension-to-category/group mapping with colors |
| `bundle_exclusions` | Extensions to skip during scanning |
| `busy_windows` | Day/time windows when scanning is deferred |
| `alert_rules` | Configured visual storage threshold alerts |

## Links

- Website: [code.matx.ca](https://code.matx.ca)
- [User Guide](UserGuide.md)
- [Workflow Guide](WORKFLOW_GUIDE.md)

## License

Copyright 2026 Mat X. All rights reserved.
