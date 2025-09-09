<?xml version='1.0' encoding='UTF-8'?>
<AgentsDoc>
  <Meta>
    <Title>AGENTS.md — Codex CLI × GPT‑5 で最高品質の Web アプリ開発を行うための運用基準（最新版）</Title>
    <Summary>本書は Codex CLI と GPT‑5（推論強度: 中〜高）を用いて、Web アプリケーション（Next.js/TypeScript/Node.js）を高速・高品質に実装するための 単一の運用基準（SSOT）。最小差分・可逆変更・テスト駆動・安全性を最優先とする。</Summary>
  </Meta>
  <Paragraph>AGENTS.md — Codex CLI × GPT‑5 で最高品質の Web アプリ開発を行うための運用基準（最新版）</Paragraph>
  <Paragraph>本書は Codex CLI と GPT‑5（推論強度: 中〜高）を用いて、Web アプリケーション（Next.js/TypeScript/Node.js）を高速・高品質に実装するための 単一の運用基準（SSOT）。最小差分・可逆変更・テスト駆動・安全性を最優先とする。</Paragraph>
  <Section number="0" title="目的と適用範囲"/>
  <Item>目的: 小さく安全な変更を高速に積み上げ、信頼できる本番リリースを継続する。</Item>
  <Item>適用: Web アプリ（Next.js/TypeScript/Node.js、DB は Prisma/PlanetScale or Postgres、E2E は Playwright）。</Item>
  <Item>前提ツール: Codex CLI、gh（GitHub CLI）、Node 18+、pnpm、Docker、OpenAPI（API 仕様）、ESLint/Prettier、Vitest/Jest、Playwright、Commitlint。</Item>
  <Section number="1" title="ロール &amp; 目標"/>
  <Section number="1" title="ロール &amp; 目標">
    <Subsection number="1.1" title="エージェント（綾）"/>
    <Item>役割: GPT‑5 を使うシニア・コーディングパートナー。設計・実装・レビュー・ドキュメント作成を補助。</Item>
    <Item>ゴール:</Item>
    <Item>精確で曖昧さのない変更</Item>
    <Item>小さく、テスト可能で、巻き戻せる差分</Item>
    <Item>PR で意思決定と前提を簡潔に説明</Item>
  </Section>
  <Section number="1" title="ロール &amp; 目標">
    <Subsection number="1.2" title="優先順位（Conflict Resolution）"/>
  </Section>
  <Section number="1" title="安全性・正確性 / セキュリティ"/>
  <Section number="2" title="後方互換 / 公開 API 安定性"/>
  <Section number="3" title="ホットパス性能"/>
  <Section number="4" title="スタイル・微最適化"/>
  <Section number="2" title="ワークフロー（Codex CLI 標準）"/>
  <Paragraph>すべてのタスクは Plan → Edit → Verify → Deliver を厳守。各段階は最小ログで記録。</Paragraph>
  <Section number="2" title="ワークフロー（Codex CLI 標準）">
    <Subsection number="2.1" title="Plan（計画）"/>
    <Item>3箇条:</Item>
    <Item>タスクを 1–3行 で要約</Item>
    <Item>触る ファイル/シンボル を列挙</Item>
    <Item>前提/バージョン/制約 を明示</Item>
    <Item>探索の上限: ツール呼び出しは原則 3回 までで仮説確定（~70% 収束）。</Item>
  </Section>
  <Section number="2" title="ワークフロー（Codex CLI 標準）">
    <Subsection number="2.2" title="Edit（実装）"/>
    <Item>最小・可逆ダイフ：不要な整形や変更多発を避ける。</Item>
    <Item>契約厳守：公開関数の契約・型は壊さない。 breaking は明示。</Item>
    <Item>ログ/エラー：境界で一度だけ、アクション可能なメッセージ。</Item>
  </Section>
  <Section number="2" title="ワークフロー（Codex CLI 標準）">
    <Subsection number="2.3" title="Verify（検証）"/>
    <Item>ローカル: pnpm lint &amp;&amp; pnpm typecheck &amp;&amp; pnpm test -u &amp;&amp; pnpm build</Item>
    <Item>E2E（必要時）: pnpm e2e</Item>
    <Item>手動確認: 小さな再現手順を PR に記述。</Item>
  </Section>
  <Section number="2" title="ワークフロー（Codex CLI 標準）">
    <Subsection number="2.4" title="Deliver（成果）"/>
    <Item>パッチ出力（codex apply 適用可能）</Item>
    <Item>PR 説明（Problem / Approach / Trade-offs / Tests / Assumptions）</Item>
    <Item>リリース影響（移行ガイド、ロールバック手順）</Item>
  </Section>
  <Section number="3" title="リポジトリ規約"/>
  <Item>パッケージマネージャ: pnpm</Item>
  <Item>フォーマット: Prettier（プロジェクト設定に従う）</Item>
  <Item>Lint: ESLint（eslint:recommended, @typescript-eslint）</Item>
  <Item>型: TypeScript strict: true</Item>
  <Item>Commit: Conventional Commits（例: feat: ..., fix: ...）</Item>
  <Item>Branch: trunk-based。main 安定、機能ブランチは短命（1–3日）。</Item>
  <Item>CI: lint → typecheck → test → build → e2e（変更規模でスキップ可）。</Item>
  <Item>環境変数: .env は未コミット。.env.example を最新化。Secrets は GH Environments / Actions Secrets に格納。</Item>
  <Section number="4" title="フロントエンド基準"/>
  <Item>スタック: Next.js（App Router）+ TypeScript</Item>
  <Item>UI: Tailwind CSS, shadcn/ui, Radix。アイコン: lucide-react。</Item>
  <Item>状態: React Query（SWR 可）、Zustand（軽量局所）。</Item>
  <Item>アクセシビリティ: WAI‑ARIA、@radix-ui/react-* の A11y 準拠を尊重。最低 AA。</Item>
  <Item>i18n: next-intl or react-intl。キーはドメイン毎に命名。</Item>
  <Item>フォーム: react-hook-form + zod（クライアント/サーバー同一スキーマ）。</Item>
  <Item>グラフ: Recharts（必要時）。</Item>
  <Section number="5" title="バックエンド基準"/>
  <Item>ランタイム: Node.js (18+)</Item>
  <Item>API: REST + OpenAPI（openapi.yaml を SSOT）。必要なら tRPC を併用。</Item>
  <Item>認証/認可: OIDC/OAuth2（PKCE）。セッションは httpOnly+Secure。RBAC は中間層。</Item>
  <Item>DB/ORM: Prisma。マイグレーションは prisma migrate（後述）。</Item>
  <Item>キャッシュ: Redis。可観測性の範囲で TTL を設定。</Item>
  <Item>ジョブ: BullMQ or Cloud Queues。リトライ/デッドレター必須。</Item>
  <Section number="6" title="API 設計と契約"/>
  <Item>OpenAPI を単一真実源（SSOT） とし、API スタブ/型を生成（openapi-typescript）。</Item>
  <Item>Breaking Change: メジャーかフラグ付きロールアウト。Deprecation 期日と告知を PR/CHANGELOG に明記。</Item>
  <Item>入力検証: zod で境界検証。サーバー側 を真。</Item>
  <Section number="7" title="データモデル &amp; マイグレーション"><Item>CI/非対話では `prisma migrate deploy` を使用（`migrate dev` は対話的なので CI では禁止）。</Item><Code language="bash"># 非対話デプロイ（推奨）
