# MIGRATION_v0.9.md

Fortune-OS **v0.9 移行実行計画**（計画のみ・未実行）。

> **本ファイルは実行計画。ファイルの移動・リネーム・参照書き換えは一切行っていない。**
> 前提：`ARCHITECTURE_REVIEW_v0.9.md` の5層構造提案と、それに対する設計判断（下記§1）が確定済み。
> 本ファイルはその「何を・どの順で・どうやって安全に動かすか」を定義する。
> 本ファイルは仕様・計画文書であり、7レイヤータグの適用対象外（`CLAUDE.md` §2 の精神に準拠）。

---

## 1. 採用した5つの設計判断（確定・前提条件）

`ARCHITECTURE_REVIEW_v0.9.md` §6 の未決定事項に対する回答。以下を確定事項として本計画に組み込む。

| # | 論点 | 決定 | 理由 |
|---|---|---|---|
| 1 | `decision_framework.md` の位置 | **`plugins/strategy/engine.md`** に格上げして配置（core/reasoning.md は作らない） | 意思決定支援は Core ではなく Strategy Plugin の実行ロジック。Core に置くと OS 全体が戦略判断に寄りすぎる |
| 2 | `compatibility.md` の位置 | **`plugins/compatibility/engine.md`** として独立Plugin化 | 夫婦・相性分析は bazi/western/psychology を横断するため、単一占術Plugin配下に置かない |
| 3 | `CLAUDE.md` の位置 | **ルート直下に残す**。内容上は Core Policy の正典として扱う | Claude Code 運用上、最優先で読ませる必要がある |
| 4 | psychology/strategy の呼称 | **"Analytical Plugins"** と呼ぶ | 占術Pluginではないが、差し替え可能な分析モジュールとして扱う |
| 5 | ディレクトリ名称 | **`core / pipeline / plugins / templates / user / docs / logs`** | 責務が明確で、将来の拡張に耐える |

**`logs/` の性質（本計画での運用定義）：** 監査・決定・レビュー・計画など「追記型・時系列の記録」を格納する。
`SOURCE_POLICY.md`・`calculation_config.md`・`LIMITATIONS.md` のような「現在有効な固定ポリシー」は
`logs/` ではなく `core/` に置く（ポリシーは"記録"ではなく"現在の規則"のため）。

---

## 2. 完全マッピング表（現行 → 移行先）

### 2-1. ルート直下（変更なし）

| 現行 | 移行先 | 備考 |
|---|---|---|
| `CLAUDE.md` | 変更なし | 決定#3。内容上はCore Policy正典 |
| `README.md` | 変更なし（内容更新） | ディレクトリ構成図・使い方を新構造に更新 |
| `ROADMAP.md` | 変更なし（内容更新） | v0.9マイルストーンを追記 |
| `CHANGELOG.md` | 変更なし（内容更新） | 移行完了エントリを追記 |

### 2-2. `logs/`（新設）— Phase A

| 現行 | 移行先 |
|---|---|
| `CALCULATION_AUDIT.md` | `logs/CALCULATION_AUDIT.md` |
| `DECISION_LOG.md` | `logs/DECISION_LOG.md` |
| `QA_CHECKLIST.md` | `logs/QA_CHECKLIST.md` |
| `REVIEW_v0.2.md` | `logs/REVIEW_v0.2.md` |
| `REVIEW_v0.2_FIX_LOG.md` | `logs/REVIEW_v0.2_FIX_LOG.md` |
| `PLAN_v0.3_calculation.md` | `logs/PLAN_v0.3_calculation.md` |
| `ARCHITECTURE_REVIEW_v0.9.md` | `logs/ARCHITECTURE_REVIEW_v0.9.md` |
| `MIGRATION_v0.9.md`（本ファイル） | `logs/MIGRATION_v0.9.md`（**実行完了後に自分自身も移動する**） |

> **注意：** これらは歴史文書。移動時に**中身の記述（当時のパス表記）は書き換えない**。
> 「`system/output_rules.md`」等の旧パス表記が本文中に残っていても、それは歴史的記録として正しい。

