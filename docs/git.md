# Git 操作メモ

Git の基本操作や、作業中によく使うコマンドをまとめたメモです。  
検証・備忘録目的のため、体系的な解説ではありません。

---

## リポジトリ操作

### リポジトリをローカルにクローン
```bash
git clone <ssh または https>
```

---

### Git の初期化
```bash
cd <対象フォルダパス>
git init
```

---

### 安全ディレクトリに登録（初回のみ）
```bash
git config --global --add safe.directory <プロジェクトパス>
```

- バックスラッシュ \ でも可ですが、Git はスラッシュ / が安定
- クォートは  " " で囲む

---

### 登録済安全ディレクトリ確認方法
```bash
 git config --global --get-all safe.directory
```

---

### 登録済安全ディレクトリから削除
```bash
 git config --global --unset safe.directory <プロジェクトパス>
```
- バックスラッシュ \等はエスケープされている可能性があることに注意
```bash
git config --global --unset-all safe.directory
```

---

## フォルダ・ファイル操作

### フォルダ作成
```bash
mkdir <フォルダ名>
```

---

### README など空ファイルの作成  
※ タイムスタンプ（最終更新日時）も更新される

#### Linux / Git Bash
```bash
touch README.md
```

#### Windows（PowerShell）
```powershell
New-Item README.md
```

---

## Git 管理関連

### .gitignore について
- Git に管理したくないファイルやフォルダを指定する
- プロジェクトのルート直下に `.gitignore` を作成して記載する

---

### 変更状態の確認
```bash
git status
```

---

### ファイルを Git 管理対象から外す  
※ ファイル自体は残るが、履歴には残るので注意
```bash
git rm -r --cached <ファイルパス>
```

---

## コミット操作

### ステージング & コミット
```bash
git add .
git commit -m "コメント内容"
```

---

### 複数行コミットメッセージ
```bash
git commit -m "1行目の要約" -m "" -m "詳細説明"
```

例：
```
【add】コミット種別と要約

変更した理由をできるだけ具体的に記載
```

---

## GitHub 連携

### リモートリポジトリ追加（初回のみ）
```bash
git remote add origin <gitのhttps>
```

---

### ブランチ設定 & プッシュ
```bash
git branch -M main
git push -u origin main
```

---

## コミット種別メモ

- add：新規（ファイル・機能）追加  
- fix：バグ修正  
- update：仕様変更・機能修正（バグではない）  
- remove：削除（ファイル・機能）

---

※ 本メモは個人用の備忘録です
