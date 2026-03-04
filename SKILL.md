---
name: obsidian-cloudflare-pages
description: Publish selected Obsidian markdown from a vault to a static site and deploy to Cloudflare Pages.
homepage: https://pages.cloudflare.com/
---

# Obsidian → Cloudflare Pages

Automates a safe publishing flow:
1. Select notes from your Obsidian vault
2. Sync to a publish workspace
3. Build static HTML with Quartz
4. Deploy to Cloudflare Pages

## Commands

- `node skills/obsidian-cloudflare-pages/bin/publishmd-cf.js init`
  - Creates `config/config.json` from example
- `node skills/obsidian-cloudflare-pages/bin/publishmd-cf.js wizard`
  - Interactive setup wizard for config (vault, folders, site/domain, Cloudflare project)
- `node skills/obsidian-cloudflare-pages/bin/publishmd-cf.js setup-project`
  - Initializes Quartz project in configured workspace if missing
- `node skills/obsidian-cloudflare-pages/bin/publishmd-cf.js doctor`
  - Validates paths + required binaries
- `node skills/obsidian-cloudflare-pages/bin/publishmd-cf.js sync`
  - Syncs selected notes/assets into publish content folder
- `node skills/obsidian-cloudflare-pages/bin/publishmd-cf.js build`
  - Runs Quartz build in project dir
- `node skills/obsidian-cloudflare-pages/bin/publishmd-cf.js deploy`
  - Deploys to Cloudflare Pages with wrangler
- `node skills/obsidian-cloudflare-pages/bin/publishmd-cf.js run`
  - sync → build → deploy

## Config

Copy and edit:

`skills/obsidian-cloudflare-pages/config/config.example.json` → `skills/obsidian-cloudflare-pages/config/config.json`

### Safety defaults
- Publish allowlist by folder
- Optional `publish: true` frontmatter gate
- Exclude private folders by default

## Requirements

- `node` 20+
- `rsync`
- `npm`
- `npx quartz`
- `wrangler`

## Cloudflare API token setup (recommended)

Create a Cloudflare API token with at least:
- **Account → Cloudflare Pages:Edit**
- (Optional) **Zone → DNS:Edit** if you want DNS automation elsewhere

You can either export env vars in your shell profile (`~/.zshrc`) or use the skill-local `.env` file.

### Option A: shell profile (`~/.zshrc`)

```bash
export CLOUDFLARE_API_TOKEN="<your-token>"
export CLOUDFLARE_ACCOUNT_ID="<your-account-id>"
```

Reload shell:

```bash
source ~/.zshrc
```

### Option B: skill-local env file (recommended for this skill)

```bash
cp skills/obsidian-cloudflare-pages/.env.example skills/obsidian-cloudflare-pages/.env
# then edit .env
```

The CLI auto-loads `skills/obsidian-cloudflare-pages/.env` (without overriding existing shell env vars).

Wizard now asks for:
- Full production domain (e.g. `YOURDOMAIN.COM`)
- Branding labels (clippings index, root index, sidebar title HTML)
- Token/account env var names (defaults above)
- Optional basic-auth protection (username/password)