### 2-3. `core/`（新設）— Phase B

| 現行 | 移行先 | 備考 |
|---|---|---|
| `system/system_prompt.md` | `core/system_prompt.md` | そのまま移動 |
| `system/output_rules.md` | `core/output_rules.md` | そのまま移動（参照数15・最多参照ファイルの1つ） |
| `system/quality_rules.md` | `core/quality_rules.md` | そのまま移動 |
| `system/safety_rules.md` | `core/safety_rules.md` | そのまま移動（参照数18・最多参照ファイル） |
| `docs/glossary.md` | `core/glossary.md` | **理論用語（四柱・西洋の用語集§4-5）はここから剥がし、§2-4の各plugin reference.mdへ移す。タグ定義（§1-3）のみcoreに残す** |
| `knowledge/calculation_config.md` | `core/calculation_config.md` | 参照数13。全プラグイン共通設定のためcore |
| `SOURCE_POLICY.md` | `core/source_policy.md` | 固定ポリシーのためlogsではなくcore |
| `LIMITATIONS.md` | `core/limitations.md` | 固定ポリシーのためcore。**移動と同時に孤立解消（README/CLAUDE.mdから参照を追加）** |

### 2-4. `pipeline/`（新設）— Phase C

| 現行 | 移行先 | 備考 |
|---|---|---|
| （新規） | `pipeline/analysis_pipeline.md` | `docs/methodology.md` §1 の7ステップを9ステップに昇格・正式版として新規執筆 |
| `docs/methodology.md` | `docs/methodology.md`（内容縮小・残留） | §1のパイプライン図は削除し `pipeline/analysis_pipeline.md` への参照に置換。§2以降（データ3階層・現実照合のやり方等）はdocsに残す |
| `core/evidence.md`（新規） | — | 7タグの契約・証拠の型を形式化。`system_prompt.md`§1・`output_rules.md`§1の内容を抽出統合 |
| `core/confidence.md`（新規） | — | 信頼度の定量化モデル（`ARCHITECTURE_REVIEW_v0.9.md` §4） |

### 2-5. `plugins/`（新設）— Phase D

**占術系プラグイン**

| 現行 | 移行先 |
|---|---|
| `astrology/bazi.md` | `plugins/bazi/engine.md` |
| `astrology/western_astrology.md` | `plugins/western/engine.md` |
| `astrology/six_star_style.md` | `plugins/six_star_style/engine.md` |
| `astrology/compatibility.md` | `plugins/compatibility/engine.md`（決定#2・独立Plugin） |
| `knowledge/astrology_reference.md` §1（四柱） | `plugins/bazi/reference.md`（分割・新規ファイル化） |
| `knowledge/astrology_reference.md` §2（西洋） | `plugins/western/reference.md`（分割・新規ファイル化） |
| `knowledge/astrology_reference.md` §3（六星系） | `plugins/six_star_style/reference.md`（分割・新規ファイル化） |
| `knowledge/astrology_reference.md` §4（体系横断の原則） | `core/evidence.md` または `pipeline/analysis_pipeline.md` に統合（複数体系にまたがる内容のため） |

**Analytical Plugins（決定#4：psychology / strategy）**

| 現行 | 移行先 |
|---|---|
| `psychology/personality.md` | `plugins/psychology/personality.md` |
| `psychology/cognitive_bias.md` | `plugins/psychology/cognitive_bias.md` |
| `psychology/stress_patterns.md` | `plugins/psychology/stress_patterns.md` |
| `knowledge/psychology_models.md` | `plugins/psychology/reference.md` |
| `strategy/decision_framework.md` | `plugins/strategy/engine.md`（決定#1） |
| `strategy/career.md` | `plugins/strategy/career.md` |
| `strategy/finance.md` | `plugins/strategy/finance.md` |
| `strategy/relationships.md` | `plugins/strategy/relationships.md` |
| `knowledge/strategy_frameworks.md` | `plugins/strategy/reference.md` |

**新規（雛形）**

