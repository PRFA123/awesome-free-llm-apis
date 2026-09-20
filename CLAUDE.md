# CLAUDE.md

Guidance for AI agents working in this repo. Read this before editing anything.

This is an awesome-list of LLM APIs with permanent free tiers. There is no build, no
dependencies, and no test suite — the scripts are plain Node.js (v20) and run directly.

## README.md is generated — never hand-edit it

`README.md` is built from `data.json` by `scripts/generate-readme.js`. The
`.github/workflows/generate-readme.yml` workflow runs it on every push to `main` that
touches `data.json` and commits the result.

**Any content change goes in `data.json`, not `README.md`.** Editing `README.md` directly
gets silently overwritten on the next generation. To preview a change locally:

```sh
node scripts/generate-readme.js   # rewrites README.md from data.json
```

`data.json` holds `lastUpdated`, a `providers` array (each with `name`, `category`
(`provider_api` or `inference_provider`), `country`, `flag`, `url`, `baseUrl`,
`description`, `footnoteRef`, and a `models` array), plus glossary and footnote entries.
Match the formatting conventions of existing entries — see `contributing.md` for the
field-by-field rules and what qualifies as a free tier.

## Provider verification

`scripts/verify-providers.js` sends a live request to each provider/model in `data.json` to
confirm the free tier still works. It is run manually (not in CI) and needs API keys, read
from `.env.verify` at the repo root or from the environment:

```sh
node scripts/verify-providers.js --out .verify/report-YYYY-MM-DD.json
node scripts/verify-providers.js --out .verify/report-x.json --provider "Groq"   # narrow
node scripts/verify-providers.js --self-test                                     # harness check
```

`.verify/` holds the dated JSON reports and `state.json`, which tracks consecutive-failure
streaks per `Provider/model` (`count`, `firstFail`, `errorClass`) across runs so a repeatedly
failing model can be spotted before it's removed from `data.json`.

## The repo's own skill

`free-llm-apis/SKILL.md` is a Claude skill that walks a user through picking a free provider
and configuring an API key, with per-provider setup guides in
`free-llm-apis/references/provider-apis.md` and
`free-llm-apis/references/inference-providers.md`. It duplicates facts that live in
`data.json` (rate limits, model names, base URLs), so when you change provider data, check
whether the skill and its references need the same update.
