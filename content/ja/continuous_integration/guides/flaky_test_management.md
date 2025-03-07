---
aliases:
- /ja/continuous_integration/guides/find_flaky_tests/
kind: ガイド
title: 不安定なテストの管理
---

{{< site-region region="gov" >}}
<div class="alert alert-warning">選択したサイト ({{< region-param key="dd_site_name" >}}) では、現時点では CI Visibility は使用できません。</div>
{{< /site-region >}}

_不安定なテスト_とは、同じコミットに対して複数回テストを実行したときに、合格と不合格の両方の状態を示すテストのことです。あるコードをコミットして CI を実行したときにテストが失敗し、再度 CI を実行したときにテストが合格していた場合、そのテストはコードの品質を証明するものとして信頼できません。

不安定なテストは、CI システムや最終製品にリスクと予測不可能性をもたらします。どのテストが不安定なのかを覚えておかなければならない場合、テスト結果に対する信頼が失われ、パイプラインの再試行に膨大な時間とリソースが浪費されることになります。

テストサービスのページで、指定したテストサービスとブランチの _Flaky Tests_ テーブルを確認します。選択した時間枠の中で、不安定なテストがすべて表示されます。

{{< img src="ci/flaky-test-management.png" alt="Test Service ページの Flaky Tests テーブル" style="width:100%;">}}

このアプリは、不安定なテストについて以下の情報を提供することで、不安定なテストの優先順位付けを支援します。

* **Average duration**: テストの実行にかかる平均時間。
* **First and Last flaked**: テストが最初に、かつ、直近に不安定な挙動を示したときの日付とコミット SHA。
* **Occurrences**: テストが不安定な挙動を示したコミット数。
* **Failure Rate**: このテストが最初に不安定になって以来、失敗したテスト実行のパーセンテージ。
* **Trend**: 不安定なテストが修正されたのか、それともまだ活発に不安定なのかを視覚的に表示します。

修正したい不安定なテストを特定したら、テストをクリックして、直近の失敗したテスト実行または最初の不安定なテスト実行を表示するリンクを参照してください。

<div class="alert alert-info"><strong>注</strong>: このテーブルは、選択された期間において最も多くのコミットが不安定だった 1000 の不安定なテストに限定されています。</div>

## 修復

不安定なテストは、過去 30 日間失敗しなかった場合、テーブルから自動的に削除されます。テスト行にカーソルを合わせると表示されるごみ箱アイコンをクリックして、不安定なテストを手動で削除することもできます。不安定な動作を再度示された場合は、再度追加されます。

## 新しい不安定なテストを確認する

1. Tests ページで、**Branches** ビューを選択します。

2. テーブルをフィルタリングして、関心のあるブランチ、サービス、またはコミットを確認します。

3. **New Flaky** 列を見て、最新のコミットでもたらされた新しい不安定なテストの数を確認します。これは、現在のブランチやリポジトリのデフォルトブランチの、以前は **Flaky Tests** テーブルに存在しなかった不安定な動作を示すテストです。

### 誤って検出された新しい不安定なテストは無視する

特定のコミットに対する新しい不安定なテストが誤って検出されたと判断した場合、そのテストを無視することができます。そのコミットが再び不安定を示すようになれば、テストは再び表示されるようになります。

**New Flaky** の番号をクリックし、**Ignore flaky tests** をクリックします。

{{< img src="ci/ignore-new-flaky-tests.png" alt="コミットに対する新しい不安定なテストをすべて無視する" style="width:100%;">}}

## 既知の不安定な失敗したテストを見張る

1. Tests ページで、**Branches** ビューを選択します。

2. テーブルをフィルタリングして、関心のあるブランチ、サービス、またはコミットを確認します。

3. **Failed** 列には、最新のコミットで失敗したテストと既知の不安定な失敗したテストの数が含まれます。既知の不安定な失敗したテストは、リポジトリの現在のブランチあるいはデフォルトブランチで挙動がおかしくなっているテストです。

{{< img src="ci/known-flaky-failed-tests.png" alt="CI Tests Branches ビューでブランチを選択し、Failed 列にテキストボックスを表示すると、1 つのテストが失敗し、1 つの既知の不安定があることがわかります" style="width:100%;">}}