| 現行 | 移行先 |
|---|---|
| （新規） | `plugins/_template/engine.md.template` |
| （新規） | `plugins/_template/reference.md.template` |

### 2-6. `knowledge/README.md` の扱い（退役）

`knowledge/` ディレクトリ自体が解体されるため、その説明役だった `knowledge/README.md` は**単独ファイルとしては廃止**し、
内容を `README.md` のディレクトリ構成節に要約統合する（Phase E）。

### 2-7. `templates/`・`user/`・`docs/`（ファイル自体は不動・内部参照のみ更新）— Phase E

| ファイル | 変更内容 |
|---|---|
| `templates/full_reading.md`・`annual_reading.md`・`compatibility_reading.md`・`decision_support.md` | ファイル名・配置は不動。本文中の `astrology/`・`psychology/`・`strategy/`・`system/`・`knowledge/` パス参照を新パスに更新 |
| `user/subject.md`・`user/wife.md`・`user/priorities.md` | ファイル名・配置は不動。同上のパス参照更新 |
| `docs/philosophy.md` | 不動。参照更新のみ（大きな内容変更なし） |
| `docs/methodology.md` | 不動。§1をpipeline参照に縮小（Phase Cで実施済みの前提で、Phase Eでは他パス参照のみ確認） |

---

## 3. 移行順序（Phase A〜E）

### Phase A：`logs/` 作成と監査系ファイル移動

1. `logs/` ディレクトリ作成
2. `git mv` で §2-2 の8ファイルを移動（歴史文書のため中身は書き換えない）
3. これらのファイルを**外部から参照している**箇所（`README.md`・`ROADMAP.md`・他のlogsファイル同士の相互参照）のパスのみ更新
4. 動作確認：新パスへの参照漏れがないか grep で全数確認

### Phase B：`core/` 作成と正典・規約系ファイル移動

1. `core/` ディレクトリ作成
2. `git mv` で §2-3 の8ファイルを移動
3. `docs/glossary.md` → `core/glossary.md` の移動と同時に、**理論用語（四柱推命・西洋占星術の用語集）を剥がして各プラグインの `reference.md` へ**分離（Phase Dの先取り。重複解消のため同時実施が望ましい）
4. `LIMITATIONS.md` → `core/limitations.md` の移動と同時に、`README.md`・`CLAUDE.md` から参照を追加（孤立解消）
5. 高参照数ファイル（旧 `output_rules.md` 参照15件、旧 `safety_rules.md` 参照18件）のため、**全ファイルgrepでの参照更新確認が必須**

### Phase C：`pipeline/` 作成と analysis_pipeline 仕様化

1. `pipeline/` ディレクトリ作成
2. `pipeline/analysis_pipeline.md` を新規執筆（`docs/methodology.md` §1 を昇格・9ステップ化）
3. `core/evidence.md`・`core/confidence.md` を新規執筆
4. `docs/methodology.md` §1 を `pipeline/analysis_pipeline.md` への参照に縮小

### Phase D：`plugins/` 作成と bazi/western/psychology/strategy/compatibility の移行

1. `plugins/` ディレクトリ作成（`bazi/`・`western/`・`six_star_style/`・`compatibility/`・`psychology/`・`strategy/`・`_template/`）
2. `git mv` で §2-5 の各ファイルを移動
3. `knowledge/astrology_reference.md` を3分割し、`plugins/{bazi,western,six_star_style}/reference.md` として新規作成（**分割は移動ではなく複製+削除のため、内容欠落・重複が最も起きやすいフェーズ**）
4. `strategy/decision_framework.md` → `plugins/strategy/engine.md` のリネーム移動（決定#1）
5. `plugins/_template/` を新規作成

### Phase E：`templates`・`user`・`docs` の参照更新

1. `templates/` 4ファイル・`user/` 3ファイル・`docs/philosophy.md`・`docs/methodology.md` の本文中の旧パス参照を新パスに更新
2. `knowledge/README.md` の内容を `README.md` に統合し、`knowledge/README.md` を削除
3. 空になった `system/`・`astrology/`・`psychology/`・`strategy/`・`knowledge/` ディレクトリを削除
4. `README.md`・`ROADMAP.md`・`CLAUDE.md` §6（読む順序）を新構造に合わせて更新
5. `CHANGELOG.md` に移行完了エントリを追記

