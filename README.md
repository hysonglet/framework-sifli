# framework-sifli-sdk (private PlatformIO framework wrapper)

This is a **thin, private** wrapper repository that makes the official
[SiFli-SDK](https://github.com/OpenSiFli/SiFli-SDK) installable as a PlatformIO
`framework-*` package, so that `pio run` can auto-provision the SDK.

## Why this repo exists

PlatformIO's VCS package install requires a `package.json` at the **repository
root**. The official SiFli-SDK does not have one at its root, which makes
PlatformIO mis-resolve it (it descends and installs a subdirectory such as the
`network_samples` RT-Thread package). This wrapper fixes that by providing the
root `package.json` and referencing the real SDK as a git **submodule**, keeping
the SDK itself pristine and tracking upstream.

## Layout

```
framework-sifli-sdk/
├── package.json      # PlatformIO framework package manifest (root)
├── .gitmodules       # points `sifli-sdk` at the official SDK (release/v2.4)
└── sifli-sdk/        # <-- git submodule: the official SiFli SDK
```

The `sifli-platform` build script resolves the SDK at
`<package_dir>/sifli-sdk` (or `<package_dir>` if the SDK is at the root).

## Updating the SDK

To track a newer upstream SDK, update the submodule:

```bash
git submodule update --remote sifli-sdk
git add sifli-sdk
git commit -m "chore: bump SiFli SDK to <new-ref>"
git push
```

## Usage

Reference this repo from `sifli-platform/platform.json`:

```json
"framework-sifli-sdk": {
  "type": "framework",
  "version": "git+https://<token>@github.com/<you>/framework-sifli-sdk.git#main"
}
```
