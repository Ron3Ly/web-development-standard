# Web Typography & Font Standards

調査更新：2026-11-13。GPT Work / Codex が新規Webサイトを設計・実装するときに参照する共通フォント規約。
This is a **font selection + implementation standard**, not a repository of bundled font binaries. Project requirements and existing stable design systems may override the defaults.

## 1. Default fonts (new Japan-facing React / Vinext projects)

- **Geist Sans**: Latin letters, English navigation, numbers, mixed-language UI. Standard Latin/UI font.
- **Noto Sans JP**: Japanese Kanji/Hiragana/Katakana and Japanese body/form text. Standard Japanese font.
- **Geist Mono**: code blocks, technical identifiers and monospaced data **only**.

**Recommended default stack**:

```css
:root {
  --font-ui: "Geist", "Noto Sans JP", "Hiragino Kaku Gothic ProN",
    "Yu Gothic", "Meiryo", sans-serif;
  --font-japanese: "Noto Sans JP", "Hiragino Kaku Gothic ProN",
    "Yu Gothic", "Meiryo", sans-serif;
  --font-code: "Geist Mono", ui-monospace, "SFMono-Regular",
    "Cascadia Code", "Segoe UI Mono", Consolas, monospace;
}
```

Important: a CSS family list **does not download or install webfonts**. Set up working font loading in each actual app (local static assets / verified framework integration), then test computed fonts and network requests. The [copyable CSS file](../../examples/typography.css) is only the fallback/token layer; it intentionally does not embed or pretend to supply font binaries.

If the site is Japanese-copy-first and maximum script consistency is important, use `--font-japanese` as the body font instead of mixing Latin Geist with Japanese Noto. For frequently changing Japanese content, avoid unsafe fixed-character-only subsets.

### Why these defaults?

- Vercel describes Geist Sans as a clear, functional typeface and Geist Mono as its monospaced companion: https://vercel.com/font
- The Digital Agency's typography guidance adopts Noto Sans JP for legible Japanese UI (and Noto Sans Mono in its own system): https://design.digital.go.jp/dads/foundations/typography/
- Google Fonts' official Noto Sans JP metadata verifies Japanese support, variable weights and OFL licensing: https://github.com/google/fonts/blob/main/ofl/notosansjp/METADATA.pb

## 2. Curated font alternatives (choose per project, not all at once)

| Family | Intended role | Adoption rule | Official source / license evidence |
| --- | --- | --- | --- |
| Geist Sans | Default Latin sans/UI; number-heavy SaaS | Default in mixed-script product UI | https://vercel.com/font ; https://github.com/vercel/geist-font (OFL-1.1) |
| Noto Sans JP | Default Japanese sans/body/forms | Default Japanese glyph coverage, especially Japan-facing services | https://github.com/google/fonts/blob/main/ofl/notosansjp/METADATA.pb (OFL) |
| Geist Mono | Code/technical identifiers | Include only on pages/components that display mono text | https://github.com/vercel/geist-font (OFL-1.1) |
| BIZ UDPGothic | Readability-oriented Japanese forms/public-facing workflows | Alternative Japanese **body** family if users, display sizes and testing support it; weight 400/700 only in Google Fonts distribution | https://github.com/google/fonts/blob/main/ofl/bizudpgothic/METADATA.pb (OFL) |
| Inter | International/English-first web apps | Choose instead of Geist Sans when project branding and reading tests favor it; requires a Japanese font when Japanese is present | https://github.com/google/fonts/blob/main/ofl/inter/METADATA.pb (OFL) |
| IBM Plex Sans JP | Enterprise, data-dense Japanese business UI | Choose when a structured business-oriented visual identity is needed; test numbers, tables and Japanese text | https://github.com/google/fonts/blob/main/ofl/ibmplexsansjp/METADATA.pb (OFL) |
| Noto Serif JP | Editorial/articles/heritage-oriented titles | Use **selectively for headings/editorial content**, not automatically for all buttons or table labels | https://github.com/google/fonts/blob/main/ofl/notoserifjp/METADATA.pb (OFL) |
| Zen Maru Gothic | Friendly consumer, community, educational headings | Optional brand accent; test long copy readability; avoid adding solely for decoration | https://github.com/google/fonts/blob/main/ofl/zenmarugothic/METADATA.pb (OFL) |

The descriptions above are **recommended design applications**, not objective rankings or claims of WCAG certification. All font software should be acquired from its own authorized distribution and pinned per project; check the actual distributed files' license and font names.

## 3. Select a preset

| Project type | Body / UI | Headings / emphasis | Code / tabular values |
| --- | --- | --- | --- |
| Default Japanese/English SaaS / dashboard | Geist Sans + Noto Sans JP fallback | Same stack, adjusted weight/size | Geist Mono for code; tabular numerals for table columns |
| Japanese government/public, readability-first forms | Noto Sans JP or BIZ UDPGothic | Same family for consistency | System/Geist Mono only if necessary |
| International/English-first product | Geist Sans **or** Inter | Same family | Geist Mono as needed; add Noto Sans JP only when Japanese required |
| Enterprise-heavy Japanese data applications | Noto Sans JP or IBM Plex Sans JP | Same family | Tabular numerals and well-designed table alignment |
| Editorial / brand storytelling | Noto Sans JP | Noto Serif JP for selected headings | Mono generally not needed |
| Friendly community / learning brand | Noto Sans JP | Zen Maru Gothic for selected headings | Mono only for code/data use |