---

## 4. 各Phaseごとのリスク

| Phase | リスク水準 | 主なリスク |
|---|---|---|
| A | **低** | 参照元は `README.md`・`ROADMAP.md`・logs相互参照のみ。件数が少なく機械的に潰せる |
| B | **中〜高** | `output_rules.md`(参照15)・`safety_rules.md`(参照18)・`calculation_config.md`(参照13) など**リポジトリ内で最も参照される8ファイル**を一度に動かす。参照更新漏れの影響範囲が全体に及ぶ |
| C | **低〜中** | 新規ファイルが中心で既存参照を壊すリスクは低いが、`analysis_pipeline.md` が `docs/methodology.md` と内容重複を起こすリスク（新設のたびに重複を作った過去のパターンの再発） |
| D | **最高** | ①ファイル数最多、②`astrology_reference.md` の3分割という「移動でなく複製」作業を含み内容欠落・重複の危険が最大、③`decision_framework.md`だけ`engine.md`にリネームする非対称な命名で混乱しやすい |
| E | **中** | templates/userは「動かないファイルだが参照が最も多い」ため、Phase A〜Dの漏れがここで一気に露呈する。`knowledge/README.md`統合は情報欠落リスクあり |

---

## 5. ロールバック方法

1. **`git mv` を使用し、移動と参照更新は同一コミットに含める。** これにより `git revert <commit>` 一発で「ファイル位置」と「参照内容」の両方を移動前の状態に戻せる。
2. **Phase単位でコミットを分割する**（§8）。あるPhaseで問題が見つかった場合、そのPhaseのコミットのみを `git revert` し、それ以降のPhaseは着手しない。
3. **旧ディレクトリ（`system/`・`astrology/`等）の削除は Phase E の最後まで行わない。** Phase A〜Dでは「新パスにコピー＋旧パスは残す」ではなく `git mv` によるファイル移動を用いるため旧ディレクトリは自然に空になるが、**空ディレクトリの明示的な削除操作自体は最終確認後に行う**（git は空ディレクトリを追跡しないため実質的にはPhase D完了時点で消えるが、削除確認はPhase Eで行う）。
4. 各Phase完了時に `logs/QA_CHECKLIST.md` の手法（grep実測）で参照切れが無いことを確認してから次Phaseに進む。**確認前に次Phaseへ進まない。**
5. どうしても切り戻しが必要な場合、**このブランチ（`claude/fortune-os-initial-setup-3l4v7m`）はまだ独立ブランチであり main には影響しない**ため、最悪の場合ブランチごと移行前のコミットに `git reset --hard <移行前コミット>` する選択肢もある（ただし通常操作では推奨せず、Phase単位revertを優先する）。

---

## 6. 参照更新チェックリスト（各Phase共通で使う）

Phase実行後、以下を**grepで実測**する（`QA_CHECKLIST.md` と同じ手法）：

- [ ] 移動元の旧パス文字列（例：`system/output_rules.md`）が、**logs/ 配下の歴史文書を除いて**リポジトリ内に残っていないか
- [ ] 移動先の新パスが、移動したファイル自身の自己参照（例：ファイル冒頭のパス表記）と一致しているか
- [ ] 節番号参照（`§1`・`§3-2`等）が、移動によってズレていないか（内容移動のみで番号は変えない前提だが要確認）
- [ ] `core/glossary.md` の用語と、各 `plugins/*/reference.md` の用語で**再定義の重複が新たに発生していないか**（Phase D の最重要チェック。過去のKnowledge C評価の再発防止）
- [ ] `README.md`・`ROADMAP.md`・`CLAUDE.md` の構成図・読む順序・優先順位（§0）が新パスと一致しているか
- [ ] `CHANGELOG.md` に該当Phaseの変更が記録されているか
- [ ] 各プラグインが `core/evidence.md` のタグ契約・`core/calculation_config.md` の設定に矛盾していないか

