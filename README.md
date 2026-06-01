# SciFlow Pro Download Mirror

This folder is a standalone public download page for SciFlow Pro v1.3.0.
It is intentionally separate from the Vite/Electron application source.

## What This Publishes

- `index.html`: static landing page with download buttons, version notes, screenshots, and license notice.
- `assets/screenshots/`: selected product screenshots.
- `installers/`: local staging folder for release installers; binaries are ignored by Git and should be uploaded to object storage or CDN.

## What This Must Not Publish

Do not deploy the normal Vite build output from `dist/` or `dist_surge/` as the public mirror. Those folders contain frontend application bundles. They are not raw source code, but they still expose client-side app implementation details.

## CDN Setup

Upload the installers to object storage or CDN, then replace the empty values in `index.html`:

```js
const mirrorDownloads = {
  windows: "",
  macos: "",
  android: ""
};
```

Expected file names:

- `SciFlow-Pro-Setup-1.3.0.exe`
- `SciFlow-Pro-1.3.0-arm64.dmg`
- `SciFlow-Pro-1.3.0-android.apk`

Until those URLs are filled in, the page keeps GitHub Release links as backup downloads.

## Deployment

Upload the contents of this folder, not the folder itself, to the static site root:

```bash
download-mirror/
  index.html
  assets/
  installers/
```

Recommended split:

- Static page: Vercel, Netlify, Cloudflare Pages, Gitee Pages, or another static host.
- Large installers: Aliyun OSS, Tencent COS, Qiniu, Cloudflare R2, or another object storage service with CDN.

For a real no-proxy download path, both the page and the installer files need to be reachable outside GitHub.
