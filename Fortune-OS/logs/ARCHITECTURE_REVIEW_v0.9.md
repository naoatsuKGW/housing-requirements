# ARCHITECTURE_REVIEW_v0.9.md

Fortune-OS **v0.9 アーキテクチャレビュー**（設計提案のみ・実装なし）。

> **本ファイルは提案文書。ファイルの移動・リネーム・実装は一切行っていない。**
> 目的：v1.0に進む前に、「占術を増やすたびに破綻する構造」を「Core／Plugin／Pipeline／Evidence／Confidence」の
> 5層構造にリファクタリングする計画を固める。承認後、別途 `MIGRATION_v0.9.md`（実行計画）→ 実装、の順で進める想定。
> 本ファイルは仕様・設計文書であり、7レイヤータグの適用対象外（`CLAUDE.md` §2 の精神に準拠）。

---

## 0. 診断：なぜ今のままだと破綻するか

### 0-1. 現状の依存関係（逆転している）

```
現状：
Fortune-OS
  └─ system/ (仕様・ルール)
       └─ astrology/ psychology/ strategy/ (個別の体系がフラットに並ぶ)
```

`astrology/` はすでに4ファイル（bazi / western_astrology / six_star_style / compatibility）。
ここに紫微斗数・宿曜・数秘術・易・九星気学が来ると、`astrology/` は無秩序なフォルダになる。
さらに厄介なのは、**体系ごとの「理論」（`knowledge/astrology_reference.md`）と「適用」（`astrology/*.md`）が別ディレクトリに分散**しており、
体系を1つ追加するたびに `knowledge/` と `astrology/` の**両方**に手を入れる必要がある（今回の `QA_CHECKLIST.md` Knowledge評価Cの重複問題もここに起因）。

### 0-2. パイプラインが「文章」としてしか存在しない

`docs/methodology.md` §1 に、実は既にパイプラインの原型がある：

```
[事実]収集 → [計算済] → [占術解釈] → [推測] → [現実照合] → [反証] → [信頼度] → 意思決定の選択肢
```

しかしこれは**散文で書かれた一例**であり、「体系間の一致率」「行動提案への変換」のステップが独立して定義されていない。
毎回の出力がこの順序を厳密になぞる保証もない。

### 0-3. 信頼度が定性的なまま止まっている

`[信頼度:高/中/低]` は各記述に個別に付くだけで、**複数体系を横断した一致率をスコア化する仕組みがない**。
「西洋は一致、四柱は不一致、心理は△ → 総合42%」のような**定量的な合成**が存在しない。

### 0-4. 結論

このまま v1.0 に進むと、体系を1つ追加するたびに `astrology/`・`knowledge/`・`templates/`・`docs/methodology.md` の
4箇所以上を手で直す必要が生じ、`QA_CHECKLIST.md` が指摘した重複・DRY違反を体系的に量産する構造になっている。
**依存の向きを反転させる必要がある。**

---

## 1. 提案する5層アーキテクチャ

```
Fortune-OS
  ├─ core/        … 体系に依存しない土台（権威・タグ規約・証拠の型・信頼度算出・推論の型）
  ├─ pipeline/     … 分析の実行順序（Core と Plugin をどの順で呼ぶか）
  ├─ plugins/      … 占術・心理・戦略。体系ごとに自己完結した1ユニット
  ├─ user/         … 対象者データ（変更なし）
  └─ templates/    … 出力フォーマット（Pipeline + Plugin の結果を受けて整形。変更なし）
```

**依存の向き：** `templates` → `pipeline` → `plugins` → `core`。
`core` は何にも依存しない。`plugins` は `core` の型に従うだけで、他の `plugins` を知らない
（四柱プラグインが西洋プラグインを直接参照しない。両者の統合は `pipeline` 側の仕事）。

これにより、新しい体系（紫微斗数等）の追加は **`plugins/` に1ユニット足すだけ**で完結し、
`core`・`pipeline`・`templates` には触れない設計にする。

---

## 2. 各層の定義と既存資産のマッピング

### 2-1. `core/`（体系に依存しない土台）

