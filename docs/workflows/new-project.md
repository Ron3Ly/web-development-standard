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
7. Define site map, route inventory, navigation hierarchy, browser history, scroll/focus policies, auth redirects and loading/error states using [Page Navigation UX](../design/page-navigation.md) and [Routing & Navigation](../architecture/routing-navigation.md). Review with [Navigation Review](navigation-review.md).
8. Read the [30-item web application development guideline](../design/ui-guidelines.md). Map applicable items No.1–30 to the project brief. Prioritize detailing No.1–14 and No.15–16 for interactive interfaces, then account for performance, reliability, security, QA and operation items. Plan UI with the Digital Agency Design System and selected asset licenses.
9. Create and review the project-specific data inventory and privacy disclosures using [Privacy Policy Standard](../legal/privacy-policy-standard.md), [Privacy Policy Template](../legal/privacy-policy.template.md) and [Privacy Review](privacy-review.md). Do not publish unverified placeholder text. Review Japanese law/2026 amendments and provider terms at release time.
10. Select an ASVS 5.0.0 target and write a threat model using [Security Standard](../security/security-standard.md) and [Threat Model Template](../security/threat-model.template.md); inspect the RSC/Vinext dependency advisory status, Cloudflare hardening and auth/data boundaries.
11. Implement, run available checks, preview on target devices, and validate deployed Cloudflare behavior including font requests, fallback and CLS. Exercise the navigation E2E matrix and SPA page-view measurement. Check privacy notice accessibility, cookie/external transmission behavior and Google OAuth policy URL. Run [Security Review](security-review.md) on the app in a non-destructive authorized test environment; record evidence and approval of risks.
12. Document changes, test results and outstanding concerns.

## Additional quality acceptance flow

After completing the initial project brief and before implementation, define the target pages, user journeys, representative browsers and accessibility scope using [Web Quality Standard](../quality/quality-standard.md) and [Project Targets](../quality/quality-targets.template.md). During implementation, follow [Measurement Playbook](../quality/measurement-playbook.md). Before release, complete [Quality Review](quality-review.md) alongside security, privacy and navigation gates. Record field vs lab metrics separately and never mark untested work Pass. Set Cloudflare SLO/monitoring based on [Reliability Guidance](../quality/reliability-observability.md).

## Reusable prompt

```text
共通仕様 https://github.com/Ron3Ly/web-development-standard を適用してください。
作業前にAGENTS.mdおよび関連する docs/ を読み、参照したファイル一覧を示してください。
案件固有要件は docs/project/brief.md を最優先し、共通技術スタックとデザイン基準を守ってください。
フォントは原則 Geist Sans + Noto Sans JP（コード用 Geist Mono）を採用し、docs/design/typography.md に従って配信・フォールバックを実装してください。案件に適した代替案があれば理由を示してください。
ページ遷移設計は docs/design/page-navigation.md と docs/architecture/routing-navigation.md に従い、サイトマップ、戻る操作、スクロール・フォーカス、読込中・エラー、必要に応じた遷移アニメーションを設計してください。docs/workflows/navigation-review.md に基づく確認結果を示してください。
プライバシーポリシーは docs/legal/privacy-policy-standard.md に従い、先にデータ取扱台帳を完成させ、実態に合う文案を作成してください。確認できない事項は事実を創作せず未確認と明記し、公開には運営者の承認を必要としてください。
品質基準は docs/quality/quality-standard.md のQ-01〜Q-36と docs/workflows/quality-review.md に従い、主要ユーザー操作、WCAG、CWVのfield/lab区別、ブラウザ、Cloudflare監視と公開後の指標を評価してください。案件別品質予算は docs/quality/quality-targets.template.md で定義し、実行していない検証をPassとしないでください。
セキュリティ基準は docs/security/security-standard.md のSEC-01〜SEC-36、Cloudflare対策は docs/security/cloudflare-hardening.md、チェックは docs/workflows/security-review.md に従ってください。脅威モデル・認証/認可・依存関係・修正済みRSCバージョン・リリースの判定と未確認項目を明示してください。
Vinext/Cloudflareの実際の対応状況と外部素材/APIの利用条件を確認してください。
最初に実装計画、最後にテスト結果・例外・未確認事項を報告してください。
```

## Future maintenance
Changes in this standards repo do **not automatically propagate** to already-created project repositories. Update dependent projects explicitly or automate reviewed synchronization later.
