# Find File Type 1.7.1 (build 2)

A memory fix for large folders, plus smaller corrections.

## Scans no longer run out of memory

Scanning a very large folder could exhaust application memory and have the app
killed by the system before the scan finished — most visibly when watching the
top level of a big network share.

The scan used to buffer every file it found in memory and write the whole list
to the database only at the end, which cost roughly 1 KB per file: around 5 GB
on a five-million-file share. Files are now streamed to the database in batches
as they are found, so peak memory is set by the batch size rather than by the
size of the folder. Measured on a 71,000-file tree: 67 MB before, 5 MB after,
and the figure stays flat as the folder grows.

Two related changes come with it. The scan loop now releases each file's
temporary system objects as it goes instead of holding them all until the scan
ends. And committing a finished scan runs off the main thread, so the app stays
responsive while a large catalog is written rather than appearing to hang.

Scan results are staged separately and merged into the catalog only once a scan
is accepted. The retained-data safety review is unchanged: choosing **Keep
Retained Data** discards the staged rows, and the live catalog is still never
touched until **Wipe and Rebuild** is confirmed. Rows left by a scan that never
finished are discarded the next time the app launches.

## Search matches names containing quotes

A filename search containing an apostrophe or quotation mark returned nothing
instead of matching, because the search text was being spliced into the database
query. Search text is now passed as a value. `%` and `_` in a search are matched
literally, so searching for `report_2026` no longer also matches `report-2026`.

## Accurate paths in mirrored trees

A file whose path contained the watched folder's full path a second time had
that second copy stripped out, and the surrounding separators with it. In a
backup or proxy tree mirroring the volume layout, a clip recorded under
`backup/Volumes/Jobs/clip.mov` was stored as `backupclip.mov`. Only the leading
path is removed now.

## Bounded file lists

The dead-symlink and file lists loaded an entire directory into memory. Both now
show the first 1,000 rows, report the true total, and say when there is more.
CSV export still writes the complete list.

## Fixed in build 2

The scanning banner along the bottom of the window flashed on and off every few
seconds. A scheduled scan worked out its next run by re-reading the folder's
stored last-scan date straight after starting the scan — but that date is
written only when a scan finishes, so the folder still looked overdue and the
next run was set 15 seconds away. A folder whose scan can never record a date,
such as one on a volume that is no longer mounted, retried forever, and because
those scans fail immediately the banner appeared and vanished each time. If the
volume's offline notice was set to ignore always, the flash was the only sign
anything was happening. The next run is now counted from when the scan started.

The **View** label above the folder tabs wrapped mid-word to "Vie / w", because
it shared its width with the segmented control beside it.

## Requirements

macOS 14.0 or later. Universal binary.
