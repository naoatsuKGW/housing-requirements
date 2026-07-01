# issues/

Fortune-OS **v1.0 RC1（Release Candidate）以降の開発方針**を実行するためのIssue管理場所。

> 本ファイルは仕様・運用文書であり、7レイヤータグの適用対象外（`CLAUDE.md` §2 の精神に準拠）。

---

Fortune OS は未来を当てるシステムではない。

また、人を分類・決めつけるシステムでもない。

Fortune OS は

Evidence
↓
Reasoning
↓
Decision

の順番で、象徴体系（占術）と現実世界を接続し、意思決定を支援する研究プロジェクトである。

Research はまだ答えが分からない問いを扱う。

Issue は実際のGold Readingから確認された問題を扱う。

Research は Issue を生むことがある。

Issue は Engine を改善することがある。

---

## 0. この方針の前提（v1.0 RC1）

Fortune-OS の目的は **Evidence-driven Symbolic Decision Support**（占術システムではなく、
証拠に基づいて象徴体系を意思決定へ接続する分析システム）である。

設計フェーズは終了した。以後は新しいEngine・分析フレーム・設計書を安易に追加しない。

```
1. 実際のGold Reading
     ↓
2. 問題点の抽出
     ↓
3. Issue化
     ↓
4. 議論
     ↓
5. 必要ならEngineへ昇格
```

**Gold Readingを書いていて改善点を見つけた場合、Engineを変更せず、必ずIssueを作成する。**
同じ問題が**3件以上**のIssueとして繰り返されたときだけ、Engineへの昇格を検討・提案する。

**禁止：** 新しいEngine追加・新しい分析フレーム追加・新しい設計書追加。Issue化のみ行う。

---

## 1. Issueの書き方

### 1-1. どこに書くか

```
Fortune-OS/issues/ISSUE_<3桁連番>_<短い識別子>.md
例：issues/ISSUE_001_alternative-hypothesis-tone.md
```

### 1-2. 起票のルール

- Issueは**実際のGold Reading執筆・レビューの中で見つかった問題**についてのみ起票する。
  仮説的・思弁的な「こうした方がいいのでは」という設計論だけでは起票しない
  （それは設計書の再発明になり、v1.0 RC1の方針に反する）。
- 1 Issue = 1 問題。複数の問題を1つのIssueにまとめない（後の「3件以上で昇格」判定が曖昧になるため）。
- 起票の際は、必ずどのGold Reading（`GOLD_00N_*.md`）のどのセクションで気付いたかを明記する。
- Issueは**その場でEngineや設計書を直すための言い訳にしない**。まず記録し、議論を経てから判断する。

### 1-3. Issueのライフサイクル

```
起票（状態：Open）
  ↓
議論・追加Evidence収集（状態：Discussing）
  ↓
同種のIssueが3件以上集まる（状態：Promotion Candidate）
  ↓
Engine昇格を提案する、または「保留」として記録し続ける（状態：Promoted / Held）
```

---

## 2. 分類

Issueは起票時に、以下のいずれか1つ（複数該当する場合は最も本質的なもの1つ）に分類する。

| カテゴリ | 内容 | 例 |
|---|---|---|
| **分析品質** | 一般論化・断定に近い表現・タグ付けの甘さなど、出力そのものの質の問題 | 「性格分析が職業ステレオタイプの域を出ていない」 |
| **構造・型** | `GOLD_STANDARD_TEMPLATE.md`等の型が実情に合わない、セクションの過不足 | 「適用条件未達のセクションが読者に停止感を与える」 |
| **Evidence関連** | Evidence Priority／Acquisition Priority等、証拠の評価・優先順位づけの問題 | 「取得容易性の★評価に客観的根拠がない」 |
| **安全性・倫理** | 断定禁止・人格否定禁止・第三者配慮等、ハードルールに関わる兆候 | 「配偶者の性格に踏み込みすぎている」 |
| **用語・命名** | 名称の不整合・分かりにくさ（例：Information Value→Decision Impact→Evidence Priorityの変遷） | 「同じ概念が複数の名前で呼ばれている」 |
| **参照・整合性** | 他のGold Reading・設計書との参照が不正確、または矛盾している | 「GOLD_003がGOLD_001の検証質問番号を誤って引用している」 |
| **Engine昇格候補** | 同種の問題が既に2件以上蓄積しており、3件目が見つかった場合に付与 | （3-3参照） |

