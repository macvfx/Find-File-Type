# Find File Type 1.7.6 (build 1)

76 more file types are recognised, and the "Sort" label in the Uncategorized
tab is fixed.

## What's new

- **Apple Encrypted Archives (`.aea`):** filed under Archives, with a note
  that they cannot be opened or previewed without their key or password.
- **Source code:** C, Go, Ruby, PHP, SQL, JavaScript modules, notebooks,
  patches and similar files are grouped as source code.
- **Xcode project files:** `.pbxproj`, `.xcscheme`, `.xcconfig`,
  `.entitlements` and others share one role, *Xcode project file*, so they are
  easy to tell apart from the temporary build files added in 1.7.5. The window
  state Xcode saves (`.xcuserstate`) is personal and safe to delete.
- **Backup copies:** `.bak`, `.backup` and suffixes such as `.json_backup` and
  `.log_backup` share one category, *backup copy*. Once you have confirmed the
  original is intact, these can usually go.
- **Crash and hang reports:** `.diag`, `.hang` and `.ips` are labelled as
  diagnostic reports. A single hang report can be tens of megabytes.
- **Settings and process files:** `.cfg`, `.ini`, `.env`, `.pid` and similar.
  Notes warn that `.env` files can contain secrets.
- **Other:** equipment rack plans, mind maps, comic documents, network
  settings profiles, Wallet passes, diagram sources, and a few installer and
  database support files.

## Sort label fixed

In the Uncategorized tab, the word "Sort" was squeezed into one letter per
line when the window was narrow. It now stays on one line next to the control.

## Notes

Existing databases pick up the new types the next time the app starts. Any
category you edited yourself is left as you set it. Git version-tag names such
as `.0-dev` were deliberately left out, because they only look like extensions.

## Requirements

macOS 14.0 or later. Universal binary.
