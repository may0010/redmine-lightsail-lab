# Redmine インストールガイド

このドキュメントでは、AWS Lightsail 上に Docker を利用して Redmine を構築する手順を説明します。

## 前提条件

* AWS アカウントを保有していること
* AWS Lightsail を利用できること
* ブラウザから SSH 接続できること

---

## 構成

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

## 1. AWS Lightsail インスタンス作成

Lightsail で新しいインスタンスを作成します。

https://lightsail.aws.amazon.com/

### 今回の設定内容

| 項目        | 設定値              |
| --------- | ---------------- |
| プラットフォーム  | Linux / Unix     |
| 設計図の選択(OSのみ)   | Ubuntu 24.04 LTS | 
| 起動スクリプト | 任意（今回は設定せず） |
| SSHキー | 初回なのでカスタムキーを作成 |
| 自動スナップショット | チェックせず |
| インスタンスプラン | 汎用     |
| ネットワークタイプ | デュアルスタック |
| (OSの)サイズ | $7の1GBメモリを選択 |
| インスタンス名   | 任意（例：redmine-lab）   |
| タグ | 任意 |

作成完了まで数分待った後、SSH 接続を行います。

「接続タブ＞SSHを使用して接続」ボタンを押すと、別ウィンドウにターミナルが開きます。

```text
以下、このような窓で囲まれた部分のコードをコピーして、ターミナルに貼りつけて実行します。
```

---

## 2. Dockerをインストール

システムを更新します（実行後、少し待ちます）。

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

バージョンが出ればOK。

---

## 3. Redmineフォルダ作成

```bash
mkdir ~/redmine
cd ~/redmine
```

後からプラグインを入れるために、フォルダのオーナーを `ubuntu` に変えておきます。

```bash
sudo chown -R ubuntu:ubuntu ~/redmine
```


---

## 4. Nanoエディタで docker-compose.yml を編集

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

* ファイルを保存して、Nanoエディタを閉じる
  * 保存： `Ctrl + O` ※ゼロではなくオー
  * 確定： `Enter`
  * 修了： `Ctrl + X`

---

## 5. DockerコンテナとRedmineを起動

コンテナを起動します。

```bash
docker compose up -d
```

初回は数分かかるので、待ってから状態確認します。

```bash
docker ps
```

正常な例：

```text
CONTAINER ID   IMAGE            STATUS
xxxxxxxxxxxx   redmine:latest   Up xx seconds
```

---

## 6. ブラウザでRedmineを開く

AWS Lightsail の画面から 「静的 IP アドレス」 をコピーし、ブラウザでアクセスします。

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

## 7. インストール成功を確認

以下を確認します。

* Redmine ログイン画面が表示される
* admin ユーザーでログインできる
* プロジェクト作成画面が表示される


---

## 次のステップ→

Redmine が正常に起動したら、View Customize プラグインを導入します。

以下のファイルを開いてください。

```text
docs/plugin.md
```