---

## 7. 移行後QAチェック項目（全Phase完了後・`logs/QA_CHECKLIST.md` 準拠の再監査）

`logs/QA_CHECKLIST.md` の6カテゴリで再監査を行う。移行特有の追加観点：

- **Architecture：** `core/pipeline/plugins` の依存方向が一方向か（plugin同士が直接参照し合っていないか）。新設4ファイル（`core/evidence.md`・`core/confidence.md`・`pipeline/analysis_pipeline.md`・`plugins/_template/`）がREADME/ROADMAP/CLAUDEから参照されているか
- **Calculation：** `calculation_config.md` 移動後も参照が全て解決しているか
- **Knowledge：** `astrology_reference.md` 分割後、**用語の重複・欠落**が発生していないか（分割は最もリスクの高い操作）
- **Output：** テンプレート4種の内部参照（プラグインパス）が正しく更新され、7レイヤー構造自体は無傷か
- **Audit：** `CHANGELOG.md` に移行履歴が反映されているか。`logs/` 配下の歴史文書が誤って書き換えられていないか
- **Maintainability：** 空になった旧ディレクトリ（`system/`・`astrology/`・`psychology/`・`strategy/`・`knowledge/`）が残っていないか。孤立ファイル・DRY違反が新たに発生していないか

---

## 8. commit分割案

| # | コミット内容 | 対応Phase |
|---|---|---|
| 1 | `chore: create logs/ and relocate audit/review/plan docs` | Phase A |
| 2 | `refactor: create core/ and relocate policy/spec docs; split glossary theory terms out` | Phase B |
| 3 | `feat: add pipeline/analysis_pipeline.md, core/evidence.md, core/confidence.md` | Phase C |
| 4 | `refactor: create plugins/ and migrate astrology/psychology/strategy into plugin units` | Phase D |
| 5 | `fix: update templates/user/docs cross-references to v0.9 structure; retire knowledge/README.md` | Phase E |
| 6 | `docs: update README/ROADMAP/CLAUDE.md navigation for v0.9; add CHANGELOG entry` | Phase E仕上げ |
| 7 | `docs: post-migration QA audit (v0.9)` | 全Phase完了後の再監査結果 |

**コミット間に必ず §6 チェックリストでの確認を挟む。** 確認せずに次のコミットに進まない。

---

## 9. 実行禁止事項

1. **一括全面移動禁止。** Phase A〜Eを1コミット・1回の作業でまとめて行わない。必ずPhase単位でコミットを分け、都度チェックリスト（§6）で確認する。
2. **実計算データ投入禁止。** 本移行はファイル配置の変更のみ。`未計算`表記の値を`[計算済]`に変える作業（v0.3の範囲）は本移行に含めない。移行完了まで占術データの状態は一切変えない。
3. **未決定事項の勝手な確定禁止。** 移行作業中に新たな設計判断が必要な論点（例：`astrology_reference.md` §4の具体的な統合先）が見つかった場合、その場で独断で決めず、`logs/DECISION_LOG.md` 形式で論点化し、ユーザーに確認を取ってから進める。

---

## 10. 推奨コミット順（最終）

```
1. Phase A → commit 1 → §6チェック → 承認
2. Phase B → commit 2 → §6チェック（特に output_rules/safety_rules 参照） → 承認
3. Phase C → commit 3 → §6チェック（methodology.mdとの重複有無） → 承認
4. Phase D → commit 4 → §6チェック（astrology_reference分割の欠落有無・最重要） → 承認
5. Phase E → commit 5, 6 → §6チェック（全体参照の最終確認）
6. 全Phase完了後 → commit 7（§7の再監査結果を logs/QA_CHECKLIST.md に追記する形で記録）
```

**Phase B と Phase D の完了後は、次のPhaseに進む前に必ずユーザーの確認を挟むことを推奨する**
（この2つが最もリスク水準が高いため）。

**本計画はここまで。実行はユーザーの承認後、別セッション/別指示で着手する。**