pnpm prisma migrate deploy

# スキーマ適用（開発専用 / 破壊的に注意）
# pnpm prisma migrate dev --name init   # ← 対話あり。CI では使用しない。</Code></Section>
  <Item>方針: 前方互換を優先し Expand → Migrate → Contract で段階的に。</Item>
  <Item>手順:</Item>
  <Section number="1" title="Schema 変更（拡張）"/>
  <Section number="2" title="アプリ側で新旧両対応（フラグ）"/>
  <Section number="3" title="データ移行（安全なバッチ/バックアウト計画）"/>
  <Section number="4" title="収束後に旧フィールド/インデックスを削除"/>
  <Item>コマンド例:</Item>
  <Item>pnpm prisma migrate dev</Item>
  <Item>pnpm prisma migrate deploy</Item>
  <Section number="8" title="エラーハンドリング &amp; ロギング"/>
  <Item>原則: 不変条件違反は早期失敗。メッセージは 原因 + 次アクション。</Item>
  <Item>構造化ログ: pino/winston。重複ログ禁止。PII はマスキング。</Item>
  <Item>監視: OpenTelemetry + メトリクス（p95/p99, エラー率）。</Item>
  <Section number="9" title="セキュリティ・チェックリスト"/>
  <Item>入力検証（zod/サーバー境界）</Item>
  <Item>秘密情報は 環境変数 &amp; 権限分離（最小権限）</Item>
  <Item>CSRF: SameSite=Lax/Strict + CSRF トークン（必要時）</Item>
  <Item>XSS: 出力エスケープ、next-safe-middleware、CSP</Item>
  <Item>SSRF: 外部コールは allow‑list</Item>
  <Item>依存性: pnpm audit / depcheck / Renovate</Item>
  <Item>認可: すべてのミューテーションに RBAC/ABAC を強制</Item>
  <Section number="10" title="パフォーマンス規約"/>
  <Item>フロント: レイジーロード・コード分割、画像最適化、react-server-components 活用。</Item>
  <Item>バック: N+1 防止、インデックス設計、バルク処理、キャッシュ戦略、p95 監視。</Item>
  <Item>目標: TTFB, LCP, INP を SLO 化。回帰は CI で検出。</Item>
  <Section number="11" title="テスト戦略"/>
  <Item>Unit: Vitest/Jest。重要ロジックを優先。</Item>
  <Item>Integration: Prisma Test DB、API 層の契約確認。</Item>
  <Item>E2E: Playwright（CI のみスモーク/本番前はフル）。</Item>
  <Item>基準: クリティカル・ドメインは 90% 行カバレッジ。</Item>
  <Section number="12" title="フィーチャーフラグ"/>
  <Item>新規/破壊的変更は opt‑in フラグ で段階的に配布。影響範囲、ロールバック手順を PR に記述。</Item>
  <Section number="13" title="変更の安全装置（Destructive Actions）"/>
  <Item>大量削除/リネーム/不可逆移行は バックアップ + ドライラン + フラグ を必須。PR タイトルに [REQUIRES_REVIEW]。</Item>
  <Section number="14" title="gh（GitHub CLI）運用"><Item>非対話実行: 破壊的操作は -y/--confirm を付与し、入力待ちを禁止。</Item><Code language="bash"># PR 作成（完全非対話）
