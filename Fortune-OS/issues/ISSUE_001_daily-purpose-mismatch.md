# ISSUE_001: Fortune Dailyの目的が利用者期待と一致していない

- 分類：分析品質
- 優先度：High
- 発見元：Fortune Daily RC1 運用テスト（2026-07-01実施、1回目・実ユーザーフィードバック）
- 状態：Open

## 問題

Fortune Dailyの目的が「分析レポート」になってしまい、利用者が期待する
「今日どう動けばいいか」との目的が一致していない。

## 再現ケース

Fortune Daily RC1 運用テスト（2026-07-01）で出力した内容を実ユーザーに渡した際のフィードバック。

## 期待結果

Fortune Dailyが、利用者の「今日どう動けばいいか」という期待と一致した目的で機能すること。

## 現状

Data Sufficiency・Evidence・Reasoningの各層は機能したが、Outputの目的設定そのものが
利用者の実際の期待（占い的な体験・今日どう動くべきかの実感）とズレている。

## Evidence

RC1初回運用で実ユーザーから「渡したけど占ってないね」というフィードバックが得られた。
これはOutputの文章品質だけの問題ではなく、Fortune Dailyの**目的そのもの**が利用者期待と
一致していないことを示す。

## 影響範囲

現時点ではFortune Daily（RC2候補・`ROADMAP.md`記載）のみに関わる。ただし、目的と利用者期待の
不一致という論点は、将来のFortune Weekly／Fortune Life等、同系統のRC2候補にも共通しうる。

## 改善案

Engineは変更しない。Fortune Dailyの目的を「今日の意思決定支援」として複数回運用し、
同様のIssueが3件以上出るか確認する。

## 保留理由

RC1運用テストはまだ1回のみ。Issueが3件以上蓄積するまではEngine変更を提案しない
（`issues/README.md` §5 Engine昇格の判定手順に従う）。
