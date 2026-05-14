# SPEC CRM Demo

Static SPEC CRM UI demo for GitHub Pages.

## Previews

- Production preview: https://lll887583-cmd.github.io/spec-crm/
- Test preview: https://lll887583-cmd.github.io/spec-crm/test/

## Branches

- `main`: stable version for colleagues.
- `test`: working version for requirement changes and self-review.

GitHub Pages is deployed by `.github/workflows/pages.yml`. The workflow publishes `main` to the site root and `test` to `/test/`, so separate long-lived Pages-copy branches are not needed.
