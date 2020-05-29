# Spring Boot Actuator
## SpringBoot Actuatorはどのような価値を提供するか？
* アプリケーションを本番環境にプッシュするときにアプリケーションを監視および管理するのに役立つ多くの追加機能を提供
    * HTTPエンドポイントまたはJMXを使用して、アプリケーションを管理および監視
    * 監査、ヘルス、およびメトリックの収集もアプリケーションに自動的に適用

## Actuatorエンドポイントにアクセスするための2つのプロトコルは何か？
* HTTPとJMX

## どのようなActuatorエンドポイントが提供されているか？
* `/actuator`をプレフィックスとして、以下が提供される
* auditevents
    * 現在のアプリケーションの監査イベント情報を公開
* beans
    * アプリケーション内のすべてのSpring Beanの完全なリストを表示
* caches
    * 利用可能なキャッシュを公開
* conditions
    * 構成クラスおよび自動構成クラスで評価された条件と、それらが一致したまたは一致しなかった理由
* configprops
    * すべての`@ConfigurationProperties`の照合リストを表示
* env
    * Springの`ConfigurableEnvironment`からプロパティを公開
* flyway
    * 適用されたFlywayデータベースの移行を表示
* health
    * アプリケーションの正常性情報を表示
* httptrace
    * HTTPトレース情報(デフォルトでは、最後の100の HTTPリクエスト/レスポンス交換)を表示
* info
    * 任意のアプリケーション情報を表示
* integrationgraph
    * Spring Integrationグラフを表示
* loggers
    * アプリケーションのロガーの構成を表示および変更
* liquibase
    * 適用されたLiquibaseデータベースの移行を表示
* metrics
    * 現在のアプリケーションの「メトリック」情報を表示
* mappings
    * すべての`@RequestMapping`パスの照合リストを表示
* scheduledtasks
    * アプリケーションのスケジュールされたタスクを表示
* sessions
    * Spring Session-backedセッションストアからのユーザーセッションの取得と削除を許可
* shutdown
    * アプリケーションを正常にシャットダウン
* threaddump
    * スレッドダンプを実行

## infoエンドポイントはなんのために利用されるか？どのようにデータを提供するか？
* アプリケーションに関する任意の情報を追加可能
* `application.properties`に`info.app.*`を設定することでその情報が閲覧可能

## loggersエンドポイントを使用してどのようにロギングレベルを変更するか？
* `configuredLevel`を特定のレベルに設定したJSONをPOSTすることにより、ログレベルを設定可能
* `null`を設定すると、リセットされる

## タグを使用してエンドポイントにアクセスするにはどうすればよいか？
* `/metrics`エンドポイントは実行しているアプリケーションから得られるすべてのメトリクス情報を記録しているため、全てをモニターするのは不可能である。
* そのため、存在するタグを指定することで特定の情報だけを取得できるようにする。

## メトリクスは何のためにあるか？
* 現在のアプリケーションの情報取得のために使用する。
    * メモリの使用量、スレッドの使用量、GC関連の統計、クラス数、CPU使用率、ログの各レベルのイベント数、稼働時間等

## どのようにカスタムメトリクスを作成するか？
* `MeterRegistry`をコンポーネントにインジェクションする

```java
class Dictionary {
    private final List<String> words = new CopyOnWriteArrayList<>();
    Dictionary(MeterRegistry registry) {
        registry.gaugeCollectionSize("dictionary.size", Tags.empty(), this.words);
    }
}
```

## ヘルスインジケータとは何か？
* アプリケーションのヘルス情報を提供する

## ヘルスインジケータは何を提供してくれるか？
* DiskSpaceHealthIndicator
    * ディスク容量が不足していないか確認
* DataSourceHealthIndicator
    * DataSourceへの接続が取得できることを確認
* MongoHealthIndicator
    * MongoDBが稼働していることを確認

## ヘルスインジケータステータスとは何か？
* ヘルスの状態を示す

## ヘルスインジケータステータスは何が提供されているか？
* DOWN
    * SERVICE_UNAVAILABLE(503)
* OUT_OF_SERVICE
    * SERVICE_UNAVAILABLE(503)
* UP
    * OK(200)
* UNKNOWN
    * OK(200)

## どのようにヘルスインジケータスタータスの重大度を変更するか？
* `application.properties`の`management.health.status.order`に順序を設定

## サードパーティ製の外部モニタリングシステムを利用する理由はなにか？
* 可視性が上がる
