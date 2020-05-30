# Spring Data JPA
## リポジトリインタフェースとはなにか？
* DBへのCRUDを自動で実装してくれるインタフェースであり、JpaRepositoryを拡張するだけで基本的なメソッドを提供してくれる

## リポジトリインタフェースはどのように定義するか？なぜクラスではなくインタフェースなのか？
* リポジトリインタフェースは以下のクラスを拡張する形で作成する
    * JpaRepository
    * PagingAndSortingRepository
    * CrudRepository
* リポジトリインタフェースの実装はアプリケーションの開始時(ApplicationContextの構成時)に自動生成されるため、インタフェースとして定義される必要がある。

## リポジトリインタフェースのメソッドの命名ルールはどのようになっているか？
* 通常「プレフィックス + フィールド名 + キーワード」で記載する
    * プレフィックス
        * find ... By
        * read ... By
        * query ... By
        * count ... By
        * get ... By
    * キーワード
        * And
        * Or
        * Is / Equals
        * Between
        * LessThan
        * LessThanEqual
        * GraterThan
        * GraterThanEqual
        * After
        * Before
        * IsNull
        * IsNotNull / NotNull
        * Like
        * NotLike 等

## SpringDataではリポジトリの実装をどのように行っているか？
* JDK dynamic proxyにより実現している

## `@Query`は何に利用可能か？
* 特定のSQLクエリを実行したい場合に使用する。
