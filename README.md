# Find File Types

A macOS swiftUI app that catalogs and monitors file storage across watched folders — NAS volumes, LucidLink filespaces, SANs, local RAIDs, and USB SSDs.

Drop a folder to watch and the app scans it recursively, recording every file by type, size, modification date, and symbolic link status into a local SQLite database. A compact bottom banner shows scan progress without blocking the UI, with a cancel button in case you change your mind on a very large local or network volume. Scheduled rescans build historical snapshots so you can track how storage grows over time and answer questions like "how many MXF camera originals are on the cloud drive?" or "is proxy usage growing?"

## What's New in 1.5

- **Uncategorized file type classifier** — a new sidebar tab lists every unknown extension sorted by impact, with an inline classification form, autocomplete, and sample file paths for context.
- **Completion tracking** — a progress bar shows what percentage of your files are categorized, with color-coded thresholds.
- **Contribute classifications** — export your user-added types as CSV or copy to clipboard to share via GitHub and help improve the default catalog for everyone.
- **Ignore junk extensions** — hide generated or irrelevant extensions so they don't clutter the list or lower your completion score.
- **NEW badges** — freshly discovered extensions after each scan are flagged for quick classification.

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
- **Menu bar dropdown** — colored pills showing storage breakdown by group, per-folder or all folders
- **Search** — find files by name, extension, size range across all watched folders
- **Offline support** — unmounted volumes show last-known data from the database
- **Alert rules** — define visual thresholds per extension, category, or group by total size or file count, across all watched folders or one folder

## Requirements

- macOS 14.0+

## Database

Stored in `~/Library/Application Support/FindFileTypes/findfiletypes.sqlite`. Contains:

| Table | Purpose |
|-------|---------|
| `watched_folders` | Registered folder paths, schedules, last scan date |
| `files` | Current file inventory (upserted each scan) |
| `scan_snapshots` | Aggregate totals per scan for historical tracking |
| `snapshot_type_summary` | Per-extension breakdown per snapshot |
| `snapshot_folder_summary` | Per-subfolder breakdown per snapshot |
| `file_type_categories` | Extension-to-category/group mapping with colors |
| `bundle_exclusions` | Extensions to skip during scanning |
| `busy_windows` | Day/time windows when scanning is deferred |
| `alert_rules` | Storage threshold alerts (planned) |

## Links

- Website: [code.matx.ca](https://code.matx.ca)

## License

Copyright 2026 Mat X. All rights reserved.
