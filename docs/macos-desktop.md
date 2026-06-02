# macOS Desktop Wrapper

This is an optional macOS-specific wrapper around the existing Odysseus web app.
It is a separate distribution path and does not replace or change the primary
Docker/native quick-start flows in the main README.

## Scope

- Backend behavior is unchanged. The wrapper starts the existing local Odysseus
  services and loads `http://127.0.0.1:7001` inside a native `WKWebView` window.
- Browser access remains supported and is the default documented path.
- This wrapper is intended for local use and development. It is not a signed or
  notarized distributable at this stage.

## Prerequisites

- macOS 13 or newer
- Xcode Command Line Tools / Swift toolchain available
- A working local Odysseus checkout with:
  - `venv` created
  - Python dependencies installed
  - `chromadb` installed in the virtualenv
  - `python setup.py` already run

## Build

```bash
cd odysseus
./scripts/build-macos-app-bundle.sh
open ./dist/Odysseus.app
```

To stop background services started by the wrapper:

```bash
./scripts/stop-odysseus-desktop.sh
```

## Repo Path Behavior

- The app stores the last working repo path in
  `~/Library/Application Support/OdysseusDesktop/repo_path.txt`.
- If the repo moves, use `Control -> Choose Repo Folder...` in the app.
- Rebuilding the bundle is not required for normal repo moves.

## Support Matrix

- Apple Silicon: supported and tested on an arm64 Mac.
- Intel: intended to be supported when built on an Intel Mac with the same
  toolchain, but should be explicitly verified before claiming parity.

## Build / Test Checklist

### Apple Silicon

Run:

```bash
./scripts/build-macos-app-bundle.sh
open ./dist/Odysseus.app
```

Verify:

- The app launches into a native macOS window.
- The Dock icon matches the Odysseus web favicon.
- `Cmd+C`, `Cmd+V`, `Cmd+X`, and `Cmd+A` work in text fields.
- `Control -> Reload`, `Open in Browser`, `Restart Backend`, and `Stop Backend`
  work as expected.
- The app starts Odysseus and ChromaDB and loads the login page successfully.
- Browser-based access to the same local instance still works.
- Moving the repo can be recovered with `Control -> Choose Repo Folder...`.

### Intel

Run on an Intel Mac:

```bash
./scripts/build-macos-app-bundle.sh
open ./dist/Odysseus.app
```

Verify the same items as Apple Silicon, plus:

- `dist/Odysseus.app/Contents/MacOS/Odysseus` is an `x86_64` Mach-O binary when
  built on Intel.
- No architecture-specific issues appear when starting `WKWebView`, Swift
  runtime, or the backend helper scripts.

## Notes for Reviewers

- This wrapper intentionally does not change the documented Docker quick start.
- This wrapper intentionally does not change the documented native browser-first
  install flow.
- The desktop path is isolated to `desktop-macos/`, helper scripts, icon asset,
  and wrapper documentation.
