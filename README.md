# company.inc — releases

Public release artifacts for the **company.inc** macOS app.

The app's source code is private (`manurueda/happy-companyinc`). This repo holds only the user-facing distribution artifacts:

- **`appcast.xml`** — the Sparkle update feed. Sparkle clients embedded in installed copies of the app fetch this URL on a schedule (currently hourly) to discover new versions.
- **GitHub Releases** — DMG downloads, attached as release assets per version (`v1.0.0`, `v1.0.1`, ...). Each DMG is notarized by Apple and Sparkle-signed.

## URLs

- **Sparkle feed (read by the app)**: <https://raw.githubusercontent.com/manurueda/companyinc-releases/main/appcast.xml>
- **DMG downloads (latest)**: <https://github.com/manurueda/companyinc-releases/releases/latest>
- **Per-version DMG**: `https://github.com/manurueda/companyinc-releases/releases/download/v<version>/companyinc-<version>.dmg`

## Pipeline

Releases are produced automatically by [`.github/workflows/release.yml`](https://github.com/manurueda/happy-companyinc/blob/main/.github/workflows/release.yml) in the source repo, triggered by `git tag v<version>`. The workflow:

1. Builds the macOS app from source.
2. Signs with the **Developer ID Application** cert.
3. Notarizes via Apple's notary service and staples the ticket.
4. Wraps in a DMG, signs the DMG.
5. Generates an Ed25519 signature for Sparkle.
6. Pushes the DMG as a release asset here.
7. Prepends a new `<item>` to `appcast.xml` here.

Installed copies of the app discover the new version on the next Sparkle poll and silently install it on next launch.

## Don't push directly

Don't manually push to this repo. Cut a release by tagging in `manurueda/happy-companyinc`:

```sh
git tag v1.0.1
git push origin v1.0.1
```
