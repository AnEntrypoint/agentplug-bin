# agentplug-bin

Release target for `agentplug-runner`, the native wasmtime host that loads `plugkit.wasm` and its sibling wasm plugins. Do not commit source here.

## What publishes into this repo

[AnEntrypoint/agentplug](https://github.com/AnEntrypoint/agentplug), workflow `.github/workflows/release.yml`. A push to `main` touching `crates/**` or `Cargo.toml` auto-bumps the workspace patch version, builds the six-platform matrix, and publishes a release tagged `v<version>` titled `agentplug-runner v<version>`.

## What lands here

Six platform binaries, each with a `.sha256` sidecar:

- `agentplug-runner-linux-x64`
- `agentplug-runner-linux-arm64`
- `agentplug-runner-macos-x64`
- `agentplug-runner-macos-arm64`
- `agentplug-runner-windows-x64.exe`
- `agentplug-runner-windows-arm64.exe`

## How consumers fetch it

`gm`'s `install.sh`/`install.ps1` download the asset matching the host platform, verify it against the `.sha256` published alongside that same release, and stage it at `~/.gm-tools/agentplug-runner`; a missing or unverifiable asset fails the install loudly rather than leaving no loader. A running `agentplug-runner` also polls this repo on its own (every 600s by default) to self-update its executable, staging the new build and swapping it in via a takeover handoff.
