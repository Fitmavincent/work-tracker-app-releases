# Worklog desktop downloads

This public repository contains the official Windows and macOS preview installers for Worklog, a private local-first desktop work tracker. The application source remains in a separate private repository.

## Download

Download the newest packages from the [latest Worklog release](https://github.com/Fitmavincent/work-tracker-app-releases/releases/latest):

- `Worklog_<version>_x64-setup.exe` — per-user installer for 64-bit Windows.
- `Worklog_<version>_universal.dmg` — universal application for Apple Silicon and Intel Macs.
- Each installer has a matching `.sha256.txt` checksum file.

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

GitHub Actions validates matching npm, Cargo, and Tauri versions; builds Windows and macOS packages; installs or mounts and launches each package in a smoke test; verifies both checksums; and publishes the complete asset set here only after both platform jobs pass.

Worklog stores records on the local device. Export JSON backups regularly because application-local data is not a backup.
