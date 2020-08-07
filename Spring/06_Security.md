# Security
## 認証と認可とはなにか？どちらが先に行われるべきか？
* 認証
    * ユーザを識別すること
* 認可
    * ユーザに何ができるか決定すること
* 認証が先

## セキュリティは横断的関心事か？どのように内部的に実装されているか？
* 横断的関心事である
* SpringAOPにより実現されている

## DelegatingFilterProxyとは何か？
* サーブレットフィルタとSpringSecurityの橋渡しをする役割
* サーブレットフィルタ上に指定した、フィルタ名と一致するBeanを取得する
    * フィルタ名を`springSecurityFilterChain`とすることで、SpringSecurityのFilterChainProxyのデフォルトBean名とリンクされる
* <https://spring.pleiades.io/spring-security/site/docs/5.1.8.BUILD-SNAPSHOT/reference/html/web-app-security.html#delegating-filter-proxy>

## SecurityFilterChainとは何か？
* セキュリティ機能を持つフィルタをチェイン(連鎖)させるもの
* FilterChainProxyから呼び出し、SecurityFilterChainが保持するFilterを利用することでセキュリティの機能を実現する
* <https://spring.pleiades.io/spring-security/site/docs/5.1.8.BUILD-SNAPSHOT/reference/html/web-app-security.html#filter-chain-proxy>

## セキュリティコンテキストとは何か？
* 実行スレッドに関するセキュリティ情報を保持するコンテキスト
* コンテキストはSecurityContextHolderに保持される

## antMatcherやmvcMatcherにおける\*\*パターンは何か？
* 指定階層以下全てのURLが対象
* 補足
    * intercept-urlに記載するpatternは最初にマッチした定義が適用される
    * \*\*はなるべくあとに書くように注意すること

## mvcMatcherがantMatcherよりも推奨される理由は何か？
* antMatcherはあくまでAnt形式の表記方法のため、それに完全一致する必要がある
* mvcMatcherはSpringMVCパターンにマッチすればよいため、MVCと親和性が高い
* antMatcher("/secured")
    * /securedのみ
* mvcMatcher("/secured")
    * /secured, /secured/, /secured.html

## SpringSecurityはパスワードハッシュに対応しているか？
* 対応している
    * BCryptPasswordEncoder
    * StandardPasswordEncoder

## メソッドセキュリティはなぜ必要か？
* 意図しないサービスへのアクセスを防ぐため
* リクエストレベルでセキュリティをかけることは可能だが、全てのリクエストとサービスの紐付けを考慮するのは難しい

## @PreAuthorizedと@RolesAllowedは何か？違いはなにか？
* メソッドセキュリティのためのアノテーション
* @PreAuthorizedはSpringSecurityのアノテーションであり、SpELを評価する
* @RolesAllowedはJavaのアノテーションであり、ロールの確認しかできない

## @PreAuthrizedと@RolesAllowedはどのように実装されているか？
* SpringAOPで実現されている

## どのセキュリティアノテーションがSpELに対応しているか？
* @PreAuthorize
* @PostAuthorize
* @PreFilter
* @PostFilter
