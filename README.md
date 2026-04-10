# TweetFlow

TweetFlow is a plugin repository for Codex and Claude Code that helps automate Twitter/X workflows. It turns repetitive tasks such as checking runtime health, scanning the timeline, selecting engagement targets, drafting replies, and posting quote-style commentary into reusable skills.

It runs on top of `opencli` and uses the wrapper scripts in this repository to standardize browser-backed actions. On first use, it can auto-install `@jackwener/opencli`, making it practical to manage Twitter/X activity with a repeatable workflow instead of manual setup every time.

## Project Overview

TweetFlow is designed to make automated Twitter/X management more practical across a few common tasks:

- Automatically check whether the local browser automation environment is ready
- Read the timeline and identify better tweets to engage with
- Draft replies that feel specific and human rather than templated
- Turn a single tweet into a short quote-style post with a clear point of view
- Reuse the same plugin and skills in both Codex and Claude Code

The main implementation lives under `plugins/tweetflow/`:

- `scripts/` contains the `tweetflow-setup` and `tweetflow-opencli` wrapper scripts
- `skills/` contains the reusable Twitter/X workflow skills
- `.codex-plugin/` and `.claude-plugin/` provide the plugin entrypoints for the two runtimes

Repository-specific runtime split:

- `.agents/plugins/marketplace.json` is the Codex marketplace entry for this repo
- `plugins/tweetflow` is the Claude plugin root
- `plugins/tweetflow/skills/` and `plugins/tweetflow/scripts/` are shared by both runtimes

Important note:

- `.agents/plugins/marketplace.json` is for Codex only
- Claude should load the plugin directory directly with `--plugin-dir`

## Core Skills

The main TweetFlow skills are:

| Skill | What it does | Best used for |
| --- | --- | --- |
| `tweetflow-setup` | Installs or checks `opencli`, then verifies Browser Bridge, Chrome, and X login readiness | First-time setup on a new machine, or when browser automation starts failing |
| `tweet-reply-top3` | Reads the latest 50 timeline tweets, picks the best 3 engagement opportunities, drafts replies, and selects 1 tweet for quote-style commentary | Maintaining account activity and improving engagement efficiency |
| `tweet-reply-top3-english-only` | Same workflow as `tweet-reply-top3`, but restricted to English-language engagement | English-only account workflows |
| `tweet-quote-commentary` | Reads a specific tweet URL, extracts the source idea, and drafts a short quote-style commentary with a clear judgment | Reacting to a single tweet with a concise, opinionated take |
| `cross-project-adapter-migration` | Imports commands from an external CLI project into OpenCLI adapters | Adapter migration work across projects |

## First Run

From the repository root:

```bash
./plugins/tweetflow/scripts/tweetflow-setup
```

This script will:

- auto-install `opencli` if it is missing
- run `opencli doctor --live`
- report statuses such as `ready`, `needs browser bridge`, or `needs login`

Before running browser-backed actions, make sure:

- Chrome is running locally
- the `opencli` Browser Bridge extension is installed and connected
- the target `x.com` account is already logged in within Chrome

## Use in Codex

Codex uses the bundled marketplace file at `.agents/plugins/marketplace.json`.

Then enable the plugin in `~/.codex/config.toml`:

```toml
[plugins."tweetflow@tweetflow-local"]
enabled = true
```

Where:

- the plugin name comes from `plugins/tweetflow/.codex-plugin/plugin.json` as `tweetflow`
- the marketplace name comes from `.agents/plugins/marketplace.json` as `tweetflow-local`

Once enabled, you can ask Codex to run TweetFlow workflows directly, for example:

- `Run TweetFlow setup and tell me what is missing.`
- `Review 50 timeline tweets and draft the top 3 replies.`
- `Read a tweet URL, write a short judgment, and post it.`

## Use in Claude Code

Claude Code should load the plugin directory, not the repository root:

```bash
claude --plugin-dir ./plugins/tweetflow
```

You can also load it with an absolute path:

```bash
claude --plugin-dir /Users/webbergao/work/src/TweetFlow/plugins/tweetflow
```

After loading, TweetFlow skills are available through the `tweetflow` namespace:

- `/tweetflow:tweetflow-setup`
- `/tweetflow:tweet-quote-commentary`
- `/tweetflow:tweet-reply-top3`
- `/tweetflow:tweet-reply-top3-english-only`
- `/tweetflow:cross-project-adapter-migration`

Codex and Claude Code share the same `plugins/tweetflow/skills/` and `plugins/tweetflow/scripts/`, so the workflow logic is largely the same across both runtimes. The main difference is how the plugin is loaded.

## Workflow Summary

A typical TweetFlow run looks like this:

1. Run `tweetflow-setup` to confirm that `opencli`, Browser Bridge, Chrome, and the X login session are ready.
2. Choose a skill based on the task.
3. Let the skill read the timeline or target tweet and extract context.
4. Draft replies or quote-style commentary that are short, specific, and opinionated.
5. Publish replies, likes, or quote-style posts through `tweetflow-opencli` once the content is ready.

## Runtime Notes

- Default private install directory: `~/.tweetflow/vendor/opencli`
- Use `TWEETFLOW_OPENCLI_BIN` to point to an existing `opencli` binary
- Use `TWEETFLOW_OPENCLI_HOME` to change the private install directory
