# alwaysmap-marketplace

The Claude Code plugin marketplace for [alwaysmap](https://github.com/alwaysmap) tools.

## What's in it

| Plugin | What it does | Source repo |
|---|---|---|
| `jfm` | Jobs for Me — tracks your job search, assesses fit against your profile, and preps you for interviews | [alwaysmap/jobs4me](https://github.com/alwaysmap/jobs4me) |
| `tmb` | Train My Brain — interviews you about a learning goal and builds a hands-on curriculum with a local Hugo site | [alwaysmap/train-my-brain](https://github.com/alwaysmap/train-my-brain) |

The authoritative list lives in [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json).

## Add it to Claude Code

```bash
claude plugin marketplace add alwaysmap/alwaysmap-marketplace
claude plugin marketplace update
claude plugin install tmb@alwaysmap
claude plugin install jfm@alwaysmap
```

The marketplace's internal name is `alwaysmap`, so plugins are addressed as `<plugin>@alwaysmap`.

In the Claude desktop app: **Customize → Plugins → Personal → + → Add marketplace**, then paste `alwaysmap/alwaysmap-marketplace`.

## How plugins get updated

Each plugin repo owns its own release process. On `git push --tags v*`:

1. The plugin repo's release workflow builds its zip and creates a GitHub Release.
2. It dispatches a `plugin-released` event to this repo with the plugin name + version.
3. This repo's [`update-versions.yml`](.github/workflows/update-versions.yml) workflow receives the event and runs [`scripts/update-plugin-version.sh`](scripts/update-plugin-version.sh) to update the `version` and `ref` fields for that plugin in `marketplace.json`.
4. Users who have the marketplace added see the new version on the next `claude plugin marketplace update`.

No manual edits to `marketplace.json` are needed for version bumps — the dispatch model keeps it in sync.

## Adding a new plugin to the marketplace

1. The plugin repo adds a `.github/workflows/release.yml` that dispatches `plugin-released` to this repo (see the tmb or jfm workflow as a template).
2. Add the plugin's initial entry to `marketplace.json` by hand (first-time only), matching the shape of the existing entries.
3. Set `MARKETPLACE_DISPATCH_TOKEN` in the plugin repo's Actions secrets — a fine-grained PAT scoped to `alwaysmap/alwaysmap-marketplace` with Contents write permission.

## History

Previously named `alwaysmap/marketplace`. Renamed to `alwaysmap/alwaysmap-marketplace` in April 2026 so the UI label reads `(alwaysmap)` instead of the generic `(marketplace)`. Fine-grained PATs survived the rename because they bind to repo ID, not repo name.
