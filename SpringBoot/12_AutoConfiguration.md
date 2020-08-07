# Spring Boot Auto Configuration
## SpringBootはどのようにして「何を設定すべきか」を知るのか？
* SpringBootの自動設定は`@EnableAutoConfiguraion`が付与されている場合に有効化される。
* 自動設定の仕組みは"@Conditional"によって実現されている。
* `@Conditional`は特定の条件下でのみBean定義を行うことを示すアノテーションであり、`Condition`インタフェースの実装クラスで条件を表現する。

## `@EnableAutoConfiguration`は何をしてくれるか？
* SpringBootの自動設定が有効化される
* クラスパスのコンポーネントをスキャンし、条件を満たす場合は自動的にBean定義する

## `@SpringBootApplication`は何をしてくれるか？
* `@Configuration`と`@EnableAutoConfiguration`と`@ComponentScan`を組み合わせたもの
    * `@Configuration`
        * コンテキストに追加の Bean を登録したり、追加の構成クラスをインポートしたりできる
    * `@EnableAutoConfiguration`
        * SpringBootの自動構成メカニズムを有効化
    * `@Componentscan`
        * アプリケーションが配置されているパッケージでコンポーネントスキャンを有効化

## SpringBootはコンポーネントスキャンを行うか？デフォルトではどこを見るか？
* `@ComponentScan`または`@SpringBootApplication`により有効化
* コンポーネントスキャンはデフォルトだとアノテーションを付与したクラスのパッケージに対してのみスキャンされる

## どのようにしてDataSourceやJdbcTemplateを自動設定させるか？
* `spring.datasource.*`をキーとするプロパティを設定
* 上記でJdbcTemplateが自動設定されるため、あとは`@Autowired`するだけ

## `spring.factories`ファイルは何のために使われるか？
* すべての自動構成クラスを定義し、アプリケーション上で何が使われる可能性があるかを推定するために使用さされる。
* SpringBootは公開されたjar内の`META-INF/spring.factories`ファイルの存在を確認し、自動構成クラスをリストアップする。

## どのようにしてSpringの自動構成をカスタマイズするか？
1. `@Configuration`を付与したクラスを作成
1. `/META-INF/spring.factories`の`EnableAutoConfiguration`に作成したクラスを登録し、自動設定候補として登録
1. `Condition`実装クラスを使用して自動設定の条件を指定

```java
@Configuration
@ConditionalOnClass(DataSource.class)
public class MySQLAutoconfiguration {
   //...
}
```

## `@Conditional`の例は？
* `@ConditionalOnclass`
    * 指定したクラスがクラスパスに存在していれば有効
* `@ConditionalOnMissingClass`
    * 指定したクラスがクラスパスに存在しなければ有効
* `@ConditionalOnBean`
    * 指定したBeanがDIコンテナ上に存在していれば有効
* `@ConditionalMissingBean`
    * 指定したBeanがDIコンテナ上に存在しなければ有効
* `@ConditionalOnExpression`
    * 指定したSpELの評価結果がtrueであれば有効
* `@ConditionalOnProperty`
    * 指定したプロパティ定義があれば有効
* `@ConditionalOnResource`
    * 指定したリソースファイルがあれば有効
* `@ConditionalOnWebApplication`
    * Webアプリケーションであれば有効
* `@ConditionalOnNotWebApplication`
    * Webアプリケーションでなければ有効
