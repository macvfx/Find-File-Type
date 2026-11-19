# Find File Type 1.7.5 (build 1)

65 more file types are recognised, so less of a scan lands in Uncategorized.

## What's new

- **Scripts:** `applescript`, `bash`, `zsh`, `csh`, `fish`, `command`, `bat`,
  `ps1` and `bats`, filed with `sh` and `scpt`.
- **Disk image:** `asif`, the Apple sparse disk image format introduced with
  macOS 26, filed with `dmg`, `iso` and `sparseimage`.
- **Documents:** `odt`, `epub`, `markdown`, `mobileconfig`, `webloc`, `vncloc`,
  `text`, `whisper` and `skill`.
- **Installer projects, archives and git data:** `pkgproj`, `pmproj`, `cpgz`,
  and the git pack files `pack`, `idx`, `rev`, `mtimes` and `graph`.
- **Media-adjacent:** `preset`, `icns`, `car` and `stl`, plus one archive container type.
- **Certificate, key and license files:** `cer`, `csr`, `scsr`, `p12`, `der`,
  `p7m`, `rkey` and similar license, backup and export files. Notes mark the
  ones that hold a private key, a recovery key or an unencrypted password
  export, which should not sit in shared folders.
- **Build intermediates:** 20 compiler and build-system products share one
  *build intermediate* category and the role *regenerable build output*, so
  folders that can be deleted and rebuilt are easy to find. Static and dynamic
  libraries (`a`, `dylib`) are labelled separately because they can also be
  shipped dependencies.

## Notes

Existing databases pick up the new types on launch. Rows you have edited are
left alone. The producer of a few types (`whisper`, `preset` and one backup type) is not
confirmed, and their notes say so.

## Requirements

macOS 14.0 or later. Universal binary.
