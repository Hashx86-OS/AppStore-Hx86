# AppStore-Hx86

Hashx86 App Store catalog and JPEG artwork release.

## Repository layout

```text
catalog.json                 Catalog consumed by the Hashx86 App Store
data/<sha256>.jpg            App icons and banners, content-addressed
.github/workflows/release.yml Publishes the catalog on a version tag
```

The App Store fetches the latest release of this repository, verifies the
`catalog.json` asset with its GitHub-provided SHA-256 digest, then displays the
apps listed there. Each app entry names a `repo`; the App Store fetches that
repository's latest release to download the application binary. This
repository's releases contain only `catalog.json` and JPEG artwork, never app
binaries.

## Artwork naming

Artwork files are named by their content hash:

```text
<sha256-of-file>.jpg
```

All artwork lives directly under `data/`; there are no per-app folders. The
release workflow rejects any non-`.jpg` file under `data/`, and rejects a
`.jpg` whose filename does not match its SHA-256. This lets a catalog release
hold artwork for many apps without `icon.jpg`/`banner.jpg` name collisions.
Only JPEG is supported; PNG and BMP are intentionally not accepted.

## Publishing

1. Update `catalog.json` and the hashed JPEG files when app metadata or artwork
   changes.
2. Commit and push the change.
3. Push a version tag, for example:
   ```sh
   git tag v0.1.1
   git push origin v0.1.1
   ```

The release workflow:

- checks out the catalog,
- validates `catalog.json`,
- rejects non-JPEG files and mismatched content-hashed names,
- checks that every `iconAsset` and `bannerAsset` referenced by the catalog is
  present,
- creates a GitHub release named after the tag,
- uploads `catalog.json` and the JPEG artwork.

The App Store only reads the latest release. The catalog references artwork by
asset name; app binaries are resolved from each app's own repository.

## Catalog notes

- `icon` is the built-in icon-font fallback.
- `iconAsset` and `bannerAsset` are content-hashed JPEG release assets displayed
  by the App Store through the ImageView GUI widget.
- `repo` is the GitHub repository whose latest release supplies the app binary.
  `asset` names the file to pick from that release.
- The Quake and Quake II banners/icons are not redistributed by the App Store
  itself; they are catalog artwork. Confirm the rights before publishing
  externally.
- Quake and Quake II shareware game data are installed by their application
  binaries, not by this catalog.
