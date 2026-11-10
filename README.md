# Valise — releases

Downloads for **Valise**, an archive utility for macOS built on a port of
[NanaZip](https://github.com/M2Team/NanaZip) (the 7-Zip derivative for modern
Windows). Open, browse, preview, extract, create and edit 7z, zip, tar, wim and
dozens of other formats, with Quick Look and Finder integration.

**[→ Download the latest release](../../releases/latest)**

Requires macOS 14 or later. Native on Apple silicon and Intel Macs.

This repository holds release builds only. The app's own source repository is
private. The `patches/` folder here carries the changes made to the upstream
engine (see [Source for the engine](#source-for-the-engine)).

## Verifying what you downloaded

Every build is signed with an Apple Developer ID and notarized by Apple. macOS
checks both on first launch. To check for yourself, before running it:

```bash
codesign --verify --deep --strict --verbose=2 Valise.app
spctl --assess --type execute --verbose Valise.app
codesign -dvvv Valise.app 2>&1 | grep Authority
```

The signing identity must read exactly:

```
Developer ID Application: Toma Vasilescu (ZGUFL4JQHW)
```

If it names anyone else, or if `spctl` does not say `accepted`, do not run it.
Each release's notes also list the SHA-256 of the disk image.

## What this does *not* prove

**You cannot audit the app.** Its source is private. The signature proves who
produced it; it says nothing about what the code does. If that is not a trade
you want to make, use [NanaZip](https://github.com/M2Team/NanaZip) on Windows or
[7-Zip](https://www.7-zip.org) / [Keka](https://www.keka.io) on macOS.

**There is no build attestation.** Attestation would bind to a private
repository nobody can read, implying more verification than exists. The
Developer ID signature and Apple's notarization are the trust anchor, and both
are checkable above.

## Source for the engine

Valise's archive engine is NanaZip's, which includes 7-Zip code under the GNU
LGPL (with the unRAR restriction). The corresponding source is:

- **NanaZip**, commit
  [`1f850e0`](https://github.com/M2Team/NanaZip/tree/1f850e0fc37cc652c4639901a92593ae165607bf)
  (NanaZip 7.0 preview, based on 7-Zip 26.04).
- **[`patches/`](patches)** — the changes Valise applies on top of it so it
  builds and runs on macOS. Apply them in order with `git apply` to that
  NanaZip commit.
- **7-Zip 26.04** from [7-zip.org](https://www.7-zip.org), which supplies the
  Unix-only files NanaZip's Windows tree leaves out.

The macOS app, the Objective-C++ bridge and the build shims are not part of the
LGPL engine and are not published.

## Credits and licenses

Valise is not affiliated with M2-Team or Igor Pavlov. NanaZip is MIT licensed;
7-Zip is LGPL with the unRAR restriction; bundled third-party libraries keep
their own licenses. The full notices are in Valise's Settings › About. This
README is under the MIT [LICENSE](LICENSE). The patches in
[`patches/`](patches) modify NanaZip and 7-Zip files and stay under those
files' licenses.
