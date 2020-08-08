# Aspect oriented programming
## AOPのコンセプトは何か？どのような問題を解決するか？横断的関心事とは何か？
* AOPのコンセプト
    * プログラムの特定の振る舞いをアスペクトと呼ばれる単位で分離することで、個別のプログラムに記載された横断的関心事を切り離し、一箇所で統一的に管理する
    * アスペクトの処理内容はアドバイスと呼ばれ、どのコードに処理を適用するかというタイミングをジョインポイントと呼ぶ
    * ジョインポイントはポイントカットと呼ばれる条件式で記載する
* AOPは横断的関心事を解決する
* 横断的関心事とは、セキュリティやロギングのようにプログラム共通的に必要な機能であるものの、1モジュールとしてまとめることができずコードに散乱してしまうようなコードのことを指す

## 横断的関心事の3つの例を示せ
* ロギング
* セキュリティ
* トランザクション
* 認可
* 例外処理
* キャッシュ

## AOPで横断的関心事を解決しなかった場合に上がる2つの問題点を述べよ
* ボイラーコードに溢れ、コードの可読性やメンテナンス性が下がる
* コードが集中管理されるわけではないため、変更に弱くなり、バグを生みやすくなる

## pointcut, join point, advice, aspect, weavingとはなにか？
* pointcut
    * adviceを差し込む条件式(メソッド名等)
* join point
    * adviceを差し込むポイント(メソッド前後等)
* advice
    * 差し込む処理
* aspect
    * 横断的な処理やモジュール自体
* weaving
    * アプリケーションコードの適切なポイントにaspectを差し込む処理のこと

## Springではどのように横断的関心事を解決するか？
* Spring AOPを利用する
* Spring AOPでは2つのプロキシオブジェクトにより実現される
    * JDK Dynamic proxy
        * インタフェースを実装するクラスの作成
    * CGLib
        * サブクラスの作成
* プロキシオブジェクトを作成することにより、Springはオブジェクト特定のタイミングに処理を差し込むことが可能となっている
* <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/core.html#aop-introduction-proxies>

## プロキシオブジェクトの制約は何か？
* Springのコンテナに登録されたオブジェクトのみ
* JDK Dynamic proxy
    * インタフェースの実装クラスとして実現するため、インタフェースがなくてはならない
* CGLib
    * サブクラスとして実現するため、finalクラスやfinalメソッドはNG
    * デフォルトコンストラクタが必要

## Spring AOPによってプロキシ化されるメソッドの可視性はなんである必要があるか？
* public

## Springがサポートするアドバイスのタイプは何か？
* Before
    * メソッド実行前
* AfterReturning
    * メソッド正常終了後
* AfterThrowing
    * メソッド異常終了後(例外throw後)
* After
    * メソッド終了後(finally)
* Around
    * メソッド実行前後
    * その他のアドバイスと異なり、ProceedingJoinPointを引数にもち、そのproceedメソッドを呼び出すことで実メソッドが実行される
* <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/core.html#aop-advice>

## もしtry/catchを行いたい場合、どのアドバイスを使うべきか？
* AfterThrowing
* Around
    * ProceedingJoinPoint#proceed()をtry/catchで囲めば良い

## `@Aspect`を有効化するためには何をすればよいか？
* 以下の2種
    * @Configurationが付与されたクラスに、@EnableAspectJAutoProxyを追加
    * XMLに`<aop:aspectj-autoproxy/>`を追記

## `@EnableAspectJAutoProxy`は何をしてくれるか？
* @Aspectをアスペクトとして認識し、有効化する

## ポイントカットの条件式を理解できますか？
* execution( [scope] [ReturnType] [FullClassName].[MethodName] ([Arguments]) throws [ExceptionType])
* ex) getter/setter双方にマッチするポイントカット式
    * `execution(void set*(*)) || execution(* get*())`
* <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/core.html#aop-pointcuts-designators>

## `JoinPoint`の引数は何に使えるか？
* いずれのアドバイスタイプでも利用可能な引数
    * AroundではサブクラスのProceedJoinPointを使うこと
* getArgs()
    * メソッド引数の取得
* getThis()
    * プロキシオブジェクトの取得
* getTarget()
    * ターゲットオブジェクトの取得
* getSignature()
    * アドバイスされているメソッドの説明の取得
* <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/core.html#aop-ataspectj-advice-params-the-joinpoint>

## `ProceedJoinPoint`とは何か？何に使えるか？
* Aroundアドバイスタイプのみで利用可能な引数で、実メソッドの実行に用いる
* <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/core.html#aop-ataspectj-around-advice>
