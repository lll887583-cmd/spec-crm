# AGENTS.md

## Project Overview

This repository is a static SPEC CRM UI demo served by GitHub Pages.

- Main entry: `index.html`
- Static assets: `assets/`
- Production preview: `https://lll887583-cmd.github.io/spec-crm/`
- Test preview: `https://lll887583-cmd.github.io/spec-crm/test/`

The project is intentionally lightweight: avoid adding build tooling unless explicitly requested.

## Branch Model

Use only the source branches for normal work:

- `main` is the stable preview branch for colleagues.
- `test` is the working preview branch for requirement changes and self-review.

GitHub Pages is deployed by `.github/workflows/pages.yml` using GitHub Actions:

- `main` is published to the Pages root path `/`.
- `test` is published to `/test/` under the same Pages site.

Do not manually maintain `gh-pages`, `gh-test-pages`, or other Pages-copy branches. If such branches exist, treat them as stale deployment artifacts unless the user explicitly says otherwise.

## Design Direction

Follow the SPEC CRM design language:

- Keep the UI clean, professional, restrained, and information-dense.
- Use the existing token system in `:root`; prefer reusing existing variables instead of introducing one-off colors.
- Brand primary color is `#1f3472`.
- Global hover color should stay consistent with sidebar hover: `var(--nav-hover)`.
- Form controls, selects, buttons, dropdowns, tabs, tooltips, modals, and toast copy should remain globally consistent.
- Do not introduce emoji or decorative symbols.

## Editing Guidelines

- Treat `index.html` as the source of truth.
- Preserve the current single-file static demo structure unless the user asks for a refactor.
- Keep copy and UI labels internationalized through `t(...)` and the `i18n.zh` map.
- Do not hardcode Chinese text in visible UI when the page supports English mode.
- For any new user-facing text, add the English source string and the Chinese translation.
- Do not commit temporary backup files such as `index.html.bak-*`.
- Avoid changing unrelated UI sections while editing a specific page or component.

## Pop-up Cards Edit Page Notes

The active route for the current demo work is:

`#backoffice-marketing-pop-up-cards-edit`

Important conventions on this page:

- Keep card content aligned with the card edges.
- Buttons outside cards should align left with the card content unless the user says otherwise.
- Button styles are limited to:
  - Primary: blue fill, white text/icon.
  - Secondary: white fill, blue border/text/icon.
  - Danger: white fill, red border/text/icon.
- Delete actions should use danger styling.
- Dropdown/popup panels should use AntD-like internal padding while preserving current colors.
- Card-level language tabs are not the same as page-level tabs; card-level tabs use 14px text and lighter interaction.

## Validation

Before handing off changes, validate the inline JavaScript:

```bash
python3 - <<'PY'
from pathlib import Path
import re
s = Path('index.html').read_text()
script = re.findall(r'<script>([\s\S]*?)</script>', s)[-1]
Path('/tmp/spec-crm-script.js').write_text(script)
PY
node --check /tmp/spec-crm-script.js
```

For visual checks, a local file URL is sufficient:

```bash
open index.html
```

For screenshot checks in headless Chrome, use the local checkout path and the target route, for example:

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless --disable-gpu --hide-scrollbars \
  --screenshot=/tmp/spec-crm.png \
  --window-size=1440,1100 \
  'file:///absolute/path/to/spec-crm/index.html#backoffice-marketing-pop-up-cards-edit'
```

## Git / Deployment

- Pushes to `main` and `test` both trigger the Pages workflow.
- Push only when the user explicitly asks to publish or push.
- Keep `main` and `test` as the only active source branches.
- Recommended flow:

```bash
git status --short
node --check /tmp/spec-crm-script.js
git add index.html AGENTS.md .github/workflows/pages.yml
git commit -m "Update spec CRM demo"
git push origin main
```

- Leave untracked backup files uncommitted unless the user specifically requests them.
