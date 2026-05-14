# Find File Types

A native macOS app that catalogs and monitors file storage across watched folders — NAS volumes, LucidLink filespaces, SANs, local RAIDs, and USB SSDs.

Drop a folder to watch and the app scans it recursively, recording every file by type, size, modification date, and symbolic link status into a local SQLite database. A prominent in-progress overlay appears while scanning, with a cancel button in case you change your mind on a very large local or network volume. Scheduled rescans build historical snapshots so you can track how storage grows over time and answer questions like "how many MXF camera originals are on the cloud drive?" or "is proxy usage growing?"

## What's New in 1.2 (build 3)

- Cancelable scan overlay for long-running folder scans, including network volumes.
- Bigger, more prominent scan progress UI with a clear cancelling state.
- Preferred schedule hour/minute pickers now show the selected value reliably.
- Alert rules can now target either total size or file count, and can be scoped to a specific watched folder.
- Sidebar watched folders now show a storage/location badge and reveal the full path on hover.
- Offline watched-folder failures now show a non-blocking banner instead of a modal error.
- Offline warning banners can now be muted for `1 hour`, `1 day`, or `always`, and `always` overrides can be managed in Settings → Alerts.
- Real-world catalog expansion: added recognition for `mpeg`, `mts`, `m2ts`, `lrv`, `flv`, `swf`, `img`, `sparseimage`, `disc`, `eml`, `yml`, `yaml`, `url`, `webarchive`, `textclipping`, `omf`, `aaf`, `lrprev`, and `lprev`.
- Junk-extension filtering is smarter about generated restore / scratch suffixes while preserving real formats like `3gp` and `7z`.

## Features

- **Drag-and-drop** folder watching — or use the + button
- **Cancelable scans** — every manual scan shows a prominent progress overlay with a `Cancel Scan` button
- **Four-axis classification** — Group (Video, Audio, Images, Documents, Archives, Projects), Category (structural file-type class), Stage (Preproduction → Archive workflow lifecycle), Role (what the file is used for)
- **100+ file types** pre-configured with sensible defaults for media/post-production workflows, including camera raw, ProRes containers, color pipeline files (CDL, CUBE, OTIO), and editorial metadata (MHL, ALE, XMP, EDL)
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
