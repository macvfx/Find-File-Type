# Find File Type 1.7.5 (build 1)

65 more file types are recognised, so fewer files land in Uncategorized.

## What's new

- **Scripts:** AppleScript, bash, zsh, fish and other shell scripts, plus
  Windows batch and PowerShell scripts, now sit with the existing shell and
  compiled-script types.
- **Apple sparse disk image (`.asif`):** the new disk image format from
  macOS 26, filed with `.dmg`, `.iso` and `.sparseimage`.
- **Documents:** OpenDocument text (`.odt`), e-books (`.epub`), `.markdown`,
  configuration profiles, web and remote-screen shortcuts, plain `.text`
  files, transcripts and skill packages.
- **Installer projects and archives:** installer project files, compressed
  archives, and the files git uses to store a project's history.
- **Certificate, key and license files:** `.cer`, `.csr`, `.p12`, `.der`,
  `.p7m`, `.rkey` and similar. Notes mark the ones that hold a private key, a
  recovery key or an unencrypted password export, which should not sit in
  shared folders.

## Xcode build files get their own category

When Xcode builds an app it leaves behind a lot of temporary files, such as
`.o`, `.d`, `.swiftdeps` and `.sdkstatcache`. On a Mac used for development
they can add up to gigabytes. They used to show up as Uncategorized. Now 20 of
them share one category, *build intermediate*, with the role *regenerable
build output*.

Filter on that role to find old build folders. They are safe to delete,
because Xcode recreates them the next time you build.

Libraries (`.a` and `.dylib`) are labelled separately. Some of them are real
dependencies that ship with an app, so they are not always leftovers.

## Notes

Existing databases pick up the new types the next time the app starts. Any
category you edited yourself is left as you set it. A few types (`.whisper`,
`.preset` and one backup type) are best guesses, and their notes say so.

## Requirements

macOS 14.0 or later. Universal binary.
