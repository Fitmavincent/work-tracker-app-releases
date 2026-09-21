# Worklog desktop downloads

> Remote execution is paused as of 2026-09-21. Every retained workflow job
> has an unconditional false guard. Build, validation, release publication,
> mirroring and landing-page deployment run locally. Descriptions of optional
> Actions builders below are retained for a future explicitly authorised reactivation.

This public repository contains the official Windows and macOS preview installers for Worklog, a private local-first desktop work tracker. The application source remains in a separate private repository.

## Download

Download the newest packages from the [latest Worklog release](https://github.com/Fitmavincent/work-tracker-app-releases/releases/latest):

- `Worklog_<version>_x64-setup.exe` — per-user installer for 64-bit Windows.
- `Worklog_<version>_universal.dmg` — universal application for Apple Silicon and Intel Macs.
- Each installer has a matching `.sha256.txt` checksum file.

Worklog 0.3.0 is the one-time bootstrap for signed in-app updates. Users on 0.2.0 or earlier must install 0.3.0 manually. From 0.3.0 onward, Worklog checks this repository's latest release and offers **Install and restart** under Settings when a newer version exists.

The remaining release assets support that in-app path:

- the Windows installer `.sig` file;
- a universal macOS `.app.tar.gz` and its `.sig` file; and
- `latest.json`, which maps each supported platform to its signed update package.

The app verifies those signatures before installation. They protect update integrity but are separate from Windows Authenticode and Apple Developer ID publisher trust.

## Preview trust notices

These packages are public preview builds, but they are not yet signed by publicly trusted publisher identities:

- Windows may display **Unknown publisher** or a Microsoft Defender SmartScreen warning.
- macOS may require approval under **System Settings → Privacy & Security** because the application is ad-hoc signed and not yet notarised.

Do not describe the current packages as verified or notarised. Authenticode signing and Apple Developer ID signing/notarisation are planned release-hardening phases.

## Verify a download

On Windows PowerShell:

```powershell
Get-FileHash .\Worklog_<version>_x64-setup.exe -Algorithm SHA256
Get-Content .\Worklog_<version>_x64-setup.exe.sha256.txt
```

On macOS:

```sh
shasum -a 256 -c Worklog_<version>_universal.dmg.sha256.txt
```

## Release process

The manual **Build Worklog desktop release assets** workflow checks out tagged source through a read-only deploy key, validates matching versions and merged-main ancestry, builds and signs Windows/macOS packages, smoke-tests both platforms, and uploads the seven native files plus source provenance. It does not publish or run on a schedule.

An authenticated maintainer uses the private source repository's `npm run release` commands to prepare `latest.json`, validate the exact eight-file payload, upload and byte-verify a draft, publish the public release, verify the stable updater feed, and publish the byte-identical private mirror. Native builds can also run locally. See `docs/local-pipelines.md` in the source repository for the maintained runbook.

Worklog stores records on the local device. Export JSON backups regularly because application-local data is not a backup.
