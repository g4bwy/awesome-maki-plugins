# Awesome Maki Plugins

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated list of community Lua plugins for [Maki](https://github.com/tontinton/maki), an AI coding agent that you can extend without touching its Rust core.

Every plugin here was shared by its author in the [Plugin showcase discussion #452](https://github.com/tontinton/maki/discussions/452). Most are a single `init.lua` file plus a `plugin.toml`. You can install a whole git bundle with `maki.pack.add`, or copy one file into `~/.config/maki/lua/`. See the [plugin docs](https://maki.sh/docs/plugins/) and the [package docs](https://maki.sh/docs/packages/) for details.

## Contents

- [Sessions and context](#sessions-and-context)
- [Agent behavior and autonomy](#agent-behavior-and-autonomy)
- [Web search and fetch](#web-search-and-fetch)
- [Providers and auth](#providers-and-auth)
- [Review and formatting](#review-and-formatting)
- [Safety and permissions](#safety-and-permissions)
- [UI and editor integration](#ui-and-editor-integration)
- [Plugin development](#plugin-development)

## Sessions and context

Tools that help you navigate, remember, and move around large sessions.

- [retitle](https://github.com/g4bwy/maki-plugins/tree/master/retitle) - Generates a readable title for each session, so the session picker stops showing truncated first prompts. Uses your current model, with no tier changes.
- [toc](https://github.com/g4bwy/maki-plugins/tree/master/toc) - Opens a table-of-contents pane with headlines for each turn, thinking block, and reply. Lets you jump through a long session instead of scrolling.
- [maki-session-index](https://github.com/zipadoodlez/maki-session-index) - Lets the agent search your past sessions. `session_index` greps a normalized outline and returns a short window around each hit. `session_read` expands the newest messages of one session. Read-only, one Lua file, no core changes.
- [notes](https://github.com/sdroege/maki-plugins/tree/main/notes) - Adds a `notes` tool and a `/notes` picker. The model saves session state that must survive compaction, and the plugin nudges it to write notes at 40, 60, and 80 percent of the context window. Ships in [maki-plugins](https://github.com/sdroege/maki-plugins), needs a few unmerged core changes.
- [history](https://github.com/sdroege/maki-plugins/tree/main/history) - Adds a `history` tool. The model reads its own session transcript: the current window, archived pre-compaction windows, and subagent transcripts, without touching session files on disk. Ships in [maki-plugins](https://github.com/sdroege/maki-plugins), needs a few unmerged core changes.
- [replay](https://github.com/g4bwy/maki-plugins/tree/master/replay) - Exports session logs to a format that [claude-replay](https://github.com/es617/claude-replay) can play back.

## Agent behavior and autonomy

Plugins that keep the agent working toward an objective or coordinate several agents.

- [goal](https://github.com/laudney/maki-essential-plugins/blob/main/plugin/goal.lua) - Keeps an objective alive across turns. When a turn ends while the goal is active, the agent is asked to continue, and it can mark the goal complete or blocked with a summary. `/goal pause|resume|clear`, saved per session. Ships in [maki-essential-plugins](https://github.com/laudney/maki-essential-plugins), tested on upstream Maki 0.6.0 and main.
- [monitor](https://github.com/laudney/maki-essential-plugins/blob/main/plugin/monitor.lua) - Watches a long-running command in the background. A matching line or the command's exit can wake the agent, or wait in the session mailbox until the next turn. `/monitors` lists and stops them. Ships in [maki-essential-plugins](https://github.com/laudney/maki-essential-plugins).
- [maki-peer](https://github.com/lu-zero/maki-peer) - Lets several agent instances run together and exchange messages. Supports Maki, OpenCode, Grok, and Vibe peers.

## Web search and fetch

Replacements and additions for the built-in search and fetch tools.

- [websearch (searxng)](https://github.com/g4bwy/maki-plugins/blob/master/websearch/init.lua) - Replaces the built-in websearch tool with your self-hosted [SearXNG](https://github.com/searxng/searxng) instance. Keeps search traffic on your own server.
- [maki-parallel](https://github.com/bzzimmy/maki-parallel) - Web search and fetch tools powered by the [Parallel AI](https://parallel.ai/) API. The author reports good result quality at low cost, and says it does not block Reddit thread extraction.
- [maki-browser-plugin](https://github.com/basvk/maki-browser-plugin) - Browser automation tools for Maki. Drives a real browser so the agent can click, fill forms, and read rendered pages.

## Providers and auth

Plugins that switch providers, or connect Maki to a subscription instead of a pay-per-token API key.

- [maki-claude](https://github.com/bzzimmy/maki-claude) - Lets Maki use a Claude Pro or Max subscription. A Python provider script handles the OAuth login and refreshes tokens. A small Lua plugin installs the script and adds a status command.
- [maki-claude-oauth](https://github.com/arxdeus/maki-claude-oauth) - A dynamic provider that reuses the OAuth session from your main Claude Code install. No separate Anthropic API key and no second browser login.
- [maki-antigravity](https://github.com/bzzimmy/maki-antigravity) - Lets Maki use a Google AI Pro / Antigravity subscription. The author uses it mainly for a cheap, fast research subagent.
- [provider](https://github.com/laudney/maki-essential-plugins/blob/main/plugin/provider.lua) - A provider and model picker. Log in to each provider once, then switch without logging out or re-entering API keys. `/provider`, `/provider zai`, or `/provider anthropic/claude-sonnet-4-6`. A change during an active run takes effect on the next run. Ships in [maki-essential-plugins](https://github.com/laudney/maki-essential-plugins).
- [maki-openai-oauth](https://github.com/arxdeus/maki-openai-oauth) - A Maki dynamic provider that authenticates with OpenAI through the ChatGPT/Codex OAuth flow.

> These subscription bridges may break the terms of service of the provider. Read the provider terms before you rely on one.

## Review and formatting

Plugins that act on your code changes.

- [maki-review](https://github.com/Asaf51/maki-review) - Adds a `/review` command. View the current changes, leave inline comments, and submit the feedback as a new prompt.
- [formatter.lua](https://gist.github.com/Crandel/8a5fd9c0505c9d3622494b2ee7c168fc) - Runs the formatter after any write, for Go, Rust, and Kotlin. The pattern extends to other languages.
- [OpenSpec plugin](https://gist.github.com/jenggo/6ae330b67f252dfa143a512638e6eec6) - Bridges Maki to [OpenSpec](https://github.com/Fission-AI/OpenSpec). It detects an existing OpenSpec init from Claude or OpenCode, and adds an "OpenSpec: create change from this plan" option to the plan-mode menu. Needs `fs_read = true` in `plugin.toml`.

## Safety and permissions

Plugins that guard the agent against destructive actions.

- [safe-ish-yolo](https://github.com/zipadoodlez/safe-ish-yolo) - A circuit breaker for the bash tool. It hooks `tool.bash.input` and refuses deletes that hit the working directory, `/etc`, or `~/.ssh`, plus `git reset --hard`, `clean -f`, `branch -D`, and `stash drop`. It reads command text only, so it does not catch a bad line inside a script.

## UI and editor integration

Plugins that change what you see on screen.

- [maki-design-plugin](https://github.com/basvk/maki-design-plugin) - Adds a `/design` toggle that switches Maki into a UI/UX designer mode with the right visual feedback loop. It builds on [maki-browser-plugin](https://github.com/basvk/maki-browser-plugin).
- [token-metrics](https://github.com/g4bwy/maki-plugins/tree/master/token-metrics) - Shows live generation metrics pulled from a vLLM Prometheus endpoint, so you can watch tokens-per-second while a local model serves you.
- [maki-herdr-pane-title-sync](https://github.com/xqqmdy/maki-herdr-pane-title-sync) - Syncs the Maki session title to the [herdr](https://github.com/ogulcancelik/herdr) pane title. Needs the change from Maki PR #1120.

## Plugin development

Starting points for writing your own plugin.

- [maki-lua-plugin-template](https://github.com/lu-zero/maki-lua-plugin-template) - A [cargo-generate](https://crates.io/crates/cargo-generate) template for a standalone Maki Lua plugin. Scaffolds the file layout and a working example.

## Conventions

- Each entry links to source you can read before you install.
- When one repository ships several plugins, each plugin gets its own entry, and the entry names the repository.
- Plugins are community work. Review the code before you grant it permissions.
- A single-file plugin that shows an author name is the author's own gist or repo.
- Descriptions come from what the author posted in [discussion #452](https://github.com/tontinton/maki/discussions/452).

## Contributing

Add a plugin after its author shares it in the showcase, or after you confirm it works on current Maki. Match the existing format: a link, then one sentence on what it does and one on how it does it. Keep the tone plain. Open a pull request with the source link and the Maki version you tested.

## License

[CC0](https://creativecommons.org/publicdomain/zero/1.0/)
