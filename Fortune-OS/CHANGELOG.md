# CHANGELOG

Fortune-OS の変更履歴。方針・マイルストーンは `ROADMAP.md` を参照。

書式は [Keep a Changelog](https://keepachangelog.com/) を緩く踏襲する。

---

## [0.1.0] — 2026-07-01

### Added（追加）
- プロジェクト骨格とディレクトリ構造（`docs/` `system/` `user/` `astrology/` `psychology/` `strategy/` `knowledge/` `templates/`）
- `knowledge/`（人物非依存の汎用リファレンス層）：astrology_reference / psychology_models / strategy_frameworks
- `user/priorities.md`（優先順位）
- **`CLAUDE.md`** — 最優先ルール（7レイヤー分離・ハード禁止・読む順序）
- `README.md` — プロジェクト概要と設計思想
- `ROADMAP.md` — v0.1〜v1.0 のマイルストーン
- `system/system_prompt.md` — コア・システムプロンプト
- `system/output_rules.md` — 出力の3ブロック構造とタグ運用
- `system/quality_rules.md` / `system/safety_rules.md` — v0.1 プレースホルダ（骨子あり）
- `user/subject.md`（本人）/ `user/wife.md`（配偶者）— 事実と未計算枠の分離
- `astrology/`（bazi / western_astrology / six_star_style / compatibility）— 未計算プレースホルダ
- `psychology/`（personality / cognitive_bias / stress_patterns）— v0.1 枠組み
- `strategy/`（career / finance / relationships / decision_framework）— v0.1 枠組み
- `docs/`（philosophy / methodology / glossary）— v0.1 骨子
- `templates/full_reading.md` — 全体リーディング（実用レベル）
- `templates/`（annual / compatibility / decision_support）— v0.1 骨子

### 設計方針
- 占術データは**意図的にすべて未計算**。まずレイヤー分離と禁止事項の骨格を固める方針。
- 事実 / 計算済 / 占術解釈 / 推測 / 現実照合 / 反証 / 信頼度 の7レイヤー分離を全ファイル共通の規約とした。

### Known limitations（既知の制約）
- 四柱・ホロスコープ・運気サイクルは未計算（v0.3 で実計算予定）。
- 出生時刻は本人・配偶者とも未検証。
- 現実照合の実データ欄（`user/*` §4）が未記入。
