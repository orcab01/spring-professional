# DataManagement: Transactions
## トランザクションとは何か？ローカルトランザクションとグローバルトランザクションの違いは何か？
* トランザクション
    * 不可分な一連の処理
    * トランザクションはACID特性を満たす
        * Atomic(原子性)
            * トランザクションに含まれるタスクが完全に実行されるか、全く実行されないかのどちらかであること
        * Consistency(一貫性)
            * トランザクションの状態に関わらずデータベースの整合性が保たれていること
        * Isolation(独立性)
            * トランザクションを複数同時に実行しても、他のトランザクションと干渉しないこと
        * Durability(永続性)
            * トランザクションの結果は永続化されて失われないこと
* ローカルトランザクション
    * 単一リソース(データベース等)のトランザクション
    * <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/data-access.html#transaction-local>
* グローバルトランザクション
    * 複数リソース(データベース等)のトランザクション
    * 2フェーズコミットによりトランザクション制御を実現
    * <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/data-access.html#transaction-global>

## トランザクションは横断的関心事の一つか？Springではどのように実装されているか？
* トランザクションは横断的な関心事の一つである
* Springのトランザクション戦略はTransactionManagerによって定義される
    * 具体的にはTransactionManagerにはPlatformTransactionManagerとReactiveTransactionManagerという2つのインタフェースが存在
    * PlatformTransactionManagerの実装は、JDBC/JTA/Hinernate等の動作環境によって様々であり、Springが提供しているものやOSSで提供されているものをインジェクションして利用する
* TransactionManagerにはトランザクション取得時にTransactionDefinitionを指定する必要があり、そこでトランザクションの伝搬や分離、タイムアウト、読み取り専用を指定
* <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/data-access.html#transaction-strategies>
* <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/data-access.html#transaction-strategies>

## Springではどのようにトランザクションを指定すればよいか？
* 以下の２種
    * 宣言的なトランザクション
        * @Transactionalの付与
        * <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/data-access.html#transaction-declarative>
    * プログラムによるトランザクション
        * トランザクションマネージャを利用して直接コミット、ロールバックする
        * <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/data-access.html#transaction-programmatic>

## `@Transactional`は何をしてくれるか？`PlatformTransactionManager`とは何か？
* @Transactionalをクラスやメソッドに付与することで、付与したメソッドがトランザクション管理対象となる
    * アノテーションベースのAOP
* PlatformTransactionManagerはトランザクション戦略を定義するためのインタフェースのこと

## JDBCテンプレートはすでに存在するトランザクションに参加することが可能か？
* 可能だが、JDBCテンプレートで利用しているデータソースがトランザクションマネージャによるトランザクション管理下に置かれていることが前提
* 上記を満たせば、@Transactional付与メソッド内でJDBCテンプレートのコードを実行すれば自動的にトランザクションに参加する

## トランザクション分離レベルとは何か？
* 他のトランザクションとの分離レベルをどれだけ高く保つかを定義したもの
* ISOLATION_READ_UNCOMMITTED
    * コミットされていないデータを読み取る
        * ダーティリード有
        * リピータブルリード有
        * ファントムリード有
* ISOLATION_READ_COMMITTED
    * コミットされたデータを読み取る
        * ダーティリード無
        * リピータブルリード有
        * ファントムリード有
* ISOLATION_REPEATABLE_READ
    * リピータブルリードの防止
        * ダーティリード無
        * リピータブルリード無
        * ファントムリード有
* ISOLATION_SERIALIZABLE
    * ファントムリードの防止
        * ダーティリード無
        * リピータブルリード無
        * ファントムリード無

## `@EnableTransactionManagement`何のために使われるか？
* @Configurationを付与したクラスに本アノテーションを付与することで、コンテナに登録されたPlatformTransactionManagerの実装クラスを自動的に設定してくれる
* XMLの場合は、`<tx:annotation-driven transaction-manager="txManager"/>`のような記載が必要
    * tx:annotaion-drivenの場合は明示的にトランザクションマネージャのBean名を指定しない場合、transactionManagerという名前で検索するのに対し、アノテーションベースの場合はBean名は影響しないという違いがある
        * XMLでも名前指定すればよいだけだが

## トランザクションの伝搬(progagation)とはなにか？
* 既存のトランザクションが存在する場合の振る舞いのこと
* PROPAGATION_REQUIRED
    * トランザクションが存在しない場合は、新規にトランザクションを開始し、既存のトランザクションが存在する場合は、そのトランザクションに参加する
    * 内部トランザクションスコープでロールバックし、外部トランザクションスコープでコミットしようとすると、UnexpectedRollbackExceptionが発生するため注意
* PROPAGATION_REQUIRES_NEW
    * 必ず異なるトランザクションで実行する
* PROPAGATION_NESTED
    * ロールバック可能な複数のセーブポイントを持つトランザクションでのみ有効
    * 内部トランザクションでロールバックされても、外部トランザクションを続行することが可能
* <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/data-access.html#tx-propagation>

## 同一のインスタンスオブジェクト内で、@Transactional付与メソッドから異なる@Transactional付与メソッドを呼び出した場合、どのような振る舞いをするか？
* 同一インスタンスだろうと異なるインスタンスだろうと振る舞いは同じで、トランザクションの伝搬に従い、トランザクションが実行される

## `@Transactional`はどこに付与するか？クラスレベルで付与した場合の典型的な利用方法は何か？
* @Transactionalはクラスかメソッドに付与
* クラスに付与した場合は、クラス内の全メソッドに付与したものと同義
    * ただし、Spring AOPの仕組み上、privateメソッドに付与しても効果なし

## 宣言的トランザクション管理とはなにか？
* AOPに基づいたトランザクション管理方式

## デフォルトのロールバックポリシーは何か？どのように上書きできるか？
* RuntimeException発生時にロールバックされる
* @Transactionalアノテーションにパラメータを付与することで特定の例外だけロールバックさせることが可能

## JUnit4の@RunWith(SpringJUnit4ClassRunner.class)やJUnit5の@ExtendWith(SpringExtension.class)の環境下で@Testアノテーションを付与されたメソッド上での@Transactionalのロールバックポリシーは何か？
* テスト完了後に必ず(正常時でも)ロールバックされる

## "unit of work"という用語がなぜとても重要であり、JDBCのオートコミットはこのパターンに違反しているのか？
* JDBCのオートコミットは各SQLステートメントをトランザクションとして扱うため、ビジネスロジック全体をトランザクションとして扱うことができない
* ビジネスロジック全体としてロールバックが必要な場合、オートコミットを有効化すると困る

## JPAを使いたい場合、Springで何をすれば良いですか？
* <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/data-access.html#orm-jpa>

## JPAを使いながらSpringのトランザクションに参加することは可能か？
* 可能
* JpaTransactionManagerにJPAで使用するデータソースを登録すれば良い
* <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/data-access.html#orm-jpa-tx>

## JPAではどの`PlatformTransactionManager`を使えば良いですか？
* JpaTransactrionManager

##  SpringでJAPを使いたい場合何を設定すれば良いですか？SpringBootだとそれは簡単ですか？
* spring-data-jpaの依存を追加
* SpringBootではspring-boot-starter-data-jpaでJPAに必要な依存やBeanを自動構成してくれる
