# REVIEW_v0.2 修正ログ（FIX LOG）

対象レビュー：`REVIEW_v0.2.md`
対応範囲：**Critical 全件 ＋ Major 全件**（Minor は今回対象外）
実施日：2026-07-01
状態：ファイル修正済み・**未コミット**（`REVIEW_v0.2.md` は残置）

方針（ユーザー指示）に沿って対応：
①C1 最優先 ②CLAUDE.md を唯一の最上位 ③system_prompt=実行プロンプト／safety=安全補助規則（最上位を名乗らせない）
④7レイヤーは分析出力に必須・仕様文書は対象外を明記 ⑤未計算/未検証/推測/計算済 の定義統一
⑥v0.3 実計算の設定（JST・出生地・黄道・ハウス・節入り・真太陽時・出典書式）定義 ⑦辛口トーンの橋渡し

---

## Critical

### C1 最上位権威の二重定義 → ✅ 修正済み
- `CLAUDE.md`：冒頭を「**唯一の最上位運用ルール**」に。safety=従属する安全補助規則、system_prompt=従属する実行プロンプトと明記。
- `CLAUDE.md` に **§0 優先順位（正典・単一ソース）** を新設：
  `[1]CLAUDE > [2]safety（安全事項では絶対）> [3]system_prompt > [4]output/quality > [5]user→astrology/psychology/strategy→knowledge→templates`
- 「読む順序」（§6）と「強さの順序」（§0）は別軸、と明記。
- `system_prompt.md`：ヘッダから「最上位」を削除し「実行プロンプト・CLAUDE/safety に従属」に。§8 を §0 参照＋CLAUDE 明記に修正。
- `safety_rules.md`：ヘッダを「安全補助規則・CLAUDE に従属・最上位を名乗らない・安全事項では絶対」に。§7 を §0 参照に修正。

---

## Major

### M1 7レイヤータグの適用範囲が未定義 → ✅ 修正済み
- `CLAUDE.md` §2：「**占断出力・分析出力に必須**。docs/system/knowledge/ROADMAP/CHANGELOG は仕様・理論・メタ文書＝適用対象外（要約適用可）。理論を人物に適用した時点でタグ必要」を明記。
- `system_prompt.md` §1・`output_rules.md` 冒頭・`knowledge/README.md` にも同趣旨を追記。

### M2 未計算／未検証の混同 → ✅ 修正済み
- `docs/glossary.md` §1 に**状態ラベル定義（正典）**を新設し、未計算＝未計算、未検証＝前提未確認、と別軸で定義。
- `system_prompt.md` §2、`output_rules.md` §1A、`templates/full_reading.md`・`annual_reading.md`・`compatibility_reading.md` §0 の「（すべて未検証）」を「（すべて未計算）＋出生時刻＝未検証」に修正。

### M3 重点テーマ数 4 vs 6 → ✅ 修正済み
- `user/priorities.md` §2 を**コア4テーマの唯一の正典**に（①仕事②創作・技術探究③夫婦④後半戦／付随：独立・組織は①に内包）。
- `user/subject.md` §2 を「コア4＋付随2」の階層に。`templates/full_reading.md` §1 と `quality_rules.md` §1 を priorities.md 参照に統一。

### M4 ROADMAP の v0.2 が実態と乖離 → ✅ 修正済み
- 実装済み項目を [x] 化。v0.2 を「整合性の確定（本レビュー対応）」として再定義し、C1〜M8 の対応を列挙。Minor は次回パッチとして [ ] 残置。

### M5 README「使い方」が CLAUDE.md 未読込 → ✅ 修正済み
- `README.md` 使い方を「1. CLAUDE.md を最初に → 2. system/（safety→system_prompt→output/quality）→ 3. user/ → 4. templates/」に。§0 参照を追記。

### M6 位置（サイン/度数/柱）が占術解釈タグ → ✅ 修正済み
- `output_rules.md` に **§3-1 位置＝`[計算済]`／意味＝`[占術解釈]` の切り分け規約**を新設。概算は `[計算済(概算)]`。
- `user/subject.md` §3-2・`user/wife.md` §2-2・`astrology/western_astrology.md` §2 の太陽サインを `[計算済(概算)]`＋意味は占術解釈に分離。

### M7 辛口トーンと未検証推測の橋渡しなし → ✅ 修正済み
- `system_prompt.md` §4 に橋渡しルール追加：「辛口の強度は証拠の強度に比例」「未検証・占術解釈段階を辛口に断ずるのは禁止」「**占術的には強く出るが現実判断では未確定**をセットにする」。
- `quality_rules.md` §1 チェックリストと §3 レッドフラグに対応項目を追加。

