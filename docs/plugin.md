# View Customize Plugin インストールガイド

このドキュメントでは、Redmine に View Customize プラグインを導入する手順を説明します。

## 目的

View Customize プラグインを利用することで、Redmine の画面に対して以下のカスタマイズを実施できます。

* JavaScript の追加
* CSS の追加
* HTML の追加
* 画面表示のカスタマイズ
* UI 改善

## 前提条件

以下が完了していること。

* Redmine が起動している
* 管理者アカウントでログインできる
* Docker コンテナ版 Redmine を利用している

詳細は以下を参照。

```text
docs/install.md
```

---

## プラグインの配置

AWS Lightsail の画面からターミナルを起動しておきます。

plugins ディレクトリへ移動します。

```bash
cd ~/redmine/plugins
```

GitHub からプラグインを取得します。

```bash
git clone https://github.com/onozaty/redmine-view-customize.git
```

---

## フォルダ名の変更

Redmine はプラグイン ID とフォルダ名が一致している必要があります。

取得直後のフォルダ名：

```text
redmine-view-customize
```

変更後：

```text
view_customize
```

変更コマンド：

```bash
mv redmine-view-customize view_customize
```

確認：

```bash
ls -la
```

以下のように表示されること。

```text
view_customize
```

---

## プラグイン Migration 実行

Redmine コンテナへ接続します。

```bash
docker exec -it redmine bash
```

プラグイン用テーブルを作成します。

```bash
SECRET_KEY_BASE=your-secret-key \
bundle exec rake redmine:plugins:migrate \
RAILS_ENV=production
```

正常終了例：

```text
Migrating view_customize...
```

コンテナから退出します。

```bash
exit
```

---

## Redmine 再起動

```bash
cd ~/redmine

docker compose restart
```

状態確認：

```bash
docker ps
```

正常な例：

```text
STATUS Up xx seconds
```

---

## インストール確認

Redmineを開き、管理画面へアクセスします。

```text
Administration
↓
Plugins
```

一覧に以下が表示されていれば成功です。

```text
View Customize
```

---

## 参考資料

* [Redmine View Customize Plugin GitHub Repository](https://github.com/onozaty/redmine-view-customize)

---

## 次のステップ→

ここまでの手順で、

**「AWS Lightsail 上に Docker を利用して Redmine を構築し、View Customize プラグインを導入する」**

作業は完了です。

また、この先のステップとして、Redmine の View Customize プラグインで利用できる

**ページ上部へ（↑）で戻るボタン**

の自作コードを、別リポジトリで紹介しています。

ご興味がありましたら、そちらもぜひご覧ください。

```text
https://github.com/may0010/redmine-view-customize-scroll-to-top/blob/main/README.md
```