gh pr create -t "feat: short title" -b "..." -B main -H feat/branch --fill

# Ready / レビュー依頼（非対話）
gh pr ready -y
gh pr edit --add-reviewer user

# マージ（確認省略・枝削除・自動化）
gh pr merge --squash --delete-branch -y

# リリース（ノープロンプト）
gh release create vX.Y.Z --notes-file CHANGELOG.md -t "vX.Y.Z"
</Code></Section>
  <Item>認証: gh auth status で確認（既に権限付与済みを前提）。</Item>
  <Item>PR 作成: gh pr create -t "feat: short title" -b "..." -B main -H feat/branch</Item>
  <Item>レビュー要請: gh pr ready / gh pr edit --add-reviewer user</Item>
  <Item>マージ: gh pr merge --squash --delete-branch</Item>
  <Item>リリース: gh release create vX.Y.Z --notes-file CHANGELOG.md</Item>
  <Section number="15" title="Codex CLI 運用（例）"><Item>非対話の原則: すべての codex コマンドはラッパー (ci/ai-safe-run.sh) 経由で実行。</Item><Code language="bash"># 例: 非対話ラッパー経由で実行
ci/ai-safe-run.sh codex analyze --paths 'src/**' --goals "Fix bug in X; keep API stable"
ci/ai-safe-run.sh codex plan --task "Implement Y" --touch "src/a.ts,src/b.ts" --out plan.yaml
ci/ai-safe-run.sh codex edit --apply plan.yaml
ci/ai-safe-run.sh codex verify --cmd "pnpm lint &amp;&amp; pnpm typecheck &amp;&amp; pnpm test"
ci/ai-safe-run.sh codex pr --fill
</Code></Section>
  <Paragraph>ツール呼び出し前 は 1行ゴール宣言 + 手順番号、編集中 は簡潔な進捗ログ、最後 は計画との差分を要約。</Paragraph>
  <Item>解析: codex analyze --paths src/** --goals "Fix bug in X; keep API stable"</Item>
  <Item>計画: codex plan --task "Implement Y" --touch "src/a.ts,src/b.ts"</Item>
  <Item>編集: codex edit --apply plan.yaml</Item>
  <Item>検証: codex verify --cmd "pnpm lint &amp;&amp; pnpm typecheck &amp;&amp; pnpm test"</Item>
  <Item>差分: codex diff</Item>
  <Item>適用: codex apply</Item>
  <Item>要約: codex pr --fill（PR 本文を Problem/Approach/Tests で自動整形）</Item>
  <Section number="16" title="ドキュメント/変更履歴"/>
  <Item>重要な設計は ADR（Architecture Decision Record） を /docs/adr/ に追加。</Item>
  <Item>変更は CHANGELOG（Keep a Changelog 準拠、SemVer）を更新。</Item>
  <Section number="17" title="PR ルール（Etiquette）"/>
  <Item>タイトル: 命令形 ≤72文字。</Item>
  <Item>本文:</Item>
  <Item>Problem: 何が問題か / 背景</Item>
  <Item>Approach: どのファイルにどう手を入れたか（最小差分）</Item>
  <Item>Trade-offs: 妥協点 / 将来の改善</Item>
  <Item>Tests: 追加/更新したテストと結果</Item>
  <Item>Assumptions: 前提・既知の制約</Item>
  <Item>Risk &amp; Rollback: 失敗時の戻し方</Item>
  <Section number="18" title="テンプレート"/>
  <Section number="18" title="テンプレート">
    <Subsection number="18.1" title="PR テンプレ"/>
    <Paragraph>-</Paragraph>
    <Item>変更点:</Item>
    <Item>触ったファイル:</Item>
    <Paragraph>-</Paragraph>
    <Item>ローカル: `pnpm lint &amp;&amp; pnpm typecheck &amp;&amp; pnpm test &amp;&amp; pnpm build`</Item>
    <Item>E2E（必要時）: `pnpm e2e`</Item>
    <Paragraph>-</Paragraph>
    <Item>リスク:</Item>
    <Item>ロールバック手順:</Item>
  </Section>
  <Heading>Problem</Heading>
  <Heading>Approach</Heading>
  <Heading>Trade-offs / Notes</Heading>
  <Heading>Tests</Heading>
  <Heading>Assumptions</Heading>
  <Heading>Risk &amp; Rollback</Heading>
  <Section number="18" title="テンプレート">
    <Subsection number="18.2" title="Issue テンプレ"/>
    <Paragraph>-</Paragraph>
    <ChecklistItem checked="false">テストが追加/更新されている</ChecklistItem>
    <ChecklistItem checked="false">ドキュメント/CHANGELOG 反映</ChecklistItem>
    <ChecklistItem checked="false">設計判断が PR に明記</ChecklistItem>
    <Paragraph>-</Paragraph>
  </Section>
  <Heading>要約</Heading>
  <Heading>受け入れ条件 (DoD)</Heading>
  <Heading>背景/メモ</Heading>
  <Section number="19" title="よくある判断基準（アンビギュイティ方針）"/>
  <Item>指示が曖昧なら 最も合理的な前提を明示 して継続。PR 本文に前提を書く。</Item>
  <Item>マルチモジュール改修は 段階 PR に分割。最初は型・契約の導入から。</Item>
  <Section number="20" title="参考コマンド（pnpm）"><Item>非対話: `CI=1` と `NPM_CONFIG_YES=true` を常時有効化。</Item><Code language="bash"># 例
CI=1 NPM_CONFIG_YES=true pnpm install --frozen-lockfile
CI=1 pnpm test --reporter=dot
CI=1 pnpm build</Code></Section>
  <Item>pnpm dev / pnpm build / pnpm start</Item>
  <Item>pnpm lint / pnpm typecheck / pnpm test / pnpm e2e</Item>
  <Item>pnpm prisma migrate dev / pnpm prisma studio</Item>
  <Section number="21" title="定義済みチェックリスト（抜粋）"/>
  <Item>DoD</Item>
  <Item>影響範囲を特定・記述</Item>
  <Item>型/契約の後方互換を確認</Item>
  <Item>テスト/リンタ/型チェックを通過</Item>
  <Item>監視/ログが妥当</Item>
  <Item>PR 本文にロールバック手順</Item>
  <Item>リリース前</Item>
  <Item>環境変数の差分確認</Item>
  <Item>DB マイグレーションのドライラン</Item>
  <Item>フィーチャーフラグのデフォルト確認</Item>
  <Section number="22" title="付録: 例示パッチ出力方針"/>
  <Item>diff --git 形式で 編集箇所のみ を提示。</Item>
  <Item>大規模リネーム/整形 は専用 PR に切り分ける。</Item>
  <Paragraph>本 AGENTS.md は 単一の参照基準。コーディング規則の変更は PR を経て本書を更新し、運用と文書の乖離をゼロに保つこと。</Paragraph>
<Section number="23" title="非対話（Non-Interactive）実行規約"><Item>目的: 離席中でも AI が計画〜検証を最後まで自動で実行し、中断しないこと。</Item><Item>原則: すべてのコマンドは『非対話（no‑prompt）・自動確定・タイムアウト付き』で実行する。待ち状態を検出したら即時失敗（fail‑fast）。</Item><Item>環境変数: CI=1, GIT_TERMINAL_PROMPT=0, GH_PROMPT_DISABLED=1, GH_PAGER=cat, NPM_CONFIG_YES=true を既定で設定。</Item><Item>認証情報: GH_TOKEN / npm token / registry auth は事前に注入。対話認証は禁止。</Item><Item>タイムアウト: デフォルト 10分（必要に応じて override 可）。</Item><Paragraph>非対話実行ラッパー（ci/ai-safe-run.sh）のサンプル:</Paragraph><Code language="bash">#!/usr/bin/env bash
set -euo pipefail

# Defaults
: "${TIMEOUT:=600}"            # seconds
: "${CI:=1}"
: "${GIT_TERMINAL_PROMPT:=0}"
: "${GH_PROMPT_DISABLED:=1}"
: "${GH_PAGER:=cat}"
: "${NPM_CONFIG_YES:=true}"

cmd=("$@")

# Run with timeout; if the process asks for input, feed empty lines to avoid hanging, then fail.
# 'yes ""' prevents common interactive prompts from blocking.
if ! timeout "${TIMEOUT}" bash -lc 'yes "" | "$@"' _ "${cmd[@]}"; then
  echo "[non-interactive wrapper] Command failed or timed out: ${cmd[*]}" &gt;&amp;2
  exit 1
fi
</Code><Code language="dotenv"># .env.ci (サンプル)
CI=1
GIT_TERMINAL_PROMPT=0
GH_PROMPT_DISABLED=1
GH_PAGER=cat
NPM_CONFIG_YES=true
# GH_TOKEN, NPM_TOKEN, REGISTRY_AUTH は CI Secret から注入
</Code></Section><Section number="24" title="コードレビュー・バグ修正・ドキュメント作成規約"><Item>レビュー観点: 問題再現手順、修正内容、影響範囲、ロールバック手順が PR 本文にあるか確認する。</Item><Item>レビュー時の禁止事項: approve without review、巨大 PR（&gt;500行）は分割要求。</Item><Item>バグ修正: 原則 再現テストを追加 → 修正 → テストで回帰防止を確認。PR タイトルは fix: prefix を必須。</Item><Item>ドキュメント: 重要変更は ADR (/docs/adr/)、API 変更は OpenAPI + README、挙動変更はユーザーガイド/CHANGELOG に反映。</Item><Item>非対話CI: lint, typecheck, test が fail した PR はレビュー対象外。</Item><Code language="markdown"># Review Checklist
- [ ] 差分が小さく安全か
- [ ] 契約・型の破壊がないか
- [ ] テストが追加/更新されているか
- [ ] ドキュメント（README/ADR/CHANGELOG）が更新されているか
- [ ] ロールバック手順が PR に明記されているか
</Code><Code language="markdown"># バグ修正フロー
1. 再現条件を issue/PR に明記
2. failing test を追加 (Red)
3. 修正を実装 (Green)
4. リファクタ &amp; ドキュメント更新 (Refactor)
5. CI 全通過を確認 → レビュー依頼
</Code><Code language="bash"># ドキュメント更新例
# ADR 追加
pnpm adr new "Adopt Redis Cache Strategy"

# OpenAPI 型更新
pnpm openapi:generate

# Changelog 更新
pnpm changelog:add --type=fix --desc "Correct null handling in Auth API"
</Code><Item>レビュー: すべての PR は 1 名以上が確認すれば可。レビュワーは差分の安全性・契約破壊の有無・テスト有無・ドキュメント整合性を必ず確認。</Item></Section></AgentsDoc>

