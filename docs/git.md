# Git 操作メモ

Git の基本操作や、作業中によく使うコマンドをまとめたメモです。  
検証・備忘録目的のため、体系的な解説ではありません。

---

# リポジトリ操作

## リポジトリをローカルにクローン
```bash
git clone <ssh または https>
```

---

## Git の初期化
```bash
cd <対象フォルダパス>
git init
```

---

## 安全ディレクトリに登録（初回のみ）
```bash
git config --global --add safe.directory <プロジェクトパス>
```

- バックスラッシュ `\` でも可ですが、Git はスラッシュ `/` が安定
- クォートは `"` で囲む

---

## 登録済安全ディレクトリ確認方法
```bash
git config --global --get-all safe.directory
```

---

## 登録済安全ディレクトリから削除
```bash
git config --global --unset safe.directory <プロジェクトパス>
```

- バックスラッシュ等はエスケープされている可能性があることに注意

```bash
git config --global --unset-all safe.directory
```

---

# フォルダ・ファイル操作

## フォルダ作成
```bash
mkdir <フォルダ名>
```

---

## README など空ファイルの作成

※ タイムスタンプ（最終更新日時）も更新される

### Linux / Git Bash
```bash
touch README.md
```

### Windows（PowerShell）
```powershell
New-Item README.md
```

---

# Git 管理関連

## .gitignore について

- Git に管理したくないファイルやフォルダを指定する
- プロジェクトのルート直下に `.gitignore` を作成して記載する

---

## 変更状態の確認
```bash
git status
```

---

## ファイルを Git 管理対象から外す

※ ファイル自体は残るが、履歴には残るので注意

```bash
git rm -r --cached <ファイルパス>
```

---

## 特定ファイルをステージ対象から外す
```bash
git restore --staged <ファイルパス>
```

例：
```bash
git restore --staged src/lib/api.ts
```

- ファイル変更自体は消えない
- commit 対象からだけ除外

---

## ファイル変更自体を取り消す
```bash
git restore <ファイルパス>
```

例：
```bash
git restore src/lib/api.ts
```

- ローカル変更を破棄
- 元に戻せないため注意

---

# コミット操作

## ステージング & コミット
```bash
git add .
git commit -m "コメント内容"
```

---

## 複数行コミットメッセージ
```bash
git commit -m "1行目の要約" -m "" -m "詳細説明"
```

例：

```text
【add】コミット種別と要約

変更した理由をできるだけ具体的に記載
```

---

## 直前コミットを修正（コミット作り直し）
```bash
git commit --amend
```

- 直前のコミットを修正する
- コミットメッセージ変更や、ファイル追加漏れ時に使用
- `git add` 後に実行する

---

## commit を一度変更状態へ戻す
```bash
git reset HEAD^
```

- commit を取り消して変更内容だけ残す
- rebase 中の commit 修正でよく使用

---

# rebase 操作

## コミット履歴を書き換える（interactive rebase）
```bash
git rebase -i HEAD~2
```

- 直近2コミットを編集対象にする
- commit の順番変更・削除・統合・修正が可能

---

## rebase 編集モード

```text
pick <commit> コメント
```

を：

```text
edit <commit> コメント
```

に変更すると、その commit を編集可能

### 主なコマンド

- `pick`：そのまま使用
- `edit`：commit を編集
- `drop`：commit 削除

---

## rebase 続行
```bash
git rebase --continue
```

- commit 修正後に rebase を再開

---

## 特定 commit を履歴から削除（drop）
```bash
git rebase -i HEAD~2
```

例：

```text
pick aaa111 localhost版
drop bbb222 192.168版
```

- 不要 commit を履歴ごと削除

---

# Vim 操作（rebase 時）

## 編集モード開始
```text
i
```

---

## 編集モード終了
```text
Esc
```

---

## 保存して終了
```text
:wq
```

---

## 終了のみ（保存しない）
```text
:q!
```

---

# 履歴・差分確認

## 特定文字列が履歴に残っているか確認
```bash
git log -S "検索文字列" --all
```

例：
```bash
git log -S "192.168" --all
```

- 過去履歴に特定文字列が存在する commit を検索

---

## 現在 commit のファイル内容確認
```bash
git show HEAD:<ファイルパス>
```

例：
```bash
git show HEAD:src/lib/api.ts
```

- 現在の commit に含まれるファイル内容確認

---

## 作業状態確認
```bash
git status
```

rebase 中は：

```text
interactive rebase in progress
```

などが表示される

---

# GitHub 連携

## リモートリポジトリ追加（初回のみ）
```bash
git remote add origin <gitのhttps>
```

---

## ブランチ設定 & プッシュ
```bash
git branch -M main
git push -u origin main
```

---

## 履歴を書き換えた後の push
```bash
git push origin --force
```

- rebase / amend 後は通常 push 不可
- リモート履歴を強制更新する

※ チーム開発時は注意

---

# detached HEAD について

```text
fatal: You are not currently on a branch.
```

- rebase 中など一時 commit 状態
- `git rebase --continue` 完了後に branch へ戻る

---

# Windows / PowerShell 補助

## PowerShell でファイル内容確認
```powershell
Get-Content <ファイルパス>
```

例：
```powershell
Get-Content src/lib/api.ts
```

- Windows で `cat` の代替として使用可能

---

# 機密情報を commit した時の基本対応

## まず修正

```text
APIキー・秘密情報は即ローテーション
```

---

## 履歴確認
```bash
git log -S "検索文字列" --all
```

---

## 不要 commit 削除
```bash
git rebase -i
```

---

## GitHub へ反映
```bash
git push origin --force
```

---

※ `192.168.x.x` は private IP のため、単体では重大事故にはなりにくい  
ただし公開 repository では履歴削除推奨

---

# コミット種別メモ

- add：新規（ファイル・機能）追加
- fix：バグ修正
- update：仕様変更・機能修正（バグではない）
- remove：削除（ファイル・機能）

---

※ 本メモは個人用の備忘録です
