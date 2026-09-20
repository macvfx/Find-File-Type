# Find File Type 1.7.1 (build 1)

A memory fix for large folders, plus three smaller corrections.

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

## Requirements

macOS 14.0 or later. Universal binary.
