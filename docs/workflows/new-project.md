# New Project Workflow — GPT Work and Codex

## Why two layers?
- **This repository** is the source of truth for shared rules and references.
- An optional separate **web-starter** repository can contain runnable Vinext/Cloudflare code, dependencies, config and a project-local AGENTS.md.
- Each client/product project has its own requirements and overrides.

## Steps
1. Create a project repository (from a runnable starter if available).
2. Copy `docs/project/brief.template.md` into the new project as `docs/project/brief.md` and fill it in.
3. Ensure the agent **actually has access** to this standards repository. For Codex, copying `AGENTS.md` and relevant standard files into the active repo is the most reliable automatic-discovery method; alternatively explicitly supply/read the external files.
4. In GPT Work, supply the standards URL plus access to the GitHub content, and ask it to enumerate what it read before making decisions. Do not assume a URL alone makes Work read everything.
5. Verify Vinext, Cloudflare, database and auth compatibility for the project's pinned dependencies. Record any deviation.
6. Read [Typography / Font Standards](../design/typography.md), select a preset by audience, and implement actual font loading + fallback using the example at `examples/typography.css`. Check Vinext's pinned font-loader behavior, licenses and Japanese performance.
7. Read the [30-item web application development guideline](../design/ui-guidelines.md). Map applicable items No.1–30 to the project brief. Prioritize detailing No.1–14 and No.15–16 for interactive interfaces, then account for performance, reliability, security, QA and operation items. Plan UI with the Digital Agency Design System and selected asset licenses.
8. Implement, run available checks, preview on target devices, and validate deployed Cloudflare behavior including font requests, fallback and CLS.
9. Document changes, test results and outstanding concerns.

## Reusable prompt

```text
共通仕様 https://github.com/Ron3Ly/web-development-standard を適用してください。
作業前にAGENTS.mdおよび関連する docs/ を読み、参照したファイル一覧を示してください。
案件固有要件は docs/project/brief.md を最優先し、共通技術スタックとデザイン基準を守ってください。
フォントは原則 Geist Sans + Noto Sans JP（コード用 Geist Mono）を採用し、docs/design/typography.md に従って配信・フォールバックを実装してください。案件に適した代替案があれば理由を示してください。
Vinext/Cloudflareの実際の対応状況と外部素材/APIの利用条件を確認してください。
最初に実装計画、最後にテスト結果・例外・未確認事項を報告してください。
```

## Future maintenance
Changes in this standards repo do **not automatically propagate** to already-created project repositories. Update dependent projects explicitly or automate reviewed synchronization later.