**Priorities:** user requirements, Japanese readability, consistent type scale, good fallback behavior and measured load performance come before stylistic novelty. Do not replace an existing project's brand font or install every font in this catalog.

## 4. Implementation guidelines (Vinext, Cloudflare, CSS)

1. **Check the pinned Vinext release and its font support.** As examined 2026-11-13, the [Vinext compatibility README](https://github.com/cloudflare/vinext#nextjs-compatibility) identifies `next/font/google` and `next/font/local` as partially implemented, and documents behavior that differs from Next.js. Do **not** assume Next.js automatic self-hosting, subset generation or fallback metrics. Source code / latest releases may evolve; verify behavior in the actual deployed version rather than relying on assumptions.
2. **Choose a font delivery strategy explicitly**:
   - Prefer reviewed **same-origin WOFF2 static assets** for predictable Cloudflare delivery and privacy where font redistribution is permitted. Document asset provenance, hash/version, served paths and font `@font-face`.
   - Or use Vinext's current font-loader support after validating **actual production font requests** (same-origin vs third-party CDN), CSS output, RSC boundaries and fallback metrics.
   - Google Fonts CDN may be a pragmatic choice when the project's privacy policies permit it; document the external font requests.
3. **Limit requests:** usually one body family and (at most) one display family; add a mono face only when required. Limit weights to those actually rendered (e.g., 400/500/700, or a suitable variable font). Avoid unnecessary italics and decorative families.
4. **Japanese font footprint matters.** Prefer official Japanese `unicode-range` CSS/chunked subsetting or a validated local subset strategy. Check coverage for dynamic user data; never ship only sampled Japanese glyphs when arbitrary text may appear.
5. **Rendering:** modern browsers prefer WOFF2. Use `font-display: swap` when the typeface must appear eventually, or `optional` where performance and no late swap are more important. Check text reflow/CLS; never suppress content while waiting for fonts.
6. **Fallback:** use sensible system Japanese sans-serif fonts, test on Windows/macOS/iOS/Android, verify punctuation and mixed Latin/Kanji baseline/weight. Avoid making semantic content depend on a specific font.
7. **Preload selectively:** do not preload every Japanese subset or weight; only verified critical font assets, testing transfer size and rendering benefits.
8. **License:** Geist, Noto Sans JP, BIZ UDPGothic, Inter, IBM Plex Sans JP, Noto Serif JP and Zen Maru Gothic are distributed in the linked sources under **SIL Open Font License**. If redistributing the font software, include required license/copyright notices and check Reserved Font Name / modifications as applicable. Do not commit third-party font binaries to **this standards repository**.
9. **Security and analytics:** do not send personal or confidential text to font services; review third-party network requests where privacy obligations apply.

### Example self-hosted @font-face (app repo only)

```css
/* This assumes you have legally obtained, versioned WOFF2 files at these paths.
   Filenames are ILLUSTRATIVE, not real assets shipped by this standards repo. */
@font-face {
  font-family: "Geist";
  src: url("/fonts/geist-latin-variable.woff2") format("woff2");
  font-style: normal;
  font-weight: 100 900;
  font-display: swap;
}

/* For Japanese text, add tested Noto Sans JP font files and accurate
   unicode-range declarations or a verified font-loader configuration.
   Do not assume one guessed WOFF2 file covers the entire JP character set. */
```

Use the [CSS token sample](../../examples/typography.css) after configuring real font sources.

## 5. Recommended typography and accessibility practices

- Default Japanese body `font-size` around `16px` and a comfortable `line-height` around `1.6–1.8`, adjusted by real users/devices. These are **design starting values**, not WCAG-mandated numeric minima.
- Keep headings, labels, helper text and table data on a consistent scale. Do not convey meaning using weight/color alone.
- For numbers in aligned tables use `font-variant-numeric: tabular-nums` **when the selected font supports it**; otherwise test with a suitable mono fallback.
- Test Japanese line breaks, Latin/Japanese punctuation, user-supplied long strings, large font preferences and zoom/reflow, including at 200%.
- Preserve minimum contrast and accessible form labels according to the project's target WCAG success criteria.
- Measure Core Web Vitals and explicitly inspect font-request failures, CLS, slow networks and reduced bandwidth.

## 6. Delivery checklist for agents

- [ ] Preset chosen and documented in the project brief, with reason for any deviation.
- [ ] Actual CSS font stack and webfont loading implemented, not only a written font recommendation.
- [ ] License & font files/weights provenance documented; notices included where required.
- [ ] Japanese and Latin glyph coverage validated, including uncommon characters if relevant.
- [ ] Vinext Cloudflare production deployed page checked for font requests and style behavior.
- [ ] No unnecessary family, weight, preload or oversized whole-font download.
- [ ] Text visible while fonts load, with acceptable reflow/CLS.
- [ ] Mobile/desktop, Windows/macOS and Japanese 200% zoom checked as practical.
- [ ] Report explicitly any untested conditions.

## References (official)

- Digital Agency typography: https://design.digital.go.jp/dads/foundations/typography/
- Vercel Geist: https://vercel.com/font ; https://github.com/vercel/geist-font
- Google Fonts catalog & licensing: https://developers.google.com/fonts?hl=ja
- Google Fonts family source and licenses: https://github.com/google/fonts/tree/main/ofl
- Google web.dev font delivery: https://web.dev/learn/performance/optimize-web-fonts ; https://web.dev/articles/optimize-webfont-loading
- Vinext compatibility: https://github.com/cloudflare/vinext
- Next.js `next/font` (not automatically identical to Vinext): https://nextjs.org/docs/app/getting-started/fonts
