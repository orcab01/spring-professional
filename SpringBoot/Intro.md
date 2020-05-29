# Spring Boot Intro
## SpringBootとは何か？
* Springベースのアプリケーションを簡単かつ高速に開発するための"Opinionated(独善的な) Framework"である。
* SpringBootを使用することで、XMLやJavaConfigによるBean定義等の設定が不要になる。
* 主な機能
    * SpringBoot Staterを使用した簡単な依存関係の管理
    * SpringBoot AutoConfiguration
    * SpringBoot Actuator
    * 組み込み型サーブレットコンテナのサポート
    * ApplicationRunnerやCommandLineRunner

## SpringBootを利用するメリットは何か？
* SpringBoot Starterは一般的なライブラリの依存関係で事前構成されているため、ユーザ側はライブラリの依存関係を調査する必要がなく、必要な依存関係が自動で構成される。
* SpringBoot AutoConfigurationにより、自動的にコンポーネントが構成される。(Bean定義が不要)
* SpringBoot Actuatorを使用することにより、Bean定義の設定やURLマッピング、環境設定やヘルスチェック等の機能を提供してくれる。
* 組み込み型サーブレットコンテナをサポートしており、アプリケーションを単体で動かすことが可能。

## なぜ"opinionated(独善的)"なのか？
* SpringBootでは簡単かつ高速に開発を行えるために、"Conversion of Configuration(設定より規約)"というアプローチを取っているためである。
    * 開発者が指定しなければならないものはアプリケーションの慣例に従わない点だけだという考え方。
    * 規約が開発者の除く動作と一致していれば設定を書く必要はなく、違った場合だけ必要な動作を設定する。

## SpringBootの設定に影響するものは何か？
* `@EnableAutoConfiguration`や`@SpringBootApplication`を設定することで設定を自動化してくれる
* 自動設定の仕組みについてはAutoConfigureation参照

## SpringBoot Starter POMとは何か？何に役立つか？
* すべての依存関係が集約されており、開発者は必要な依存を見失うことなく設定することが可能。
* また互換性のあるバージョンを設定してくれている。

## SpringBootではプロパティとYMLファイルをサポートしています。それらを見たとき、あなたはそれらを認識・理解しますか？(意味不明)
* SpringBootではapplication.propertiesとapplication.ymlを標準の設定ファイルとして読み込む。(YMLも標準サポート)
    * ただし、`@PropertySource`を用いた読み込みは不可

## SpringBootではどのようにしてロギングを制御できますか？
* SpringBootでは、内部ロギングにcommons-loggingを使用しているが、ログ実装に何を使用するかは開発者に委ねられている
    * デフォルト構成では以下が提供されている
        * Java Util Logging
        * Log4J2
        * Logback
    * Starterを使用した場合はLogbackがデフォルトで使用される
    * ログ出力先(コンソール/ファイル出力)の設定やログレベルの設定はapplicaiton.propertiesを用いて行う

## SpringBootはデフォルトではどこでプロパティファイルを検索しますか？
* 以下の優先順が存在し、優先順の高いものでオーバーライドされる。
    1. アプリケーションの実行ディレクトリ内にある"/config"サブディレクトリ
    1. アプリケーションの実行ディレクトリ内
    1. アプリケーション内の"config"パッケージ
    1. クラスパスのルート

## プロファイル固有のプロパティファイルはどのように定義できるか？
* `application-${profile}.properties`と定義する。
* アクティブなプロファイルが設定されていないときには、profileにはdefaultが設定されるため、`application-default.properties`が読み込まれる

## プロパティファイルに定義したプロパティへはどうのようにアクセス可能か？
* `@Value`を使用

```java
@Value("${jdbc.url}")
private String url;
```

* `@configurationProperties(prefix="xxx")`を使用することで、プロパティのプレフィックスを設定し、自動的にバインドさせることも可能

```java
@ConfigurationProperties(prefix="jdbc")
public class XXX {
    private String url; // jdbc.urlの値がバインド
}
```

## デフォルトのスキーマと初期データはどのように構成できるか？
* `spring.datasource.initialize`(デフォルト値: true)をtrueに設定することで、データベースの初期化を行うかを決定する。
* 上記がtrueの場合、SpringBootはクラスパスのルート上の`schema.sql`と`data.sql`ファイルを使用して初期化する。
* データベースごとにスクリプトを切り替えたい場合は、プロパティ`spring.datasource.platform`を設定し、`schema-${platform}.sql`と`data-${platform}.sql`を用意する。

## fat jarとは何か？
* 実行可能jarであり、コードの実行に必要なすべてのjarを含むアーカイブのこと。

## 組み込み型コンテナとwarの違いは何か？
* warファイルにはそれを実行するためのWebコンテナが必要
* 組み込み型コンテナの場合は、コンテナが組み込まれているため、jarファイルのみあればスタンドアロンで実行可能

## SpringBootのサポートしている組み込み型コンテナは何か？
* Tomcat
* Jetty
* Undertow

# 参考
* [SpringBootリファレンス日本語訳](https://spring.pleiades.io/spring-boot/docs/2.1.11.RELEASE/reference/html/)
