# Find File Types — User Guide

## Getting Started

### Adding a Watched Folder

1. **Drag and drop** a folder from Finder onto the app window, or
2. Click the **+** button in the sidebar toolbar and select a folder

The app immediately starts scanning the folder and adds it to the sidebar under "Watched Folders."

While a scan is running, the app shows a centered progress overlay with the folder name, file count, and a **Cancel Scan** button. This is especially useful if you accidentally start scanning a very large NAS, SAN, or cloud-backed folder and want to stop before it completes.

### Supported Volume Types

Find File Types works with any mounted volume:
- Local drives and RAIDs
- NAS volumes (SMB/AFP/NFS)
- LucidLink filespaces
- SAN volumes (Xsan, StorNext)
- USB/Thunderbolt SSDs and HDDs

If a volume is unmounted, the app shows the last-known data from the database and marks the folder with a warning icon.

---

## Main Window

### Sidebar

The sidebar has two sections:

- **Navigation** — Overview, Insights, and Search views
- **Watched Folders** — all registered folders with last-scan timestamps

Right-click a folder for options:
- **Scan Now** — trigger an immediate rescan
- **Active** toggle — pause/resume scheduled scanning
- **Reveal in Finder** — open the folder in Finder
- **Remove** — stop watching and delete scan data

If a scan is already running, new manual scans are blocked until the current one finishes or is cancelled.

### Overview

Shows summary cards (folder count, total size, total files, type groups) and a list of all watched folders with their schedule status:
- **Green badge** — next scan scheduled
- **Orange badge** — scan deferred (busy window active)
- **Grey badge** — paused

### Folder Detail (click a folder in sidebar)

Three tabs:
- **By Type** — pie chart and table showing storage breakdown by file extension, with category and group labels
- **By Folder** — horizontal bar chart and table showing storage per top-level subfolder
- **Dead Symlinks** — list of symbolic links pointing to non-existent targets

The toolbar also includes **Scan Now** for the selected folder. Starting a scan from here uses the same cancelable overlay as drag-and-drop or the sidebar `+` button.

### Insights

Select a folder and a trend type:
- **Total Size** — how total storage has changed over time
- **By Extension** — track a specific extension (e.g., `mxf`) over time
- **By Group** — track a group (e.g., "Video") over time

Also shows a scan history table with date, total size, file count, and scan duration.

### Search

Search across all files in watched folders by:
- **Filename or path** — partial match
- **Extension** — exact match (e.g., `mxf`)
- **Size range** — min/max in MB

Results show file name, extension, size, path, modification date, and symlink status.

---

## Menu Bar

Click the **doc.viewfinder** icon in the menu bar to see a dropdown summary:

- **Folder picker** — view breakdown for a specific folder or all folders combined
- **Colored pills** — one row per file group (Video, Audio, Documents, etc.) showing file count, total size, and percentage
- **Stacked bar** — visual proportion of each group
- **Open App** button — bring the main window to front

---

## Settings (Cmd+,)

Access via the app menu: **Find File Types > Settings...**

### File Types Tab

Manage how every file extension is classified. Each row carries four classification axes plus styling:

- **Extension** — the file extension (without dot), e.g., `mxf`
- **Category** — specific structural file-type class, e.g., `professional container`, `lossless audio`, `vector design`
- **Group** — broad family used by the menu bar and the high-level charts: Video, Audio, Images, Documents, Archives, Projects
- **Stage** — where the file sits in the post-production lifecycle: Preproduction, Acquisition, Ingest, Editorial, Graphics, Finishing, Delivery, Archive, Admin
- **Role** — what job the file plays for the user, e.g., `camera original`, `editorial media`, `timeline interchange`, `color pipeline`
- **Color** — used in charts and menu bar pills

Each row also carries hidden `IsLegacy` / `IsNiche` flags that round-trip through CSV export/import — they are not surfaced in the UI in this release. A user-editable tags column is still planned for a future release; until then the flags exist as data only.

Every cell is editable in place. Click the **↺ reset** button next to a row to restore its v1.1 default values — that button only appears when the row has been edited away from the default. Use the **Reset All to Defaults** button at the top to wipe customizations and start over.

Bulk schema changes go through the **Import / Export** menu:
- **Export CSV…** — writes the full 9-column CSV (Extension, Category, Group, Stage, Role, Color, IsLegacy, IsNiche, Notes)
- **Import CSV (merge)…** — adds new extensions and updates existing rows. Columns not present in the CSV are left untouched
- **Import CSV (replace)…** — wipes the table and imports exactly what's in the CSV. Missing columns get neutral defaults

A 4-column v1.0 CSV (Extension, Category, Group, Color) still imports cleanly in merge mode — older fields are preserved.

Filter by **Group** or **Stage** using the pickers in the header to find specific extensions quickly.

### Exclusions Tab

**Include zero-byte files in breakdowns** — off by default. Files that are 0 bytes and not symbolic links are excluded from the type and folder breakdowns so totals reflect actual storage use. Dead symlinks (counted at 0 bytes by design) are always shown. Turn this on if you want to see empty placeholder files in the charts.