| 新ファイル（提案） | 中身 | 移行元（既存） |
|---|---|---|
| `core/authority.md` | 優先順位の正典（現 `CLAUDE.md` §0 相当） | `CLAUDE.md`（**CLAUDE.md自体はリポジトリ直下に残す**。§0の内容は core/authority.md に実体を持たせ、CLAUDE.mdからは参照するだけにする案／または現状維持でCLAUDE.mdをcoreの一部として直下据え置きも可、要協議） |
| `core/system_prompt.md` | 実行プロンプト | `system/system_prompt.md`（そのまま移動） |
| `core/output_rules.md` | 出力3ブロック構造・タグ書式 | `system/output_rules.md`（そのまま移動） |
| `core/quality_rules.md` | 自己採点基準 | `system/quality_rules.md`（そのまま移動） |
| `core/safety_rules.md` | 安全補助規則 | `system/safety_rules.md`（そのまま移動） |
| `core/glossary.md` | 7タグ・未計算/未検証の定義（正典） | `docs/glossary.md`（そのまま移動。**理論用語（四柱・西洋の用語集§4-5）はここから剥がして各plugin/reference.mdへ移す**＝重複解消と同時実施） |
| `core/evidence.md`（新規） | 「証拠の型」の形式化：`[事実]/[計算済]/[占術解釈]/[推測]/[現実照合]/[反証]` の相互関係と、Plugin側が守るべき出力契約 | 新規（`system_prompt.md` §1・`output_rules.md` §1 の内容を型として抽出・一本化） |
| `core/confidence.md`（新規） | 信頼度の**定量化**モデル（§4で詳述） | 新規（`quality_rules.md` §4 の定性基準を土台に拡張） |
| `core/reasoning.md`（新規、または `decision_framework.md` を移設） | Plugin横断で「事実→占術解釈→一致率→推測→現実照合→反証→結論→行動提案」に変換する推論規則 | `strategy/decision_framework.md` を移設 + 一致率ステップを追加 |

> **論点：** `strategy/decision_framework.md` は「戦略プラグイン」なのか「core の推論エンジン」なのか。
> ユーザーの図では plugins 側に「戦略」も並んでいるが、decision_framework は**全プラグイン共通の出口**であり、
> 特定体系（キャリア／財務／関係性）に依存しない。**`core/reasoning.md` に格上げし、`career.md`/`finance.md`/`relationships.md` のみを plugin 側の "strategy" プラグインとして残す**ことを提案する（要承認）。

### 2-2. `pipeline/`（分析の実行順序）

| 新ファイル（提案） | 中身 | 移行元 |
|---|---|---|
| `pipeline/analysis_pipeline.md`（新規） | Step1〜9 の正式定義（§3で詳述） | `docs/methodology.md` §1 の**昇格・正式版**。methodology.md 側は「詳細解説」として残すか、本ファイルへの参照に縮小する（重複回避のため後者を推奨） |

### 2-3. `plugins/`（体系ごとに自己完結した1ユニット）

各プラグインは同じ内部構造を持つ（§4「Plugin契約」で規定）。

| 新プラグイン | 中身 | 移行元 |
|---|---|---|
| `plugins/bazi/engine.md` | 四柱推命の適用ロジック | `astrology/bazi.md` |
| `plugins/bazi/reference.md` | 四柱推命の理論・用語 | `knowledge/astrology_reference.md` §1 を分離 |
| `plugins/western/engine.md` | 西洋占星術の適用ロジック | `astrology/western_astrology.md` |
| `plugins/western/reference.md` | 西洋占星術の理論・用語 | `knowledge/astrology_reference.md` §2 を分離 |
| `plugins/six_star_style/engine.md` | 六星占術系の適用ロジック | `astrology/six_star_style.md` |
| `plugins/six_star_style/reference.md` | 六星系の理論 | `knowledge/astrology_reference.md` §3 を分離 |
| `plugins/compatibility/engine.md` | 相性分析（複数プラグイン横断） | `astrology/compatibility.md`（※唯一「複数プラグインを跨ぐ」特殊プラグイン。§6で別途扱いを協議） |
| `plugins/psychology/*.md` | 性格・バイアス・ストレス | `psychology/personality.md`・`cognitive_bias.md`・`stress_patterns.md`（そのまま移動） |
| `plugins/psychology/reference.md` | 心理モデルの理論 | `knowledge/psychology_models.md` を移設 |
| `plugins/strategy/career.md`・`finance.md`・`relationships.md` | 各戦略領域の適用 | `strategy/career.md`・`finance.md`・`relationships.md`（そのまま移動） |
| `plugins/strategy/reference.md` | 戦略フレームワークの理論 | `knowledge/strategy_frameworks.md` を移設 |
| `plugins/_template/`（新規） | 新規プラグイン追加用の雛形（`engine.md.template`・`reference.md.template`） | 新規作成。紫微斗数・宿曜・数秘術・易・九星気学を追加する際、このディレクトリをコピーして名前を変えるだけで着手できるようにする |

