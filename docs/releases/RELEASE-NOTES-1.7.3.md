# Find File Type 1.7.3 (build 1)

A more trustworthy retained-data safety dialog, and a sidebar fix.

## The safety dialog now catches scans that hit read errors

A scan on a NAS, LucidLink filespace, or any network-mounted share could run
into a permission problem or a network interruption partway through. Those
errors were silently skipped, so the scan would look complete even though it
undercounted — and if the undercount wasn't large enough to trip the existing
zero-file or major-drop checks, it would merge straight into the catalog,
deleting records for files that were still really there.

The app now counts anything it couldn't read during a scan and always asks
before merging if that happened, even when the drop itself looks small. The
dialog also now says which watched folder it's about, and whether the issue
was a read error, a zero-file result, or a major drop — previously it gave no
way to tell.

A related fix: a folder on a slow network mount can have files briefly report
as 0 bytes before they finish syncing. Those don't count toward storage
totals by design, but they used to also look like missing data to the safety
check. The check now tracks the real file and folder count separately so a
sync hiccup can't be mistaken for data loss.

## Sidebar add/remove buttons stay visible

The **+**/**−** buttons above the sidebar, used to add or remove a watched
folder, could get clipped if the sidebar was dragged narrow. The sidebar's
minimum width is wider now, so they stay visible. The sidebar toggle still
hides the column completely for anyone who wants it gone.

## Requirements

macOS 14.0 or later. Universal binary.
