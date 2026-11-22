# AppStore-Hx86

Hashx86 App Store catalog and release assets.

## Repository layout

```text
catalog.json                 Catalog consumed by the Hashx86 App Store
data/quake/icon.bmp           Quake app icon
data/quake/banner.bmp         Quake app banner
.github/workflows/release.yml Publishes the catalog on a version tag
```

The App Store fetches the latest release of this repository, verifies the
`catalog.json` asset with its GitHub-provided SHA-256 digest, then resolves each
app's `asset` against the same release.

## Publishing

1. Update `catalog.json` when app metadata changes.
2. Commit and push the change.
3. Push a version tag, for example:
   ```sh
   git tag v0.1.0
   git push origin v0.1.0
   ```

The release workflow:

- checks out the catalog,
- downloads the latest `Quake.bin` from `Hashx86-OS/Quake-Hx86`,
- validates the catalog and image assets,
- creates a GitHub release named after the tag,
- uploads `catalog.json`, the icon, the banner and `Quake.bin`.

The App Store only reads the latest release. The catalog references files by
asset name; it does not contain the release tag or asset IDs.

## Catalog notes

- `icon` is the built-in icon-font glyph used by the current App Store UI.
- `iconAsset` and `bannerAsset` are release asset names reserved for the
  image-capable UI.
- The Quake banner/icon are not redistributed by the App Store itself; they are
  catalog artwork. Confirm the rights before publishing externally.
- Quake shareware game data is installed by `Quake.bin`, not by this catalog.
