# SciFlow Pro Public Homepage

This folder is a standalone public homepage and download mirror for SciFlow Pro v1.3.10.
It is intentionally separate from the Vite/Electron application source and can be deployed directly to a cloud server as a static site.

## What This Publishes

- `index.html`: static homepage with product positioning, workflow, module system, plugin library plan, screenshots, download buttons, and license notice.
- `reset-password/index.html`: public Supabase password-recovery page for `https://www.sciflowpro.cn/reset-password/index.html`.
- `BRAND.md`: public homepage brand-system notes for logo, color tokens, icon style, and product-tour layout.
- `assets/brand/`: SciFlow Pro SVG mark and wordmark.
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
  macosArm64: "",
  macosX64: "",
  android: ""
};
```

Expected file names:

- `SciFlow-Pro-Setup-1.3.10.exe`
- `SciFlow-Pro-1.3.10-arm64.dmg`
- `SciFlow-Pro-1.3.10-x64.dmg`
- `SciFlow-Pro-1.3.10-android.apk`

Until those URLs are filled in, the page keeps GitHub Release links as backup downloads.

## Cloud Server Deployment

Upload the contents of this folder, not the folder itself, to the static site root:

```bash
download-mirror/
  index.html
  assets/
  installers/
```

Example server path:

```bash
/var/www/sciflowpro/
  index.html
  assets/
  installers/
```

Minimal Nginx server block:

```nginx
server {
  listen 80;
  server_name sciflowpro.cn www.sciflowpro.cn;

  root /var/www/sciflowpro;
  index index.html;

  location / {
    try_files $uri $uri/ /index.html;
  }

  location ~* \.(png|jpg|jpeg|webp|svg|css|js)$ {
    expires 7d;
    add_header Cache-Control "public, max-age=604800";
  }
}
```

Recommended split:

- Static page: Vercel, Netlify, Cloudflare Pages, Gitee Pages, or another static host.
- Large installers: Aliyun OSS, Tencent COS, Qiniu, Cloudflare R2, or another object storage service with CDN.

For a real no-proxy download path, both the page and the installer files need to be reachable outside GitHub.

## Current Plugin Distribution Status

- CDN domain: `plugins.sciflowpro.cn`
- CNAME: `plugins.sciflowpro.cn.queniuaa.com`
- Index path: `/plugins/index.json`
- Current state: HTTPS works through CDN private OSS origin.
- Default plugin index URL: `https://plugins.sciflowpro.cn/plugins/index.json`
- Remaining production step: complete ICP filing and switch CDN coverage from overseas to domestic when ready.

## Current Homepage Status

- Public URL: `https://www.sciflowpro.cn/`
- Password reset URL: `https://www.sciflowpro.cn/reset-password/index.html`
- CDN domain: `www.sciflowpro.cn`
- CNAME: `www.sciflowpro.cn.queniuaa.com`
- Origin: `sciflow-plugins-1329180323221987.oss-cn-hangzhou.aliyuncs.com`
- Root object: `/index.html`
- Current state: HTTPS works through Aliyun CDN private OSS origin.
- Certificate: Let's Encrypt, expires on 2026-08-31.
- Local access note: if this Mac resolves `www.sciflowpro.cn` to `198.18.*`, Clash Verge fake-ip filtering must include `*.sciflowpro.cn` and `*.queniuaa.com`.
