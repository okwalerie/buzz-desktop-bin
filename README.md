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

- Source is cloned directly from the immutable upstream `block/buzz` release tag selected in the workflow dispatch form.
- The release notes record the upstream tag and commit.
- Each release includes a `SHA256SUMS` file. Verify it before installation.
- Builds are unsigned. Fedora will warn about this for a locally downloaded RPM; that is expected until an RPM signing key is explicitly configured.

## Build workflow

Run **Actions → Build Asahi-native Buzz RPM → Run workflow**. The default upstream source is `desktop-v0.5.5`. The workflow uses GitHub-hosted ARM64 Linux runners and publishes a GitHub Release after a successful RPM build.
