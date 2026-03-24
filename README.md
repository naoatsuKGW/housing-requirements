# あいだに住む - プレゼンテーションスライド

家族の間に生まれる建築のプレゼンテーションスライド全13ページのオフラインバックアップです。

## 📦 パッケージ内容

このアーカイブには以下が含まれています：

- **index.html** - すべてのスライドへのナビゲーションページ
- **slides/** - 全13ページの完全なHTMLスライド
  - slide_01.html 〜 slide_13.html
  - 各ファイルは完全なスタンドアロンHTML（CSS、レイアウト含む）
- **images/** - スライドで使用される画像ファイル（7枚）
  - ⚠️ 現在はプレースホルダー画像
  - 実際の画像は元のGensparkサーバーから取得できませんでした
- **README.md** - このファイル
- **.gitignore** - Git用の除外設定

## 📊 スライド一覧

| ページ | タイトル | 内容 | 画像 |
|--------|----------|------|------|
| P01 | あいだに住む | カバーページ | ReiT4bb6 |
| P02 | 家族の今 | 現在の生活状態 | なし |
| P03 | 家族のこれから | 将来の変化 | ltlHwPw9 |
| P04 | 1F Entrance & 2F WIC | 玄関・WICの要件 | T4VVvLUt |
| P05 | Mother's Private Room | 母の個室 | NtFFrBd1, Bgsen7Qi |
| P06 | 1F Living | 1階リビング | なし |
| P07 | 2F Kitchen & Dining | 2階キッチン | gx7aKfu6 |
| P08 | 2F Living | 2階リビング | UaxBIoHw |
| P09 | Couple's Bedroom | 夫婦の寝室 | なし |
| P10 | Child's Room | 子供部屋 | なし |
| P11 | Outdoor Spaces | 外部空間 | なし |
| P12 | 建築仕様 (1/2) | 構造・断熱 | なし |
| P13 | 建築仕様 (2/2) | 設備・外装 | なし |

## 🖥️ ローカルでの閲覧方法

### 方法1: 直接開く
1. `index.html` をブラウザで開く
2. スライド一覧からページを選択

### 方法2: ローカルサーバーを起動
HTMLファイルを正しく表示するため、ローカルサーバーの使用を推奨します：

```bash
# Python 3がインストールされている場合
cd aida_ni_sumu_complete_offline
python3 -m http.server 8000

# ブラウザで http://localhost:8000 にアクセス
```

```bash
# Node.jsがインストールされている場合
npx serve .

# 表示されるURLにアクセス
```

## ⚠️ 画像について

### 現状
- **images/** フォルダには7枚のプレースホルダー画像が入っています
- これらは元のGenspark CDN画像のダウンロードが403エラーで失敗したため作成されたものです
- HTMLの構造とレイアウトは完全に機能しますが、実際の画像は表示されません

### 画像を取得する方法
実際の画像を取得するには、以下の手順が必要です：

1. 元のGensparkプレゼンテーションシステムにログイン
2. 各画像を個別にダウンロード
3. 画像IDに対応するファイル名で `images/` フォルダに保存：
   - `ReiT4bb6.png`
   - `ltlHwPw9.png`
   - `T4VVvLUt.png`
   - `NtFFrBd1.png`
   - `Bgsen7Qi.png`
   - `gx7aKfu6.png`
   - `UaxBIoHw.png`

画像を配置後、HTMLファイルの変更は不要です（既にローカルパスを参照しています）。

## 🎨 デザインシステム

### カラーパレット
- `--paper: #fdfbf7` - 背景色（クリーム紙）
- `--ink: #2c2c2c` - メインテキスト（濃灰色）
- `--muted: #888888` - サブテキスト（中灰色）
- `--line: #d1d1d1` - 線・区切り（薄灰色）
- `--accent: #e6e2da` - アクセント（ベージュ）

### フォント
- **見出し**: Shippori Mincho (28-34px)
- **本文**: Noto Sans JP (11-13px)
- **メタ情報**: Inter (9-10px)

### レイアウト
- スライドサイズ: 1280×720px (16:9)
- ヒーローセクション: 50-60% height
- 要件グリッド: 3-4カラム、16pxギャップ

## 🚀 GitHubへのアップロード

このフォルダをGitHubリポジトリとして公開する手順：

### 1. ローカルGitリポジトリの初期化

```bash
cd aida_ni_sumu_complete_offline
git init
git add .
git commit -m "Initial commit: あいだに住む presentation slides backup"
```

### 2. GitHubにリポジトリを作成

1. https://github.com にログイン
2. 右上の「+」→「New repository」
3. リポジトリ名: `aida-ni-sumu-slides`
4. Description: `あいだに住む - 家族の間に生まれる建築のプレゼンテーションスライド`
5. Public/Private を選択
6. 「Create repository」をクリック

### 3. リモートリポジトリに接続してプッシュ

```bash
git remote add origin https://github.com/YOUR_USERNAME/aida-ni-sumu-slides.git
git branch -M main
git push -u origin main
```

### 4. GitHub Pagesで公開（オプション）

1. リポジトリの「Settings」→「Pages」
2. Source: `Deploy from a branch`
3. Branch: `main` / `/ (root)`
4. 「Save」をクリック
5. 数分後、`https://YOUR_USERNAME.github.io/aida-ni-sumu-slides/` でアクセス可能に

## 📝 今後の更新

スライド内容を変更する場合：

1. `slides/slide_XX.html` を編集
2. 変更をコミット：
   ```bash
   git add slides/slide_XX.html
   git commit -m "Update: [変更内容の説明]"
   git push
   ```

画像を追加する場合：

1. `images/` フォルダに画像ファイルを配置
2. コミット：
   ```bash
   git add images/
   git commit -m "Add: 実際の画像ファイルを追加"
   git push
   ```

## 🔧 技術仕様

- **フォーマット**: HTML5 + CSS3
- **フォント**: Google Fonts経由（インターネット接続必要）
- **画像**: ローカル相対パス（../images/*.png）
- **ブラウザ**: 最新のChrome, Firefox, Safari, Edgeで動作確認済み
- **レスポンシブ**: 固定サイズ（1280×720px）のプレゼンテーション形式

## 📅 バックアップ情報

- **作成日**: 2026年3月24日
- **バージョン**: 1.0
- **元データ**: Genspark Presentation System
- **スライド数**: 13ページ
- **画像数**: 7枚（プレースホルダー）

## 📧 Contact

このプロジェクトに関する質問や提案がある場合は、GitHubのIssuesセクションをご利用ください。

---

**あいだに住む** - 家族の間に生まれる建築  
© 2026 All rights reserved.
