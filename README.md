# nullclaw snap

Snap packaging for [NullClaw](https://github.com/nullclaw/nullclaw) — a lightweight local-first autonomous AI assistant distributed as a static binary.

## Building

```
snapcraft
```

Snapcraft will fetch the latest NullClaw release binary from GitHub and install it into the snap. No build toolchain is required.

## Installing

```
sudo snap install --classic nullclaw_<version>_amd64.snap --dangerous
```

## Usage

```
nullclaw onboard          # first-run setup wizard
nullclaw                  # interactive CLI
nullclaw.lemonade         # pick a local Lemonade model (interactive TUI)
nullclaw.inference-snap   # pick a Canonical inference snap (interactive TUI)
```

### Local AI with Lemonade

`nullclaw.lemonade` detects a running [lemonade-server](https://lemonade-server.ai)
(`http://127.0.0.1:13305`), lists its loaded chat models, and lets you choose one
as NullClaw's primary provider. It writes `~/.nullclaw/config.json` and restarts
the gateway so the change takes effect immediately. Re-run it any time to switch
models. Because NullClaw is a dependency-free static binary, this picker is a pure
POSIX-sh + curl TUI (rather than the Node TUI used by Node-based claw snaps).

### Local AI with Canonical inference snaps

`nullclaw.inference-snap` detects installed [Canonical inference snaps](https://snapcraft.io/search?q=inference)
such as `gemma4`, `gemma3`, `deepseek-r1`, `nemotron-3-nano`, or `qwen-vl`, probes
their OpenAI-compatible API, and lets you choose one as NullClaw's primary provider.
It writes `~/.nullclaw/config.json` and restarts the gateway so the change takes
effect immediately. Re-run it any time to switch models.

```
sudo snap install gemma4
nullclaw.inference-snap
```

The background gateway service is installed and enabled as a systemd user unit the first time any `nullclaw` command is run:

```
systemctl --user status nullclaw
systemctl --user stop nullclaw
systemctl --user start nullclaw
```

## Design notes

**Static binary** — NullClaw ships as a pre-built static binary from GitHub releases (`nullclaw-linux-${ARCH}.bin`). The `nil` plugin is used with a custom `override-pull` that fetches the correct binary for the target architecture.

**Auto provider detection** — On first start, `setup-providers` probes for Lemonade (port 13305) and Ollama (port 11434). If found, it writes `~/.nullclaw/config.json` with the best available model as primary. No cloud account or API key required. For an interactive choice, use `nullclaw.lemonade` instead.

**Classic confinement** — NullClaw performs agentic tasks (file operations, code execution, scheduled jobs) that require broad system access.

## Links

- Upstream project: <https://github.com/nullclaw/nullclaw> (https://nullclaw.io)
- Snap packaging: <https://github.com/kenvandine/nullclaw-snap>
- Report a snap issue: <https://github.com/kenvandine/nullclaw-snap/issues>

## License

NullClaw is licensed under **MIT**. This snap packaging lives in [kenvandine/nullclaw-snap](https://github.com/kenvandine/nullclaw-snap).
