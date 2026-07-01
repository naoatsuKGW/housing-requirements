# QA_CHECKLIST.md

Fortune-OS **品質保証チェックリスト**（v1・今後のどのバージョンでも使う共通様式）。

> 本ファイルは監査・品質保証文書であり、7レイヤータグの適用対象外（`CLAUDE.md` §2 の精神に準拠）。
> **使い方：** 新バージョンをコミットする前に、このチェックリストの様式で6カテゴリを実際に検証し、
> 「実施結果」欄に証拠（ファイル・行）付きで記録する。過去の実施結果は消さず、下に追記していく
> （`DECISION_LOG.md`・`CALCULATION_AUDIT.md` と同じ「追記型ログ」の思想）。

---

## 監査対象・実施日

- 対象バージョン：v0.2.5（コミット前）
- 実施日：2026-07-01
- 対象ファイル数：39（`Fortune-OS/` 配下 `.md` 全ファイル）
- 方法：全ファイル読了＋grep による相互参照・重複定義・断定表現の実地検査（推測や記憶によらない）

---

## Architecture

- [x] **CLAUDE.md が唯一の正典になっているか**
  → **合格。** `CLAUDE.md` §0 が優先順位の単一ソース。`system_prompt.md`・`safety_rules.md`・`README.md`・`ROADMAP.md`・`LIMITATIONS.md` は全て「正典はCLAUDE.md §0」と参照するのみで、独自の優先順位を主張していない（grep で実測・矛盾なし）。
- [x] **優先順位の循環参照がないか**
  → **合格。** `CLAUDE.md`→`safety_rules`→`system_prompt`→`output/quality`→`user`→`astrology/psychology/strategy`→`knowledge`→`templates` の一方向。逆流参照は検出されず。
- [ ] **ファイル構成に責務の重複がないか**
  → **不合格（新規発見）。** v0.2.5で追加した4ファイル（`SOURCE_POLICY.md`・`CALCULATION_AUDIT.md`・`DECISION_LOG.md`・`LIMITATIONS.md`）が、`README.md` のディレクトリ構成・`ROADMAP.md` のマイルストーン・`CLAUDE.md` §6 読む順序の**どこにも記載されていない**。特に `LIMITATIONS.md` は他のどのファイルからも参照されておらず（grep で参照数0を確認）、`safety_rules.md`／`docs/philosophy.md` との責務の線引きが宣言されているだけで、実際には接続されていない「浮いた要約」になっている。

---

## Calculation

- [x] **計算済／未計算／未検証／推測 が混在していないか**
  → **合格（v0.2で是正済み）。** `docs/glossary.md` §1 が定義の正典。全ファイルgrepで確認した限り、astrology系・templates系の「未検証」表記は出生時刻など前提未確認の対象にのみ使われ、四柱・ハウス等の未計算データには一貫して「未計算」が使われている。`psychology/*.md` の「未検証」は心理仮説（現実照合前）を指す別文脈での使用であり、定義と矛盾しない。
- [x] **calculation_config.md と矛盾がないか**
  → **合格。** `SOURCE_POLICY.md`・`DECISION_LOG.md` の設定値（JST・トロピカル・Placidus・節気基準）は `knowledge/calculation_config.md` と全て一致（grep で実測）。捏造・架空の計算結果も無い（`CALCULATION_AUDIT.md` の記入例は「架空データ」と明記済み）。
- [x] **出典必須項目が欠けていないか**
  → **合格。** `[計算済]` の出典書式（ツール名・バージョン／設定・算出日）が `output_rules.md` §3-2・`calculation_config.md` §5・`SOURCE_POLICY.md` §5 の3箇所で完全に一致した書式として定義されている。

---

## Knowledge

- [ ] **glossary が唯一の用語定義になっているか**
  → **不合格（新規発見・重要）。**
