# SciFlow Pro Public Homepage

This folder is the standalone GitHub Pages download site for SciFlow Pro v1.3.15 desktop builds.
It is intentionally separate from the Vite/Electron application source and must publish only static product information and download links.

## What This Publishes

- `index.html`: static homepage with product positioning, workflow, module system, plugin library plan, screenshots, download buttons, and license notice.
- `reset-password/index.html`: public Supabase password-recovery page; keep the configured redirect URL aligned with the active static-site host.
- `BRAND.md`: public homepage brand-system notes for logo, color tokens, icon style, and product-tour layout.
- `assets/brand/`: SciFlow Pro SVG mark and wordmark.
- `assets/screenshots/`: selected product screenshots.
- `installers/`: optional local staging folder; binaries are ignored by Git and must not be committed to the Pages branch.

## What This Must Not Publish

Do not deploy the normal Vite build output from `dist/` or `dist_surge/` as the public mirror. Those folders contain frontend application bundles. They are not raw source code, but they still expose client-side app implementation details.

## GitHub Release Distribution

Publish installers and updater metadata to the public `gwennsteglik252-create/sciflow-downloads` Release. Point every download button in `index.html` directly to the versioned Release asset URL:

```text
https://github.com/gwennsteglik252-create/sciflow-downloads/releases/download/vX.Y.Z/<asset-name>
```

Expected file names:

- `SciFlow-Pro-Setup-1.3.15.exe`
- `SciFlow-Pro-1.3.15-arm64.dmg`
- `SciFlow-Pro-1.3.15-arm64.zip`
- `latest.yml` and `latest-mac.yml` for desktop auto-update metadata.

The default release path does not upload installers or updater metadata to Aliyun OSS/CDN. Publish only this folder's static files to the public repository's `gh-pages` branch.

## Current Plugin Distribution Status

- CDN domain: `plugins.sciflowpro.cn`
- CNAME: `plugins.sciflowpro.cn.queniuaa.com`
- Index path: `/plugins/index.json`
- Current state: HTTPS works through CDN private OSS origin.
- Default plugin index URL: `https://plugins.sciflowpro.cn/plugins/index.json`
- Remaining production step: complete ICP filing and switch CDN coverage from overseas to domestic when ready.

## Current Download Page Status

- Public URL: `https://gwennsteglik252-create.github.io/sciflow-downloads/`
- Source: public `sciflow-downloads` repository, `gh-pages` branch.
- Installer source: versioned assets in the public GitHub Release.
- Desktop updater source: `latest.yml`, `latest-mac.yml`, installers, zip files, and blockmaps in the same Release.
- Aliyun OSS/CDN is not part of the default release or verification path.
