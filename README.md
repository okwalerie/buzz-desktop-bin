# Buzz Desktop for Fedora Asahi

This repository produces a native **aarch64 RPM** of [Block Buzz](https://github.com/block/buzz) Desktop for Fedora Asahi Linux.

## Why an RPM?

The upstream Linux AppImage is not usable on Fedora Asahi's 16 KiB page-size systems: its bundled native runtime aborts before the application starts. This workflow builds Tauri's native RPM bundle on an ARM64 Linux runner instead. It deliberately does **not** publish an AppImage.

## Install

Download `Buzz-*-1.aarch64.rpm` from the [latest release](../../releases/latest), then layer it on Fedora Atomic/Asahi:

```bash
rpm-ostree install ./Buzz-*-1.aarch64.rpm
systemctl reboot
```

Rollback the deployment if required:

```bash
rpm-ostree rollback
systemctl reboot
```

For a mutable Fedora installation, use:

```bash
sudo dnf install ./Buzz-*-1.aarch64.rpm
```

## Provenance and trust

- Source is fetched directly from the selected upstream `block/buzz` release tag or full commit SHA, then checked out detached. The exact source commit is recorded.
- The release notes record the upstream tag and commit.
- Each release includes a `SHA256SUMS` file. Verify it before installation.
- Builds are unsigned. Fedora will warn about this for a locally downloaded RPM; that is expected until an RPM signing key is explicitly configured.

## Build workflow

Run **Actions → Build Asahi-native Buzz RPM → Run workflow**. Leave `upstream_ref` blank to build the latest stable Desktop release, or enter an explicit `desktop-vX.Y.Z` tag or full commit SHA to rebuild a specific source.

The weekly scheduled build also discovers the latest stable Desktop release. Discovery checks all upstream release pages, excludes drafts, prereleases, and non-Desktop releases, and chooses the highest numeric version. If discovery fails or finds no matching release, the workflow fails rather than rebuilding an old fallback.

The workflow uses GitHub-hosted ARM64 Linux runners and publishes a GitHub Release after a successful RPM build. Each release records the resolved upstream ref and exact source commit in its notes and `BUILD-PROVENANCE.txt`.

After publishing, it keeps only the three newest community RPM releases by publication time and removes older releases and their assets. Rebuilds count as separate releases. It leaves Git tags and unrelated releases intact. Publishing and pruning share one concurrency group so runs cannot race.
