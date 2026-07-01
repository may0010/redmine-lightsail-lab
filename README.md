# Redmine on AWS Lightsail

AWS Lightsail 上に Docker を利用して Redmine を構築し、View Customize プラグインを導入するための手順をまとめたリポジトリです。

（Redmine setup guide on AWS Lightsail using Docker and View Customize plugin.）

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

## ディレクトリ構成

```text
.
├── README.md                                # リポジトリ概要
└── docs
    ├── install.md                           # Redmine構築手順
    ├── plugin.md                            # View Customize導入手順
    ├── closing.md                           # 環境削除手順
    └── decisions
        └── ADR-001-why-lightsail.md         # 技術選定の記録
```

本リポジトリの手順は、以下の順に参照してください。

```text
docs/install.md
    ↓
docs/plugin.md
    ↓
docs/closing.md
```

なお、docs/decisions/ADR-001-why-lightsail.mdは技術選定時の検討内容を記録したメモです。

環境構築には必須ではないため、必要に応じて参照してください。


## Disclaimer

本手順は個人検証環境での利用を想定しています。

本番環境で利用する場合は、適切なセキュリティ設定、バックアップ、監視設定を実施してください。

## License

MIT License

