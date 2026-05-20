# vespeng/zh-CN-china-pack

Personal branch for @vespeng's mainland-China customization of Astro Rocket, based on `v1.4.1`.

**This branch is not merged upstream.** It carries region-specific code (Chinese localization, ICP/公安备案 footer) that doesn't belong in the generic theme. Rebase it onto future Astro Rocket releases when you want the latest features.

## What's included

1. **i18n flipped to `zh-CN` as the default.** `src/config/i18n.config.ts` has `enabled: true`, `defaultLocale: 'zh-CN'`, `locales: ['zh-CN', 'en']`. Chinese serves at the site root (`/`, `/blog`), English at `/en/...`.
2. **Content folder renamed.** All 16 demo blog posts moved from `src/content/blog/en/` to `src/content/blog/zh-CN/` with their frontmatter `locale` updated. Content is still English — replace or translate at your own pace. A single English stub (`src/content/blog/en/welcome.mdx`) keeps `/en/blog` resolving.
3. **`zh-CN.json` UI dictionary.** `src/i18n/zh-CN.json` has first-pass Simplified Chinese translations of every UI string (nav, blog, footer, contact form, etc.). Refine the wording as you like.
4. **Locale registered.** `src/i18n/index.ts` imports `zh-CN.json`. `src/content.config.ts` adds `'zh-CN'` to the locale enum for blog, projects, and pages.
5. **`china` compliance footer row.** A thin sub-bar below the main footer renders ICP and 公安备案 info when the config fields are set. Pattern matches what Bilibili, Zhihu, and most mainland-China sites use.

## Three things to do after pulling

### 1. Fill in your ICP filing number

Open `src/config/site.config.ts` and find the `compliance` block:

```ts
compliance: {
  icpNumber: '',                              // ← paste your ICP number here, e.g. '京ICP备12345678号-1'
  icpLink: 'https://beian.miit.gov.cn/',     // default, fine to leave
  publicSecurityNumber: '',
  publicSecurityLink: '',
  publicSecurityBadge: '',
},
```

Once `icpNumber` is non-empty, the sub-footer row will appear on every page.

### 2. Fill in your 公安备案 (Public Security Network Record) number

Same block — paste:

- `publicSecurityNumber`: e.g. `'京公网安备 11010102001234号'`
- `publicSecurityLink`: the link to your record on `beian.gov.cn` (usually `https://www.beian.gov.cn/portal/registerSystemInfo?recordcode=<your-code>`)
- `publicSecurityBadge` (optional): drop the official badge image into `/public/` and reference it, e.g. `'/beian-badge.png'`. The image renders inline next to the number.

### 3. Refine the Chinese translations

`src/i18n/zh-CN.json` ships with a working first pass. Open it and adjust any wording that doesn't match the tone of your site.

## Verifying locally

```bash
pnpm install
pnpm dev
```

Then check:

- `/` and `/blog` serve in Chinese.
- `/en/` and `/en/blog/welcome` serve in English.
- The LanguageSwitcher in the header toggles between `简体中文` and `English`.
- After filling in your ICP number, the compliance row appears at the bottom of every page.
- `<html lang>` reflects the URL (Chinese pages have `lang="zh-CN"`, English pages have `lang="en"`).

## Staying current with upstream

When a new Astro Rocket release ships:

```bash
git fetch origin
git rebase origin/main
```

Resolve any conflicts in `i18n.config.ts`, `content.config.ts`, or `site.config.ts` — keep your `zh-CN` settings and your compliance values. The layout fixes from `v1.4.1` (this branch's base) ensure the i18n routing works correctly.
