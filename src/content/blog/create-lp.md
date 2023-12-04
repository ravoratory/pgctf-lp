---
author: Ravie403
pubDatetime: 2023-12-05T00:00:00+09:00
title: PGCTFのサイトを作成しました
postSlug: create-pgctf-webpage
featured: true
draft: false
tags:
  - landing
description: PGCTFのサイトをAstroで作りました。
---

PGCTFのサイトを作成しました。技術スタックは[Bun](https://bun.sh/) + [Astro](https://astro.build/)となっており、GitHub Pagesによってホストされています。

## ビルドであった障害

最初のデプロイでは以下のエラーが発生しデプロイに失敗しました。

```plaintext
node:internal/process/promises:288
            triggerUncaughtException(err, true /* fromPromise */);
            ^

MissingSharp: Could not find Sharp. Please install Sharp (`sharp`) manually into your project or migrate to another image service.
    at loadSharp (file:///home/runner/work/pgctf-lp/pgctf-lp/dist/chunks/astro/assets-service_a3b407bb.mjs:352:11)
    at async Object.transform (file:///home/runner/work/pgctf-lp/pgctf-lp/dist/chunks/astro/assets-service_a3b407bb.mjs:364:15)
    at async generateImageInternal (file:///home/runner/work/pgctf-lp/pgctf-lp/node_modules/astro/dist/assets/build/generate.js:120:24)
    at async generateImage (file:///home/runner/work/pgctf-lp/pgctf-lp/node_modules/astro/dist/assets/build/generate.js:67:28)
    at async file:///home/runner/work/pgctf-lp/pgctf-lp/node_modules/p-queue/dist/index.js:118:36 {
  loc: undefined,
  title: 'Could not find Sharp.',
  hint: "See Sharp's installation instructions for more information: https://sharp.pixelplumbing.com/install. If you are not relying on `astro:assets` to optimize, transform, or process any images, you can configure a passthrough image service instead of installing Sharp. See https://docs.astro.build/en/reference/errors/missing-sharp for more information.\n" +
    '\n' +
    'See https://docs.astro.build/en/guides/images/#default-image-service for more information on how to migrate to another image service.',
  frame: undefined,
  type: 'AstroError'
}
```

どうやらこれはBun固有のsharpに関する問題らしく、[issue](https://github.com/oven-sh/bun/issues/4549)にあった通り、以下の記述を`package.json`に追加し、`node_modules`と`bun.lockb`を削除&`bun install`によって解決しました。

```json
  "trustedDependencies": [
    "sharp"
  ],
```
