# TweetFlow

TweetFlow packages reusable Twitter/X workflow skills and uses `opencli` as its runtime.

This repository is a shared plugin root for both Codex and Claude Code:

- Codex reads the manifest at `.codex-plugin/plugin.json`
- Claude Code reads the manifest at `.claude-plugin/plugin.json`
- Both runtimes share the same top-level `skills/` and `scripts/` directories

## Runtime model

- TweetFlow does not bundle the `opencli` executable.
- On first use, TweetFlow will auto-install `@jackwener/opencli` if `opencli` is not already available in `PATH`.
- The default private install location is `~/.tweetflow/vendor/opencli`.

## First run

Run:

```bash
./scripts/tweetflow-setup
```

The setup script will:

- auto-install `opencli` if needed
- run `opencli doctor --live`
- report `STATUS: ready`, `STATUS: needs browser bridge`, or `STATUS: needs login`

## Use in Codex

Codex uses the existing plugin manifest at `.codex-plugin/plugin.json`.

The shared skills exposed by this plugin are:

- `tweetflow-setup`
- `twitter-quote-commentary`
- `opencli-twitter-reply-top3`
- `opencli-twitter-reply-top3-english-only`
- `cross-project-adapter-migration`

## Use in Claude Code

Claude Code uses the plugin manifest at `.claude-plugin/plugin.json`.

From the repo root, load the plugin locally with:

```bash
claude --plugin-dir .
```

From the parent directory of this repo, run:

```bash
claude --plugin-dir ./TweetFlow
```

Or run with an absolute path:

```bash
claude --plugin-dir /Users/webbergao/work/src/TweetFlow
```

Claude exposes the shared skills through the `tweetflow` namespace:

- `/tweetflow:tweetflow-setup`
- `/tweetflow:twitter-quote-commentary`
- `/tweetflow:opencli-twitter-reply-top3`
- `/tweetflow:opencli-twitter-reply-top3-english-only`
- `/tweetflow:cross-project-adapter-migration`

This repo intentionally uses plugin-only exposure for Claude Code. It does not duplicate skills under `.claude/skills` or `.claude/commands`.

## Browser requirements

Browser-backed Twitter/X actions still require:

- Chrome running locally
- the opencli Browser Bridge extension installed and connected
- an authenticated `x.com` session in Chrome

TweetFlow only auto-installs `opencli`. It does not install the browser extension or sign the user into X/Twitter.

## Environment overrides

- `TWEETFLOW_OPENCLI_BIN`
  Use an existing `opencli` binary instead of auto-installing one.

- `TWEETFLOW_OPENCLI_HOME`
  Change the private install directory used for `@jackwener/opencli`.