- [ ] **同じ定義が複数ファイルに存在しないか**
  → **不合格（新規発見・重要）。**

  **証拠：** `docs/glossary.md` §4-5（四柱推命・西洋占星術用語）と `knowledge/astrology_reference.md` §1-2 に、
  ほぼ同一の用語定義が**重複して存在**する。

  | 用語 | glossary.md | astrology_reference.md |
  |---|---|---|
  | 日主（日干） | §4「日柱の天干。本人を表す中心」 | §1-2「本人を表す中心」 |
  | 通変星 | §4「日主から見た他の干の役割」 | §1-2「日主から見た他干の役割」 |
  | 蔵干 | §4「地支に内蔵される天干」 | §1-2「地支に内蔵される天干」（**文言ほぼ完全一致**） |
  | 用神／喜忌 | §4「命式のバランスを取るために必要とされる五行」 | §1-2「命式の五行バランスを取るために必要とされる要素」 |
  | 大運／年運 | §4 | §1-2 |
  | ASC・ハウス | §5 | §2-1 |

  これは今回のユーザー指摘（「正典を一つにする」）が的中した実例。**知識層とタグ用語集の役割分担が未定義**なまま両方に書いてしまっている。

---

## Output

- [x] **7レイヤーが分析テンプレートに適用されているか**
  → **合格。** `templates/` 全4種（full/annual/compatibility/decision_support）すべてに、計算状態宣言ブロック・タグ付き本文・信頼度サマリの3ブロック構造が存在する。
- [x] **推測が断定になっていないか**
  → **合格。** `astrology/`・`psychology/`・`strategy/`・`user/`・`templates/` 全文を「確実に」「間違いなく」「必ず〜になる」でgrepしたが該当なし。
- [x] **辛口表現が人格否定になっていないか**
  → **合格。** 「あなたは〜だ／〜な人間」型の表現を全分析ファイルでgrepしたが該当なし。`system_prompt.md` §4 の橋渡しルール（辛口の強度は証拠の強度に比例）も維持されている。
- [ ] **（追加観点）出力テンプレート間でブロックA（計算状態宣言）の書式が統一されているか**
  → **不合格（既知の未解消 Minor・m2）。** `full_reading.md` は「■ 計算状態」見出し＋4項目、`annual_reading.md` は見出しなし＋5項目、`compatibility_reading.md` は「未検証」の書き方が他と異なる、`decision_support.md` には「未検証の前提」欄が無い。4テンプレートで細部の様式が揃っていないことを実地確認。

---

## Audit

- [x] **CALCULATION_AUDIT に記録方法があるか**
  → **合格。** 記入フォーマット・不一致時の扱い・信頼度の付け方まで定義済み。現時点でログは空（実計算未実施のため正しい状態）。
- [x] **DECISION_LOG に理由が残るか**
  → **合格。** 確定済み5件（D1〜D5）・未決定6件（D6〜D11）すべてに「理由」欄が記入されている。
- [ ] **CHANGELOG と整合するか**
  → **不合格（新規発見・重要）。** `CHANGELOG.md` は `[0.1.0]` のエントリしか無く、v0.2（権威の単一ソース化・タグ適用範囲・未計算/未検証の定義統一 等、17ファイル変更）も、v0.2.5（`SOURCE_POLICY.md`・`CALCULATION_AUDIT.md`・`DECISION_LOG.md`・`LIMITATIONS.md`・`PLAN_v0.3_calculation.md` の新設）も**一切記録されていない**。実際に行った変更履歴と `CHANGELOG.md` が乖離している。

---

## Maintainability

- [ ] **DRY違反**
  → **不合格（既知の未解消 Minor・m5）。** 「出生時刻が未検証のため、時刻依存要素（時柱・ASC・ハウス・月の度数）の信頼度を下げる」という一文が、`user/subject.md`・`user/wife.md`・`astrology/western_astrology.md`・`knowledge/astrology_reference.md`・`knowledge/calculation_config.md`・`system/system_prompt.md`・`docs/methodology.md`・templates 2種など**12ファイルに独立して重複記載**されている（grep で実測）。正典を1箇所（例：`system_prompt.md` §2 か `docs/methodology.md`）に定め、他は参照に変えるべき状態が続いている。
