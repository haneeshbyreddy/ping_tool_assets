# ping_tool_assets

Public installers for the HansaNet / WISP network monitor. Binaries live in
[GitHub Releases](../../releases); this repo's tree holds only the install scripts
and this README.

Releases are tagged by component and version:

- `edge-vX.Y.Z` — the edge probe (Windows and Linux)
- `field-app-vX.Y.Z` — the Android field app

Every release includes a `manifest.json` with the sha256 of each artifact.

Edge releases are built here, by this repo's `edge-release` workflow, from a tag of
the private source repo, and published as a draft that flips to public only once every
platform's binary and the manifest are in place. A release with a `-` in the asset list
or no `manifest.json` is one that is still building.

## Windows edge

1. Download `wisp-edge-setup-win-amd64.exe` from the latest `edge-v*` release.
2. Run the installer. It asks for the central URL, org, node name, and enrollment
   token — get these from your central dashboard.
3. The edge runs as a background service and starts on boot.

## Linux edge

Download the `.deb` for your architecture (`amd64` or `arm64`) from the latest
`edge-v*` release, then:

```sh
sudo apt install ./wisp-edge-linux-amd64.deb   # or arm64
```

Configure `/etc/wisp/edge.env` with your central URL, org, node name, and token,
then `sudo systemctl restart wisp-edge`.

Alternatively, install from source with the scripts in `deploy/`:

```sh
sudo ./deploy/install-edge-src.sh --central https://<your-central> --token <token> --org <org> --node <name>
```

(`install-edge-src.ps1` is the Windows equivalent.)

## Android field app

1. Download `wisp-field.apk` and `manifest.json` from the latest `field-app-v*` release.
2. Verify the download before installing:

   ```sh
   sha256sum wisp-field.apk
   ```

   The hash must equal the `sha256` value in that release's `manifest.json`.
3. Copy the APK to the phone and open it. Allow "install from unknown sources"
   for your browser or file manager when prompted.
4. Sign in with the credentials from your central dashboard.

## Verifying any download

All artifacts can be checked the same way: compare `sha256sum <file>` against the
matching entry in the release's `manifest.json`.
