# Data Managagement: JDBC
## 検査例外と非検査例外の違いは何か？
* 検査例外はプログラム上で必ず補足(catch)しなければ、コンパイルエラーとなる例外
    * 呼び出し側で例外をハンドリングできる場合に用いる
* 非検査例外は検査例外の逆で、補足しなくてもよい(してもよい)
    * 通常は、処理の続行が困難な場合に投げる例外

## Springにおいて非検査例外が好まれる理由は何か？
* 非検査例外とすることにより、定期的なcatch/throwブロックや例外宣言を行う必要がなくなり、適切なレイヤーで必要な例外だけを処理するようにすることが可能

## データアクセス例外階層とは何のことか？
* Springでは`DataAccessException`をルート例外とし、テクノロジ固有(Jdbc, MyBatis, Hibernate等)の例外をラップする
    * テクノロジ固有の例外をラップすることにより、元の例外情報を失うリスクがなくなる他、テクノロジに寄らない一貫したコーディングが可能になる
* `DataAccessException`はルート例外であり、例外の内容に応じて例外が階層化されている
    * DataAccessException
        * NonTransientDataAccessException
            * CleanupFailureDataAccessException
            * etc...
        * TransientDataAccessException
            * QueryTimeoutException
            * etc...
        * RecoverableDataAccessException
        * ScriptException
            * etc...
* <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/data-access.html#dao-exceptions>

## Springではどのように`DataSource`を設定するか？どのBeanが開発・テストに用いるデータベースとして利用しやすいか？
* 以下の4つがある
    * JDBCDriverを用いたデータソース
        * DriverManagerDataSource
    * DBコネクションプールを用いたデータソース
        * commons-dbcp等
    * JNDIを用いたデータソース
        * JndiDataSourceLookup
    * 組み込みデータベースを用いたデータソース
        * <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/data-access.html#jdbc-embedded-database-support>
* 開発・テストに用いる際には、組み込みデータベースを用いるのが効率的である
    * 組み込みデータベースはオンメモリ上で動作するため、環境的な制約から開放される
    * 組み込みデータベースは初期化時にDDL・DMLを自動で流すことが可能であるため、セットアップが容易

## TemplateデザインパターンとJDBCテンプレートとは何か？
* TemplateMethodパターンのこと？
    * 大まかな処理の流れを抽象クラスで定義し、処理の内容は具象クラスで実装する方式のこと
    * JDBCテンプレートは具象クラスを作るわけではないので、上記とはちょっと違う気がするが、テンプレート化することによってボイラーコードをなくすことができるのだと言いたいのだと思われる
* JDBCテンプレートとは、JDBCに良く見られるボイラーコードを気にすることなく以下のことを可能とするもの
    * SQLクエリの実行
    * ステートメントとストアドプロシージャコールを更新する
    * クエリ結果のResultSetへの格納
    * JDBC例外のDataAccessExceptionへの変換
* <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/data-access.html#jdbc-JdbcTemplate>

## コールバックとは何か？クエリに利用することが可能な3つの`JdbcTeamplate`のコールバックインタフェースは何か？また、それぞれ何に役立つか？
* コールバック
    * 引数として渡されるサブルーチンのこと
* JdbcTemplateでは以下のコールバックインタフェースが用意されている
    * RowCallbackHandler
        * ResultSetの各行に対して処理するためのインタフェース
    * CallableStatementCreator
        * Connectionを指定するとCallableStatementを用意してくれる
    * PreparedStatementCreator
        * Connectionを指定するとPreparedStatementを用意してくれる
* <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/data-access.html#jdbc-JdbcTemplate>
* ※試験においてインタフェースの名前まで把握する必要はないが、サンプルコートを見たときに何をしているかがわかるレベルである必要がある

## JDBCテンプレートで素のSQLを実行することは可能か？
* 可能

## いつJDBCテンプレートはコネクションを取得・解放するか？メソッド呼び出しごとか、テンプレートごとか？
* テンプレートごと
    * JdbcTemplateのインスタンス生成時に、DataSourceインスタンスを渡す
    * あとはトランザクションマネージャに従い、トランザクションごとにコネクションの取得・解放を行う

## `JdbcTemplate`は一般的なクエリをどのようにサポートしているか？それは何(オブジェクト？マップ？リスト？)を返すか？
* query/queryForObject/queryForMap/queryForListメソッドにより一般的なクエリを実行可能
    * メソッドによってResultSetやRowMapperを使うことによるオブジェクト、Mapが返却される