**Rebuild Current Data** — deletes all scanned file records, historical snapshots, and last-scan dates, then rescans every watched folder from scratch. It keeps your watched folders, file-type categories, exclusions, alerts, and other settings. Use this when you want to recatalog against updated classification rules or after changing what should be ignored.

**Bundle Exclusions** — extensions listed here are **skipped** during scanning. The scanner will not descend into directories with these extensions.

Defaults: `.fcpbundle`, `.app`, `.photoslibrary`, `.fcpproject`

Add or remove exclusions as needed. Useful for skipping large Final Cut Pro libraries or application bundles.

### Schedules Tab

Configure scan frequency per watched folder:
- **Interval** — Hourly, Daily, or Weekly
- **Preferred time** — for Daily/Weekly, set the hour and minute when the scan should run

The preferred-time menus now show the exact selected hour and minute directly in the control, which makes it easier to confirm the saved schedule at a glance.

Busy windows (configured per folder in the database) defer scanning to outside the window, similar to BackupTrust's busy-window feature.

### Alerts Tab

Define storage threshold rules:
- **Type** — match by extension, category, or group
- **Scope** — apply the rule to all watched folders or to one specific watched folder
- **Value** — the extension/category/group name to watch
- **Metric** — choose whether the threshold is based on total size or file count
- **Threshold** — either size in GB or number of files, depending on the chosen metric

Alerts are currently visual only (shown in the UI). Notification support is planned for a future version.

Offline watched-folder failures also appear here conceptually now: when a volume is unavailable, the app shows a non-blocking banner instead of a modal alert, and you can mute that folder's offline warning for **1 hour**, **1 day**, or **always**. Any folder muted with **always** appears in an override list in Alerts so you can turn warnings back on later.

### About Tab

Shows app version, build number, copyright, and link to [code.matx.ca](https://code.matx.ca). Current release: **1.2 (build 3)** — 

## File Type Categories

The app ships with 100+ pre-configured file types. Each row layers four classification axes — Group, Category, Stage, Role — plus optional legacy/niche flags and notes.

| Group | Example Categories | Example Extensions |
|-------|-------------------|-------------------|
| Video | professional container, camera raw, video container, project file, motion graphics project, delivery video, transport stream, proxy video | mxf, r3d, braw, ari, mov, prproj, aep, mp4, mkv, ts, mts, m2ts, lrv |
| Audio | lossless audio, lossy audio, core audio container | wav, aiff, flac, caf, mp3, aac, m4a |
| Images | camera raw, vector design, page layout, raster image, vector image, high-dynamic-range image, image sequence / scan, font | cr3, nef, arw, dng, psd, ai, indd, jpg, svg, exr, dpx, ttf |
| Documents | document, spreadsheet, tabular data, fixed-layout document, presentation, screenplay, plain text, markup document, email message, web archive, text snippet | docx, xlsx, csv, pdf, pptx, pages, fdx, html, txt, md, eml, url, webarchive, textclipping |
| Archives | archive, disk image, disc image metadata | zip, tar, gz, 7z, dmg, iso, img, sparseimage, disc |
| Projects | library bundle, project file, subtitle, structured data, media hash list, metadata sidecar, edit decision list, media interchange, lut / color transform, archive stub, preview cache | fcpbundle, fcpevent, srt, xml, yml, mhl, xmp, edl, ale, omf, aaf, otio, cube, cdl, lrprev, p5a, p5c, plist |

### Walkthrough: a `.mov` file

A QuickTime container can play several roles depending on where it lives in your workflow. The default for `.mov` is:

- **Group**: Video
- **Category**: video container
- **Stage**: Editorial
- **Role**: editorial media

If you have a folder of camera-original `.mov` files (e.g. from a DSLR), you can edit that one row in Settings to change its Stage to Acquisition and its Role to camera original, without affecting any other file type. Or use a CSV import to apply that change in bulk.

You can add, modify, or remove any of these in **Settings > File Types**. Use **Export CSV…** to share your customizations with teammates.

---

## Symbolic Links

- Symbolic links are **counted as files** but assigned **0 bytes** (since the target file is counted at its real location)
- **Dead symlinks** (pointing to a non-existent target) are flagged with a red warning and listed in the "Dead Symlinks" tab
- This prevents double-counting when symlinks point to files within the same watched folder

---

## Database Location

`~/Library/Application Support/FindFileTypes/findfiletypes.sqlite`

The database uses SQLite with WAL journal mode for safe concurrent reads. Historical snapshots are retained indefinitely. For an in-app refresh, use **Settings > Exclusions > Rebuild Current Data**. To fully wipe everything, quit the app and delete the database file.

---

## Tips

- **Quick check for MXF on cloud drives** — select the cloud folder, go to the "By Type" tab, and look for `.mxf` in the table. The Insights view can show you if MXF usage is growing over time.
- **Find large files** — use Search with a minimum size filter (e.g., 1000 MB) to find files over 1 GB.
- **Dead symlink cleanup** — after migration or reorganization, check the Dead Symlinks tab to find broken links that need fixing.
- **Schedule scans overnight** — set the preferred time to a low-activity hour (e.g., 2:00 AM) to avoid impacting editing workflows.
- **Cancel a mistaken scan quickly** — if you drop a huge network folder by accident, use the centered scan overlay’s **Cancel Scan** button before the crawl finishes.
