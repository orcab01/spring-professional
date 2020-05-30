# Spring Boot Testing
## `@SpringBootTest`アノテーションはいつ使うのか？
* SpringBootアプリケーションのテストを行いたい場合に使用する。
* 標準のSpringの場合は、spring-testの`@ContextConfugration`アノテーションを使用するが、その代替として使用可能
* アノテーションを使用することで、Springアプリケーションに必要な`ApplicationContext`を作成してくれる
* 参考
    * [Spring Boot アプリケーションのテスト](https://spring.pleiades.io/spring-boot/docs/2.1.11.RELEASE/reference/html/boot-features-testing.html#boot-features-testing-spring-boot-applications)

## `@SpringBootTest`における自動設定は何か？
* `@SpringBootApplication`で必要とする設定を自動的に検索する
* ただし、デフォルトではサーバ起動を行わないため、Webアプリケーションの場合はMockMvcを追加で構成する必要がある。
* 参考
    * [テスト構成の検出](https://spring.pleiades.io/spring-boot/docs/2.1.11.RELEASE/reference/html/boot-features-testing.html#boot-features-testing-spring-boot-applications-detecting-config)
    * [自動構成されたテスト](https://spring.pleiades.io/spring-boot/docs/2.1.11.RELEASE/reference/html/boot-features-testing.html#boot-features-testing-spring-boot-applications-testing-autoconfigured-tests)

## spring-boot-starter-testはクラスパス上にどのような依存をもたらすか？
* JUnit4
* SpringTest
* AssertJ
* Hamcrest
* Mockito
* JSONassert
* JsonPath
* 参考
    * [テスト範囲の依存関係](https://spring.pleiades.io/spring-boot/docs/2.1.11.RELEASE/reference/html/boot-features-testing.html#boot-features-test-scope-dependencies)

## `@SpringBootTest`はWebアプリケーションの統合試験にどのように対応しているか？
* `@SpringBootTest`ではサーバ起動を行わないため、モック環境で試験を行うか、実行中のサーバで試験を行う必要がある。
* モック環境でのテスト
    * `@AutoConfigureMockMvc`を使用することにより、MockMvcを追加で構成可能
    * また`@AutoConfigureWebTestClient`を使用することにより、WebTestClientを構成することも可能
    * 参考
        * [モック環境でのテスト](https://spring.pleiades.io/spring-boot/docs/2.1.11.RELEASE/reference/html/boot-features-testing.html#boot-features-testing-spring-boot-applications-testing-with-mock-environment)
* 実行中のサーバでのテスト
    * `@SpringBootTest`アノテーションに`webEnvironment`属性を追加し、`RANDOM_PORT`を指定することで、ランダムポートをLISTENする組み込みサーバを起動する。
    * 参考
        * [実行中のサーバーでテストする](https://spring.pleiades.io/spring-boot/docs/2.1.11.RELEASE/reference/html/boot-features-testing.html#boot-features-testing-spring-boot-applications-testing-with-running-server)

## `@WebMvcTest`はいつ使う必要があるか？何が自動構成されるか？
* 完全な`ApplicationContext`が不要で、Webレイヤーのみに焦点をあわせたい場合に使用。
* `@AutoConfigureMockMvc`と異なる点は、スキャン対象のBeanが`@Controller`、`@ControllerAdvice`、`@JsonComponent`、`Converter`、`GenericConverter`、`Filter`、`WebMvcConfigure`、`HandlerMethodArgumentResolver`に制限される。
    * すなわち、`@Component`はスキャンされないため、コントローラ層だけに集中できる
* `@WebMvcTest`は`MockMvc`を自動構成するためサーバが不要で、`@MockBean`と組み合わせてコンポーネントをモック化することでコントローラを素早くテスト可能。
* 参考
    * [自動構成されたSpring MVCテスト](https://spring.pleiades.io/spring-boot/docs/2.1.11.RELEASE/reference/html/boot-features-testing.html#boot-features-testing-spring-boot-applications-testing-autoconfigured-mvc-tests)

## `@MockBean`と`@Mock`の違いは何か？
* `@Mock`はMockitoライブラリの一部であり、`@MockBean`はSpringBootのクラスである。
* `@MockBean`を使用することで、Mockitoで追加したモックをApplicationContextに追加または、置き換えることが可能
    * すなわちApplicationContextにBeanとして登録されているか否かが違いである。
* 参考
    * [Beanのモックとスパイ](https://spring.pleiades.io/spring-boot/docs/2.1.11.RELEASE/reference/html/boot-features-testing.html#boot-features-testing-spring-boot-applications-mocking-beans)

## `@DataJpaTest`はどのようなときに利用されるか？何が自動構成されるか？
* JPAアプリケーションをテストしたい場合に利用する。
    * デフォルトでは`@Entity`をスキャンし、リポジトリを構成する。
    * 組み込みデータベースが利用可能な場合、自動で構成する
    * 実際のデータベースを使用したい場合は、`@AutoConfigureTestDatabase`を付与する
    * トランザクションの扱いは通常のSpringTestと同様で、テスト完了後に自動でロールバックされる
* 参考
    * [自動構成されたデータJPAテスト](https://spring.pleiades.io/spring-boot/docs/2.1.11.RELEASE/reference/html/boot-features-testing.html#boot-features-testing-spring-boot-applications-testing-autoconfigured-jpa-test)
