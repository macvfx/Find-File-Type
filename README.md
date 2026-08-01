# Find File Types

A native macOS app that catalogs and monitors file storage across watched folders — NAS volumes, LucidLink filespaces, SANs, local RAIDs, and USB SSDs.

Drop a folder to watch and the app scans it recursively, recording every file by type, size, modification date, and symbolic link status into a local SQLite database. A compact bottom banner shows scan progress without blocking the UI, with a cancel button in case you change your mind on a very large local or network volume. Scheduled rescans build historical snapshots so you can track how storage grows over time and answer questions like "how many MXF camera originals are on the cloud drive?" or "is proxy usage growing?"

## What's New in 1.6 (build 1)

- **Retained-data scan safety** — zero-file and major-drop rescans keep the existing catalog until you explicitly approve replacement.
- **Guarded full rebuilds** — every watched folder is scanned and checked before any current inventory or historical snapshots are removed.
- **Volume-aware Insights** — identically named folders show their volume/location alias, with the complete path available as hover help.
- **Uncategorized evidence export** — working CSV and clipboard exports include counts, sizes, and up to three representative paths for context.
- **New file types** — contributed defaults for Bill of Materials, calendar, FileMaker, OmniGraffle, SNS logs, Swift, Python, configuration, Perl, and RED metadata files.
- **Reproducible updates and releases** — the updater is pinned to the remote GitHubUpdateChecker 1.0.2 package and the project now includes validated signed/notarized release tooling.

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

- macOS 14.0+
- Xcode 16.0+
- Swift 6.0

## Build

Open `FindFileTypes.xcodeproj` in Xcode and build (Cmd+B), or from the terminal:

```bash
cd FindFileTypes
swift build
swift run
```

The app resolves `GitHubUpdateChecker` from its remote GitHub Swift package at
the revision recorded in `Package.resolved`. Build machines need package-repo
access; installed copies of the app do not.

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
| `alert_rules` | Configured visual storage threshold alerts |

## Links

- Website: [code.matx.ca](https://code.matx.ca)
- [User Guide](UserGuide.md)
- [Workflow Guide](WORKFLOW_GUIDE.md)

## License

Copyright 2026 Mat X. All rights reserved.
