# ADR-001: Why AWS Lightsail Was Chosen for the Redmine Test Environment

* なぜその構成にしたかを記したADR（Architecture Decision Record）です
* 読まなくても構築できます（後日の自分用メモです）

## Summary

以下、ChatGPTとの会話です。

```text
個人的な実験用で、Redmineを立てて、プラグインを入れたり、JavaScript（View Customize）を試したいです。
→それなら、結論としては Amazon Lightsail 一択に近いです。
　AWS公式でも「簡単なWebアプリ・Dev/Test環境向け」とされています。
```

## Context

Redmine のプラグインや View Customize を利用した JavaScript の動作検証を行うため、AWS 上に個人用の実験環境を構築する必要があった。

主な要件は以下の通り。

* 低コストで運用できること（月額で数100円程度）
* 短時間で環境構築できること
* Redmine プラグインの追加や削除が容易であること
* 環境を壊しても簡単に作り直せること
* 本番環境レベルの高可用性やスケーラビリティは不要

## Options Considered

### Option 1: AWS EC2

EC2 上に Ubuntu を構築し、Docker と Redmine を導入する方法。

#### Advantages

* AWS の標準的な構成
* 自由度が高い
* VPC、IAM、ALB、RDS などと連携しやすい
* 本番環境へ発展させやすい

#### Disadvantages

* 初期設定が比較的複雑
* 学習コストが高い
* 実験用途としてはオーバースペック
* コスト管理が Lightsail より分かりにくい

---

### Option 2: AWS Lightsail

Lightsail 上に Ubuntu を構築し、Docker と Redmine を導入する方法。

#### Advantages

* 数分でサーバーを作成できる
* 固定料金に近くコストを把握しやすい
* SSH 接続が容易
* スナップショットによる復元が簡単
* 実験環境の再作成が容易
* Docker を利用した検証に十分な機能を持つ

#### Disadvantages

* EC2 と比較すると自由度が低い
* 高度な AWS 構成の学習には向かない
* 本格的な本番運用には追加検討が必要

## Decision

AWS Lightsail を採用する。

また、Redmine は Docker コンテナとして構築する。

## Rationale

今回の目的は Redmine の機能検証およびプラグイン開発・評価であり、AWS インフラ自体の学習ではない。

そのため、

* 構築の容易さ
* 運用コストの低さ
* 環境再作成の容易さ

を優先した。

Lightsail はこれらの要件を満たしており、個人検証環境として十分な機能を提供している。

さらに Docker を採用することで、

* Redmine のバージョン変更
* プラグイン導入
* 環境再構築

を容易に実施できる。

## Consequences

### Positive

* 数十分以内に検証環境を構築できる
* Redmine のプラグイン検証を容易に行える
* GitHub Actions との連携検証が可能
* スナップショットによるバックアップが容易

### Negative

* EC2 や AWS ネットワーク設計の学習には向かない
* 本番環境へ移行する場合は構成の見直しが必要になる可能性がある


## References

* https://docs.aws.amazon.com/decision-guides/latest/lightsail-elastic-beanstalk-ec2/lightsail-elastic-beanstalk-ec2.html
* https://docs.aws.amazon.com/lightsail/latest/userguide/amazon-lightsail-quick-start-guide-redmine.html
* https://hub.docker.com/_/redmine