- [x] **デッドファイル（使われていない残骸）**
  → **該当なし。** 39ファイルすべてに存在意義があり、内容の無い残骸ファイルは無い。
- [ ] **孤立ファイル（どこからも参照されない）**
  → **不合格（新規発見）。** `LIMITATIONS.md` が他のどのファイルからも参照されていない（grep で参照数0を確認・Architecture項と同一事象）。
- [x] **相互リンク切れ**
  → **合格。** `glossary.md`・`output_rules.md` 等、v0.2で節番号が振り直されたファイルへの参照を全数チェックしたが、古い番号を指す壊れた参照は検出されなかった。

---

## 採点

| カテゴリ | 判定 | 評価 |
|---|---|---|
| Architecture | 3項目中1項目不合格（新規追加ファイルが構成図・読む順序に未統合） | **B** |
| Calculation | 全項目合格 | **A** |
| Knowledge | 2項目とも不合格（用語定義の重複が実在） | **C** |
| Output | 主要3項目合格・追加観点1件不合格（テンプレ書式不統一） | **B** |
| Audit | 2項目合格・CHANGELOG不整合1件不合格 | **B**（CHANGELOGの乖離が実害に近いため厳しめ） |
| Maintainability | DRY違反・孤立ファイルの2件不合格 | **C** |

### 総合評価：**B**（B寄りのB。CまでではないがA帯には届かない）

---

## v1.0へ進んでよいか：**否（Not yet）**

**判定理由：**
Fortune-OS の設計思想・権威構造・計算ルールの骨格（Architecture の根幹、Calculation 全般）はすでに**A〜B相当で安定**しており、
これ自体は長期運用に耐える水準にある。しかし v1.0（「運用可能な意思決定OS」）を名乗るには、
今回 **Knowledge と Maintainability で C評価の実害（重複定義・孤立ファイル・DRY違反）**が確認されており、
これらは「正典を一つにする」というユーザー自身の指摘そのものに該当する。実データを計算する v0.3 に進む前に、
**ファイルを増やす作業を止め、次の1回は統合・削減のパスに充てるべき。**

**v1.0前に必須の是正（優先順）：**
1. `knowledge/astrology_reference.md` の用語定義を `docs/glossary.md` への参照に置き換え、重複を除去する（Knowledge C の解消）。
2. `CHANGELOG.md` に v0.2・v0.2.5 のエントリを追記し、実際の変更履歴と一致させる（Audit の是正）。
3. `README.md` のディレクトリ構成・`ROADMAP.md`・`CLAUDE.md` §6 に、v0.2.5 で追加した4ファイルを組み込む（Architecture の是正）。
4. 「時刻依存→信頼度を下げる」の12ファイル重複を1箇所の正典＋他は参照に統合する（Maintainability の是正・既存 Minor m5）。
5. `LIMITATIONS.md` を実際にどこか（`README.md` 使い方 or `CLAUDE.md` 読む順序）から参照させ、孤立を解消する。
6. テンプレート4種のブロックA書式を統一する（既存 Minor m2）。

**是正後の見込み：** 上記6件はいずれも「新規作成」ではなく「統合・参照化」であり、ファイル数は増えない。
これが完了すれば Knowledge・Maintainability・Architecture が B以上に上がり、v1.0判定の再監査に進める状態になる見込み。

---

## 次回監査に向けて（本チェックリストの運用）

- 次回もこの様式（6カテゴリ・チェックボックス・証拠付き・採点表・v1.0判定）を使い、下に新しい「実施結果」ブロックを追記する。
- 過去の実施結果は上書きせず残す（経緯を追えるようにするため。`DECISION_LOG.md`・`CALCULATION_AUDIT.md` と同じ思想）。