### 2-4. 計算設定・出典系（cross-plugin infrastructure）

| ファイル | 扱い |
|---|---|
| `knowledge/calculation_config.md` | `core/calculation_config.md` へ移設（全プラグイン共通のJST/黄道/ハウス方式等の設定であり、特定プラグインに属さないため core が適切） |
| `SOURCE_POLICY.md` | 同様に `core/source_policy.md` へ（プラグイン横断の出典優先順位） |
| `CALCULATION_AUDIT.md` / `DECISION_LOG.md` / `QA_CHECKLIST.md` / `LIMITATIONS.md` / `REVIEW_*.md` / `PLAN_*.md` | **リポジトリ直下に残す。** これらはプロジェクト運営・監査の記録であり、layer構造（core/plugin/pipeline）のいずれにも属さない「メタ記録」として扱う |

### 2-5. 変更なし

- `CLAUDE.md`・`README.md`・`ROADMAP.md`・`CHANGELOG.md`：リポジトリ直下のまま
- `user/`：対象者データ。プラグインに依存しないためそのまま
- `templates/`：出力層。Pipeline実行後の整形フォーマットとしてそのまま

---

## 3. `pipeline/analysis_pipeline.md` の設計案

`docs/methodology.md` §1 の7ステップを、**明示的な9ステップ**に拡張して正式版とする
（心理分析・行動提案・一致率を独立ステップとして切り出す）。

```
Step1  計算        … core/calculation_config.md の設定でプラグインが値を算出（[計算済] or 未計算宣言）
Step2  事実抽出     … user/ から [事実] を収集。占術と混ぜない
Step3  占術解釈     … 各プラグインが独立に読む（体系名必須。矛盾は統合しない）
Step4  一致率評価   … core/confidence.md の型で、プラグイン間の一致/不一致をスコア化（新設ステップ）
Step5  心理分析     … plugins/psychology が Step1-4 とは独立レンズとして評価（新設ステップとして分離）
Step6  現実照合     … user/*.md §4 の実データと突き合わせ
Step7  反証         … 意識的に反証を探す（見つからない場合もその旨を明記）
Step8  結論（信頼度確定） … core/confidence.md で最終スコアを確定
Step9  行動提案     … core/reasoning.md（旧decision_framework）で選択肢・トリガー条件に変換
```

- 各ステップの出力は `core/evidence.md` のタグ契約に従う。
- `docs/methodology.md` は本ファイルへの参照に縮小し、内容の重複を持たせない（実行フェーズで対応）。

---

## 4. `core/confidence.md` の設計案（信頼度の定量化）

### 4-1. 現状（定性のみ）
`[信頼度:高/中/低]` が個別の記述に付くのみで、体系横断の合成スコアが無い。

### 4-2. 提案：一致率スコアリングモデル

各プラグインの読みを「対象の論点」ごとに一致／不一致／部分一致で評価し、重み付き合成する。

```
論点：独立適性は高いか
- 西洋占星術（plugins/western）    ： 一致 ✓
- 四柱推命（plugins/bazi）         ： 不一致 ✗
- 心理分析（plugins/psychology）   ： 部分一致 △
- 現実照合（user/*.md §4 実データ）： （最優先。あれば他を上書きする重み）

合成：
  一致=1.0, 部分一致=0.5, 不一致=0 として単純平均 → 一致率%
  現実照合が存在する論点は、現実照合の結果を最優先重みとして扱う
  （`docs/philosophy.md` §3「現実照合が最上位」の原則をスコアに反映）

出力：
  Confidence: 42%（西洋✓ 四柱✗ 心理△、現実照合データなし）
  Confidence: 95%（西洋✓ 四柱✓ 心理✓、現実照合も一致）
```

- **原則：現実照合データが無い論点は、一致率が高くても Confidence の上限を「中」までに制限する**
  （`quality_rules.md` §4「信頼度を盛らない」を定量モデルにも継承）。
- 星取り表示（★1〜5）と％表示を併記してよいが、**★5＝現実照合込みで全一致の場合のみ**とする。

### 4-3. 未決定事項
- 部分一致の重み（0.5固定か、プラグインごとに調整可能か）
- プラグイン数が増えた場合（5体系以上）の合成式の扱い（単純平均のままか、加重平均にするか）
- これらは `DECISION_LOG.md` 形式で別途決定する（本レビューでは決定しない）

---

## 5. Plugin契約（新規体系を追加するときの型）