---

## 3. 優先順位

| 優先度 | 基準 |
|---|---|
| **Critical** | 断定禁止・未計算の隠蔽・人格否定など、`CLAUDE.md`のハード禁止に抵触する疑いがある |
| **High** | 複数のGold Readingで同種の問題が繰り返されている、またはEngine昇格候補になっている |
| **Medium** | 単発だが分析品質・読者の理解に明確な影響がある |
| **Low** | 表記ゆれ・命名の不統一など、軽微で緊急性のないもの |

**Critical は即座に議論する。** それ以外は蓄積を待ってから判断してよい。

---

## 4. Issueテンプレート

新規Issueを起票する際、以下をコピーして`issues/ISSUE_NNN_<識別子>.md`として保存する。

```markdown
# ISSUE_NNN: <短いタイトル>

- 分類：<分析品質 / 構造・型 / Evidence関連 / 安全性・倫理 / 用語・命名 / 参照・整合性>
- 優先度：<Critical / High / Medium / Low>
- 発見元：<GOLD_00N_*.md の該当セクション（例：GOLD_003_COMPATIBILITY.md §13）>
- 状態：<Open / Discussing / Promotion Candidate / Promoted / Held>

## 問題
（何が問題か。1〜3文で簡潔に）

## 再現ケース
（実際にどのGold Readingのどの記述で発生したか。引用可）

## 期待結果
（本来どうあるべきだったか）

## 現状
（実際には何が起きているか。期待結果との差分）

## Evidence
（この問題が実在することを示す具体的な根拠。推測ではなく引用・事実で示す）

## 影響範囲
（このIssueが放置された場合、他のどのGold Reading・セクション・Engine概念に影響するか）

## 改善案
（あれば。無ければ「未定」と明記してよい。ここでEngineの実装まで踏み込まない）

## 保留理由
（今すぐ対応しない場合、その理由。「3件集まるまで様子見」等）
```

---

## 5. Engine昇格の判定手順

1. 同じ**分類**・同じ**本質的な問題**を指すIssueが3件以上`Open`または`Discussing`になった時点で、
   該当Issue群に「Engine昇格候補」の分類を追記する。
2. 3件のIssueを並べ、共通する根本原因を1文で言語化する。
3. その根本原因を解決するために、既存のEngine（`ENGINE_DESIGN_v1.0.md`）の修正で足りるか、
   新規Engineが必要かを議論する。**新規Engineが必要という結論を急がない。** 既存の型の微修正で
   済む場合が多いことを前提に検討する。
4. 昇格が承認された場合のみ、該当する設計書（`ENGINE_DESIGN_v1.0.md`等）を更新する。
   この手順を経ない設計書の追加・変更は行わない。

---

## 6. この方針とEngine構成の関係

Fortune-OSの分析基盤は、今後は次の3つに集約する方針とする（新規Engineはこれらへの統合を原則とする）：

- **Evidence Engine** — 証拠の収集・評価（Evidence Acquisition Priority・Acquisition Cost等）
- **Reasoning Engine** — 仮説構築・統合（Root Cause分析・Alternative Hypothesis・Disconfirming Evidence等）
- **Output Engine** — 出力整形（`templates/`・`GOLD_STANDARD_TEMPLATE.md`等）

Issueが「Engine昇格候補」になった場合も、まずこの3区分のどれかへの機能追加として検討し、
第4のEngineを新設することは既定路線としない。
