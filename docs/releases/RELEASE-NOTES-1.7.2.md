# Find File Type 1.7.2 (build 3)

Fourteen more file types are recognized out of the box, several of them
reported by users whose scans turned up extensions the app didn't know yet.

## Newly recognized extensions

- **`.itt`** — Apple iTunes Timed Text subtitle/caption, used for iTunes
  Store and Apple TV delivery and also imported/exported by Final Cut Pro.
- **`.ttc`** — TrueType Collection font bundle. Apps such as InDesign may
  need the fonts converted to standalone `.ttf` first.
- **`.aet`** — Adobe After Effects project template (the binary variant of
  `.aep`).
- **`.avif`** — AV1 Image File Format, a modern high-efficiency still
  image codec.
- **`.glb`** — binary glTF 3D model asset.
- **`.dotx`** — Microsoft Word template.
- **`.acsm`** / **`.acsi`** — a proprietary security camera recording
  export and its paired index file, likely from Axis Communications
  camera/companion software. (`.acsm` is also, separately, a long-standing
  Adobe ebook-DRM format elsewhere — the two share a name by coincidence.)
- **`.fileslack`** — a recovered-data fragment. Seen as a compound suffix
  appended after an existing extension by a forensic/data-recovery tool
  marking a fragment pulled from filesystem slack space.
- **`.nupkg`** — a NuGet software package, also used as the installer/
  update package format for Squirrel-based Windows installers such as the
  Axis Companion installer.
- **`.zop`** — a plain ZIP archive under another name; renaming a sample
  to `.zip` expands it normally.
- **`.err`** — a generic application error log.
- **`.manifest`** — a generic manifest file; its exact meaning depends on
  the app that wrote it.
- **`.fcpcopy`** — a transient file Final Cut Pro writes while copying
  media into a library.

Existing custom classifications are preserved.

## Requirements

macOS 14.0 or later. Universal binary.
