# kube — Interior Design Skill

This repo registers a Claude Code **skill** for interior design work, so Claude can
generate and edit interior visuals directly in conversation: room renders, mood boards,
furniture layouts, color palettes, and before/after renovation makeovers.

It is adapted from [kkoppenhaver/cc-nano-banana](https://github.com/kkoppenhaver/cc-nano-banana),
a general-purpose image-generation skill built on the Gemini CLI's `nanobanana` extension
(MIT licensed — see [LICENSE](LICENSE)). This version focuses its prompts, command table,
and examples specifically on interior design.

## Where the skill lives

```
.claude/skills/interior-design/SKILL.md
```

Claude Code auto-loads any skill under `.claude/skills/` in the repo, so once this repo
is checked out, the skill is available automatically — no manual copy step needed.

## Prerequisites

1. **Gemini CLI**
   ```bash
   npm install -g @google/gemini-cli
   ```
2. **Gemini API key** — get one from [Google AI Studio](https://aistudio.google.com/app/apikey), then set it locally
   (never commit it — see `.env.example`):
   ```bash
   export GEMINI_API_KEY="your-api-key"
   ```
   To make it permanent, add that line to your shell profile (`~/.bashrc` or `~/.zshrc`), or copy
   [`.env.example`](.env.example) to `.env` and fill it in — `.env` is already git-ignored.
3. **nanobanana extension**
   ```bash
   gemini extensions install https://github.com/gemini-cli-extensions/nanobanana
   ```

## Usage

Once the prerequisites above are set up, just ask Claude for interior design help in
natural language, e.g.:

- "صمم لي غرفة معيشة بستايل اسكندنافي" / "Design a Scandinavian-style living room"
- "Furnish this empty bedroom photo in a modern minimalist style"
- "Make a mood board for a bohemian bedroom"
- "Show a before/after renovation of this kitchen"
- "Give me 3 color palette options for a sage green kitchen"

Claude will pick up the `interior-design` skill automatically and drive the Gemini CLI
to produce the images. Generated files are saved to `./nanobanana-output/`.

## Model & cost

Default model is `gemini-2.5-flash-image` (~$0.04/image). For higher quality, more
photorealistic room renders:

```bash
export NANOBANANA_MODEL=gemini-3-pro-image-preview
```

## Bundled plugin: caveman (token compression)

`.claude/settings.json` in this repo registers the [caveman](https://github.com/juliusbrussee/caveman)
plugin (third-party, by Julius Brussee) and enables it. It cuts Claude's *output* token
usage (~65% in the author's benchmarks) via `SessionStart`/`UserPromptSubmit` hooks that
run automatically on every session.

⚠️ **This is unverified third-party code that runs automatically inside your Claude Code
environment.** Review [its source](https://github.com/juliusbrussee/caveman) before relying
on it. To disable it, remove the `enabledPlugins` / `extraKnownMarketplaces` entries from
`.claude/settings.json`, or set `"caveman@caveman": false` in `.claude/settings.local.json`
(gitignored, personal override — won't affect teammates).

**To verify it's active:** open Claude Code in this repo and run `/hooks` — you should see
`caveman` hooks listed under `SessionStart` and `UserPromptSubmit`.

## Credits & License

Adapted from [cc-nano-banana](https://github.com/kkoppenhaver/cc-nano-banana) by
Kurt Koppenhaver, MIT licensed. This repo's contents are also MIT licensed — see
[LICENSE](LICENSE).
