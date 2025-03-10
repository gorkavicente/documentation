---
app_id: impala
app_uuid: faf33df8-b1e0-4529-a281-7e176b2365ec
assets:
  dashboards:
    Impala - Catalog - Overview: assets/dashboards/impala_catalog_overview.json
    Impala - Daemon - Overview: assets/dashboards/impala_daemon_overview.json
    Impala - Overview: assets/dashboards/impala_overview.json
    Impala - Statestore - Overview: assets/dashboards/impala_statestore_overview.json
  integration:
    configuration:
      spec: assets/configuration/spec.yaml
    events:
      creates_events: false
    metrics:
      check: impala.daemon.jvm.gc.count
      metadata_path: metadata.csv
      prefix: impala.
    service_checks:
      metadata_path: assets/service_checks.json
    source_type_name: Impala
  logs:
    source: impala
author:
  homepage: https://www.datadoghq.com
  name: Datadog
  sales_email: info@datadoghq.com (日本語対応)
  support_email: help@datadoghq.com
categories:
- ログの収集
dependencies:
- https://github.com/DataDog/integrations-core/blob/master/impala/README.md
display_on_public_website: true
draft: false
git_integration_title: impala
integration_id: impala
integration_title: Impala
integration_version: 1.0.1
is_public: true
kind: integration
manifest_version: 2.0.0
name: impala
oauth: {}
public_title: Impala
short_description: Apache Impala の健全性とパフォーマンスを監視します。
supported_os:
- linux
- windows
- macos
tile:
  changelog: CHANGELOG.md
  classifier_tags:
  - Supported OS::Linux
  - Supported OS::Windows
  - Supported OS::macOS
  - Category::Log Collection
  configuration: README.md#Setup
  description: Apache Impala の健全性とパフォーマンスを監視します。
  media: []
  overview: README.md#Overview
  support: README.md#Support
  title: Impala
---



## 概要

このチェックは、Datadog Agent を通じて [Impala][1] を監視します。

## セットアップ

ホストで実行されている Agent 用にこのチェックをインストールおよび構成する場合は、以下の手順に従ってください。コンテナ環境の場合は、[オートディスカバリーのインテグレーションテンプレート][2]のガイドを参照してこの手順を行ってください。

### APM に Datadog Agent を構成する

Impala チェックは [Datadog Agent][3] パッケージに含まれています。
サーバーに追加でインストールする必要はありません。

### コンフィギュレーション

1. Impala のパフォーマンスデータの収集を開始するには、Agent のコンフィギュレーションディレクトリのルートにある `conf.d/` フォルダーの `impala.d/conf.yaml` ファイルを編集します。使用可能なすべてのコンフィギュレーションオプションについては、[サンプル impala.d/conf.yaml][4] を参照してください。

ここでは、デーモンを監視する例を示します。

```yaml
init_config:

instances:
  ## @param service_type - 文字列 - 必須
  ## 監視したい Impala サービス。使用可能な値は、`daemon`、`statestore`、`catalog` です。
  #
- service_type: daemon

  ## @param openmetrics_endpoint - 文字列 - 必須
  ## OpenMetrics 形式のメトリクスを公開する URL。
  ##
  ## サービスのデフォルトポートは以下の通りです。
  ## - Daemon: 25000
  ## - Statestore: 25010
  ## - Catalog: 25020
  #
  openmetrics_endpoint: http://%%host%%:25000/metrics_prometheus
```

また、同じ Agent で複数のサービスを同時に監視することも可能です。

```yaml
init_config:

instances:

- service_type: daemon
  service: daemon-1
  openmetrics_endpoint: http://<DAEMON-1-IP>:25000/metrics_prometheus
- service_type: daemon
  service: daemon-2
  openmetrics_endpoint: http://<DAEMON-2-IP>:25000/metrics_prometheus
- service_type: statestore
  openmetrics_endpoint: http://<STATESTORE-IP>:25010/metrics_prometheus
- service_type: catalog
  openmetrics_endpoint: http://<CATALOG-IP>:25020/metrics_prometheus
```

2. [Agent を再起動します][5]。

### 検証

[Agent の status サブコマンドを実行][6]し、Checks セクションで `impala` を探します。

## 収集データ

### メトリクス
{{< get-metrics-from-git "impala" >}}


### イベント

Impala インテグレーションには、イベントは含まれません。

### サービスのチェック
{{< get-service-checks-from-git "impala" >}}


### ログ管理

Impala インテグレーションは、Impala のサービスからログを収集し、Datadog に転送することができます。すべてのログを収集する方法については、[コンフィギュレーションファイルの例][9]を参照してください。

## トラブルシューティング

ご不明な点は、[Datadog のサポートチーム][10]までお問合せください。


[1]: https://impala.apache.org
[2]: https://docs.datadoghq.com/ja/agent/kubernetes/integrations/
[3]: https://app.datadoghq.com/account/settings#agent
[4]: https://github.com/DataDog/integrations-core/blob/master/impala/datadog_checks/impala/data/conf.yaml.example
[5]: https://docs.datadoghq.com/ja/agent/guide/agent-commands/#start-stop-and-restart-the-agent
[6]: https://docs.datadoghq.com/ja/agent/guide/agent-commands/#agent-status-and-information
[7]: https://github.com/DataDog/integrations-core/blob/master/impala/metadata.csv
[8]: https://github.com/DataDog/integrations-core/blob/master/impala/assets/service_checks.json
[9]: https://github.com/DataDog/integrations-core/blob/master/impala/datadog_checks/impala/data/conf.yaml.example#L632-L755
[10]: https://docs.datadoghq.com/ja/help/