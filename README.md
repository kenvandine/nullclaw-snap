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
nullclaw onboard     # first-run setup wizard
nullclaw            # interactive CLI
```

The background gateway service is installed and enabled as a systemd user unit the first time any `nullclaw` command is run:

```
systemctl --user status nullclaw
systemctl --user stop nullclaw
systemctl --user start nullclaw
```

## Design notes

**Static binary** — NullClaw ships as a pre-built static binary from GitHub releases (`nullclaw-linux-${ARCH}.bin`). The `nil` plugin is used with a custom `override-pull` that fetches the correct binary for the target architecture.

**Auto provider detection** — On first start, `setup-providers` probes for Lemonade (port 8000) and Ollama (port 11434). If found, it writes `~/.nullclaw/config.json` with the best available model as primary. No cloud account or API key required.

**Classic confinement** — NullClaw performs agentic tasks (file operations, code execution, scheduled jobs) that require broad system access.
