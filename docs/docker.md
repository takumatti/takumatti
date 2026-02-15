# 🐳 Docker 操作まとめ

WSL + Docker 環境で、現場で実際によく使うコマンドを整理。

---

## 🖥 WSL 起動

### 起動
```bash
wsl
```

### ディストリビューション確認
```bash
wsl -l -v
```

### ポイント
- Docker Desktop と連携している前提
- 作業は基本 WSL 上で実施

---

## 📂 ディレクトリ移動（cd）

### 目的
docker-compose.yml があるディレクトリへ移動

```bash
cd /mnt/c/Users/xxx/project
```

### ポイント
- compose 実行前は必ずディレクトリ確認
- pwd で現在位置を確認できる

---

## 🚀 docker compose up

### 基本
```bash
docker compose up
```

### バックグラウンド起動
```bash
docker compose up -d
```

### ビルド込みで起動
```bash
docker compose up -d --build
```

### ポイント
- -d ：バックグラウンド実行
- --build ：イメージを再ビルド
- 初回やソース変更時は --build 推奨

---

## 📦 コンテナ一覧確認（docker ps）

### 起動中のみ表示
```bash
docker ps
```

### 停止中も含めて表示
```bash
docker ps -a
```

### ポイント
- コンテナ名確認に必須
- STATUS を見る習慣をつける

---

## 🗑 コンテナ削除（docker rm）

### 単体削除
```bash
docker rm コンテナ名
```

### 強制削除（停止せず削除）
```bash
docker rm -f コンテナ名
```

### 複数削除
```bash
docker rm -f $(docker ps -aq)
```

### ポイント
- -f は強制削除
- 開発環境リセット時によく使う

---

## 📜 ログ確認（docker logs）

### 基本
```bash
docker logs コンテナ名
```

### リアルタイム表示
```bash
docker logs -f コンテナ名
```

### 直近100行のみ
```bash
docker logs --tail 100 コンテナ名
```

### ポイント
- エラー調査はまず logs
- -f は Spring Boot 起動確認でよく使う

---

# 🧠 自分ルール

- 起動後は必ず docker ps で確認
- エラー時はまず docker logs
- うまくいかないときは rm -f → up --build

---

# 💡 学び

- 「コンテナ名を正しく把握すること」が重要
- ログを読めるとトラブル対応が早い
