# FontSource

A versioned public source for reviewed, redistributable font files used by TrePDF and other Tre applications.

## Layout

```text
catalog.json
fonts/<family-key>/<version>/
  manifest.json
  <font files>
  LICENSE or LICENSE.md
  AUTHORS
```

Each family version is self-contained. Its manifest records the exact upstream release or mirror archive, license, face metadata, byte size, SHA-256, and declared coverage. Font bytes are kept unchanged after extraction from the upstream package.

## Included families

- **Hack 3.003** — Regular, Italic, Bold, Bold Italic. The release is co-licensed under the Hack MIT terms and the Bitstream Vera License. Hack Regular does not contain eight less-common Vietnamese precomposed glyphs; its manifest marks Vietnamese coverage as partial.
- **Iosevka 34.9.1** — the default Normal-width static TrueType Regular, Italic, Bold, and Bold Italic faces. Other widths, weights, oblique faces, and variants remain available upstream but are intentionally not bundled in this family entry.
- **Liberation Sans 2.1.5** — Regular, Italic, Bold, Bold Italic.
- **Liberation Serif 2.1.5** — Regular, Italic, Bold, Bold Italic.
- **Liberation Mono 2.1.5** — Regular, Italic, Bold, Bold Italic.
- **Sarasa Gothic CL 1.0.43** — the Classical orthography (`CL`) Gothic variant, with static TrueType Regular, Italic, Bold, and Bold Italic faces. It includes CJK glyphs while keeping each face below TrePDF's 64 MiB per-face limit.

All three newly added families use static TrueType files and stay within TrePDF's format and per-face size constraints. Coverage is recorded per family in the manifest; Hack is intentionally marked partial for Vietnamese. No TTC/OTC, variable, CFF2, or color-font files are included.

## Adding another family or version

1. Create `fonts/<family-key>/<version>/` using the exact public family name from the font metadata.
2. Add unchanged static font files and the applicable license/author notices.
3. Add a `manifest.json` with exact face metadata, archive provenance, and SHA-256 values.
4. Add the family/version entry to `catalog.json`.
5. Validate every face: static TrueType/OpenType format, family/style metadata, file size, ASCII coverage, required language coverage, and checksum.

A TrePDF provider should construct download URLs from a pinned release or commit, verify the manifest SHA-256 before installation, and install a complete family atomically. Do not use mutable `main`/`latest` URLs in a production provider. A mirror URL may be recorded as the download URL only when its bytes are checked against the pinned upstream archive digest.