新しい占術（例：紫微斗数）を追加する場合、**以下だけを満たせば `core`・`pipeline`・`templates` に一切触れずに追加できる**ことを目標とする。

```
plugins/<system_name>/
├── engine.md       … 適用ロジック。対象者への計算・解釈。core/evidence.md のタグ契約に従う
├── reference.md     … 理論・用語（人物非依存）。core/glossary.md の用語と重複させない
└── status.md（任意）… 計算済/未計算の状態一覧（現行 astrology/*.md の「未計算」表がこれに相当）
```

**契約条件（プラグインが守るべきこと）**
1. `core/evidence.md` の7タグ・体系名必須ルールに従う
2. `core/calculation_config.md` の共通設定（JST・黄道・ハウス方式等）に反する場合は理由を明記
3. 他プラグインを直接参照しない（プラグイン間の統合は `pipeline/` と `plugins/compatibility/` が担う）
4. `reference.md` に書く用語が `core/glossary.md` に既にある場合は再定義せず参照する（**今回のKnowledge C評価の再発防止**）
5. `SOURCE_POLICY.md`（→ `core/source_policy.md`）の優先順位表に、新体系の情報源信頼度を追記する

---

## 6. 未決定・要協議事項（実行前に決めるべきこと）

1. `strategy/decision_framework.md` は `core/reasoning.md` に格上げするか、`plugins/strategy/` に残すか（§2-1参照）
2. `astrology/compatibility.md`（複数プラグイン横断の相性分析）は独立プラグインか、`pipeline/` の一部（Step4一致率評価の応用）か
3. `CLAUDE.md` の内容（特に §0 優先順位）を `core/` に実体移動するか、直下に残したまま `core/` からは参照のみにするか
4. `psychology/` と `strategy/` を本当に「plugin」と呼ぶか（占術と同列に扱うことの是非。ユーザー案では同列だが、性質が異なる可能性）
5. ディレクトリ名を `plugins/` とするか、他の呼称（`engines/` 等、ユーザー案にも両方の表現が登場）にするか

---

## 7. 移行の順序（推奨・実行フェーズ用の下書き）

**推奨：v0.3（実データ計算投入）より先に本リファクタリングを行う。**
理由：先に `astrology/bazi.md` 等へ実データを投入してしまうと、直後に `plugins/bazi/engine.md` への移動で
また出典・タグごと書き直す二度手間になる。構造を先に固めてから計算データを流し込む方が手戻りがない。

想定シーケンス（実行フェーズで詳細化）：
1. `core/glossary.md` を正典としつつ `plugins/*/reference.md` へ理論用語を分離（Knowledge C評価の解消と同時実施）
2. `core/`・`pipeline/`・`plugins/` のディレクトリを作成し、既存ファイルを移動（内容変更なし、パスのみ）
3. 全ファイルの相互参照（`grep`で全数チェック済みの手法を流用）を新パスに更新
4. `analysis_pipeline.md`・`confidence.md`・`reasoning.md`・`evidence.md` を新規執筆
5. `README.md`・`ROADMAP.md`・`CLAUDE.md` §6 の構成図・読む順序を新構造に合わせて更新
6. `QA_CHECKLIST.md` の様式で再監査（特にArchitecture・Knowledge・Maintainabilityの再採点)
7. 上記が完了して初めて v0.3（実計算）・v1.0 判定に進む

---

## 8. 本レビューでの結論

- ユーザー提案の**依存方向の反転（Core→Plugin）は妥当**。現状の `astrology/` フラット構造は、体系追加のたびに
  `knowledge/`・`docs/methodology.md`・`templates/` へ波及する設計になっており、拡張性に欠ける。
- **Pipeline（analysis_pipeline.md）と Confidence（confidence.md）は実質的に新規要素**だが、
  `docs/methodology.md` §1 と `quality_rules.md` §4 に原型があるため、**ゼロからの新設ではなく既存資産の昇格**として位置づけられる。
- Plugin契約に「`core/glossary.md` の用語を再定義しない」を明記することで、今回の `QA_CHECKLIST.md` Knowledge評価Cの
  再発を構造的に防止できる。
- **未決定事項が5点**（§6）残っており、これらはユーザー（設計者）の判断が必要。本レビューでは決定していない。
- 実行順序として、**v0.3（実計算）より先にこのリファクタリングを行うことを推奨**。

**次のステップ（提案）：** §6の未決定事項を確定した上で、`MIGRATION_v0.9.md`（具体的なファイル移動コマンド・
新規ファイルの本文・全参照の書き換え内容）を作成し、レビューを経てから実行に移す。
