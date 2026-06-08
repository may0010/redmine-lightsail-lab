# Redmine Installation Guide

このドキュメントでは、AWS Lightsail 上に Docker を利用して Redmine を構築する手順を説明します。

## Prerequisites

* AWS アカウント
* AWS Lightsail を利用できること
* ブラウザから SSH 接続できること

---

## Architecture

```text
Browser
  ↓
AWS Lightsail
  ↓
Ubuntu
  ↓
Docker
  ↓
Redmine
```

---

## 1. Create a Lightsail Instance

Lightsail で新しいインスタンスを作成します。

### Settings

| Item          | Value                   |
| ------------- | ----------------------- |
| Platform      | Linux/Unix              |
| Blueprint     | Ubuntu 24.04 LTS        |
| Plan          | Smallest available plan |
| Instance Name | redmine-lab             |

作成完了後、SSH 接続を行います。

---

## 2. Install Docker

システムを更新します。

```bash
sudo apt update
sudo apt upgrade -y
```

Docker をインストールします。

```bash
curl -fsSL https://get.docker.com | sh
```

Docker グループへ現在ユーザーを追加します。

```bash
sudo usermod -aG docker ubuntu
newgrp docker
```

インストール確認。

```bash
docker --version
```

---

## 3. Create Working Directory

```bash
mkdir ~/redmine
cd ~/redmine
```

---

## 4. Create Docker Compose File

```bash
nano docker-compose.yml
```

以下の内容を保存します。

```yaml
services:
  redmine:
    image: redmine:latest
    container_name: redmine
    restart: unless-stopped

    ports:
      - "80:3000"

    environment:
      REDMINE_SECRET_KEY_BASE: your-secret-key

    volumes:
      - ./plugins:/usr/src/redmine/plugins
      - ./themes:/usr/src/redmine/public/themes
      - ./files:/usr/src/redmine/files
```

> Note:
> 実運用では `your-secret-key` を十分長いランダム文字列に変更してください。

---

## 5. Start Redmine

コンテナを起動します。

```bash
docker compose up -d
```

状態確認。

```bash
docker ps
```

正常な例：

```text
CONTAINER ID   IMAGE            STATUS
xxxxxxxxxxxx   redmine:latest   Up xx seconds
```

---

## 6. Access Redmine

Lightsail の Public IP を確認し、ブラウザでアクセスします。

```text
http://<Public-IP>
```

初期ログイン情報：

```text
Username: admin
Password: admin
```

初回ログイン時にパスワード変更を求められます。

---

## 7. Verify Installation

以下を確認します。

* Redmine ログイン画面が表示される
* admin ユーザーでログインできる
* プロジェクト作成画面が表示される

---

## Useful Commands

### Check Running Containers

```bash
docker ps
```

### View Logs

```bash
docker logs redmine
```

### View Last 50 Log Lines

```bash
docker logs redmine --tail 50
```

### Restart Redmine

```bash
docker compose restart
```

### Stop Redmine

```bash
docker compose down
```

---

## Next Step

Redmine が正常に起動したら、View Customize プラグインを導入します。

See:

```text
docs/plugin.md
```
