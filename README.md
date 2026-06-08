# redmine-lightsail-lab

Redmine setup guide on AWS Lightsail using Docker and View Customize plugin.

# Redmine on AWS Lightsail

AWS Lightsail 上に Docker を利用して Redmine を構築し、View Customize プラグインを導入するための手順をまとめたリポジトリです。

個人学習や検証環境の構築を目的としています。

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

## 検証環境

| Component             | Version             |
| --------------------- | ------------------- |
| AWS Lightsail         | Ubuntu Instance     |
| Ubuntu                | 24.04 LTS           |
| Docker                | Latest              |
| Redmine               | Latest Docker Image |
| View Customize Plugin | Latest              |

## このリポジトリで扱う内容

* AWS Lightsail の作成
* Docker のインストール
* Redmine の起動
* View Customize プラグインの導入
* よくあるエラーと対処方法

## ディレクトリ構成

```text
.
├── README.md
└── install.md
```

## Disclaimer

本手順は個人検証環境での利用を想定しています。

本番環境で利用する場合は、適切なセキュリティ設定、バックアップ、監視設定を実施してください。

## License

MIT License