### M8 実計算の設定規約・出典書式が未定義 → ✅ 修正済み
- **`knowledge/calculation_config.md` を新設**（正典）：JST 固定・DST 無・出生地座標・トロピカル・Placidus・節入り基準・真太陽時補正・六星の体系明記・`[計算済]` 出典書式・v0.3 前チェックリスト。
- `output_rules.md` §3-2 に `[計算済]` 出典書式テンプレートを追加。`astrology/bazi.md`・`western_astrology.md` §5 から calculation_config を参照。
- `CLAUDE.md` §3-3 と `README.md`（ディレクトリ構成・バージョン欄）から `knowledge/calculation_config.md` への実質参照を追加（最終チェックで両方に参照があることを確認）。

---

## コミット前 最終チェック（5点・ユーザー指示）

1. **FIX_LOG内のC1〜M8表の重複** → 無し（各見出し1回ずつ、`grep` で確認済み）。
2. **CLAUDE.md が唯一の最上位正典か** → §0（優先順位）・冒頭で「唯一の最上位運用ルール」と明記。他ファイルはそこを参照するのみ。
3. **system_prompt.md / safety_rules.md に「最上位」表現が残っていないか** → 残存する「最上位」「正典」の語は**すべて CLAUDE.md を指す参照**（自称ではない）。両ファイルとも冒頭に「本ファイルは最上位を名乗らない」と明記済み。
4. **未計算／未検証／推測／計算済 の定義が glossary.md に集約され矛盾がないか** →
   - `glossary.md` §1 が正典。全文検索で「未検証」の残存箇所を確認し、`system_prompt.md` §2 の旧例文（末尾の宙ぶらりんな「未検証」）に**修正漏れ**を発見 → 出生時刻に明示的に紐付ける形に修正。
   - `CLAUDE.md` §3-3 も「未計算 / 未検証」を同列羅列していた**同種の残存混同**を発見・修正（未計算を主に、未検証は前提未確認の場合の別軸と明記）。
   - `psychology/*` の「未検証」（Big5・愛着スタイル等の仮説）は astrology データとは別の対象（現実照合前の仮説）であり、glossary定義と矛盾しない用法として許容。
5. **`knowledge/calculation_config.md` が README または CLAUDE.md から参照されているか** → 当初 README のみ（バージョン履歴の一文のみで実質的でない）、CLAUDE.md に参照なし。**両方に実質参照を追加**（CLAUDE.md §3-3 のハード禁止規定内、README のディレクトリ構成内）。

上記2件の追加修正（system_prompt.md 例文、CLAUDE.md §3-3）を行った上で、5点とも問題なし。

---

## 今回未対応（Minor・次回パッチ）

指示範囲（Critical＋Major）外のため未着手。`REVIEW_v0.2.md` の Minor 節参照。

- m1 `user/subject.md` §4：`[事実]` タグの箇条書きに推論（「…と読める」）が混在 → `[推測]` へ分離。
- m2 テンプレの計算状態ブロック書式の完全統一（full_reading は §0 に「■ 計算状態」見出しを追加済み。annual/compatibility の見出し様式は要微調整）。
- m3 「外れログ」の実体（`templates/miss_log.md` 等）が未作成。
- m5 「時刻依存→信頼度を下げる」の重複記載（DRY）を canonical 1 箇所へ集約。
- m6 `CHANGELOG.md` の「プレースホルダ（骨子あり）」表記を実態（実装済み）に更新。
- m7 「六星占術系」表記の一貫化（商標配慮）。

> Minor もまとめて対応する場合は指示ください。

---

## 変更ファイル一覧（今回）

**新規（2）**
- `knowledge/calculation_config.md`
- `REVIEW_v0.2_FIX_LOG.md`（本ファイル）

**修正（14）**
- `CLAUDE.md`
- `system/system_prompt.md` / `system/safety_rules.md` / `system/output_rules.md` / `system/quality_rules.md`
- `docs/glossary.md`
- `knowledge/README.md`
- `astrology/bazi.md` / `astrology/western_astrology.md`
- `user/subject.md` / `user/wife.md` / `user/priorities.md`
- `templates/full_reading.md` / `templates/annual_reading.md` / `templates/compatibility_reading.md`
- `README.md` / `ROADMAP.md`

（`REVIEW_v0.2.md` はレビュー原本として残置・変更なし）
