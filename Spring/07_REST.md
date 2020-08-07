# REST
## RESTは何の略か？
* **RE**presentational **S**tate **T**ransfer

## リソースとは何か？
* 名前のつけることができるデータのこと

## CRUDはどういう意味か？
* Create
* Read
* Update
* Delete

## RESTはセキュアか？どのようにセキュアにするか？
* セキュアではない
* SpringSecurityによりOAuth2の認可方式の導入やOpenIDによる認証方式の導入などが必要

## RESTはスケーラブルまたは相互運用可能(interoperable)
* RESTはステートレスであるためスケール可能であり、HTTP/HTTPSプロトコルによるやり取りであるため相互運用可能

## どのHTTPメソッドをRESTでは利用するか？
* GET
* PUT
* POST
* DELETE
* PATCH

## `HttpMessageConverter`とはなにか？
* HTTPリクエストやHTTPレスポンスとJavaオブジェクトを変換するための戦略インタフェース
* コンバータごとに処理できるメディアタイプが異なるため、それに合わせて選択する必要がある

## RESTは通常ステートレスか？
* ステートレスである

## `@RequestMapping`は何をしてくれるか？
* RESTのエンドポイント(URI)のハンドラメソッドを定義する

## `@Controller`はステレオタイプか？`@RestController`はステレオタイプか？
* @Controllerはステレオタイプだが、@RestControllerはステレオタイプでない

## ステレオタイプアノテーションとはなにか？
* @Component, @Service, @Controller, @Repository
* Springのレイヤごとに分けられたアノテーション

## `@Controller`と`@RestController`の違いは何か？
* @Controllerはハンドラメソッドの結果がビューに渡される
* @RestControllerはハンドラメソッドの結果がHttpMessageConverterにより変換されて、HTTPレスポンスに書き込まれる
* @RestController自体は@Controllerと@ResponseBodyの合成アノテーションであるため、@Controllerと@ResponseBodyの組み合わせで同様の実装が可能

* 以下は同義

```java
@Controller
public XxxController {
    @RequestMapping("/xxx/{yyy}")
    @ResponseBody
    public String xxx(@PathValiable String yyy) {
        return "zzz";
    }
}
```

```java
@RestController
public XxxController {
    @RequestMapping("/xxx/{yyy}")
    public String xxx(@PathValiable String yyy) {
        return "zzz";
    }
}
```

* <https://spring.pleiades.io/spring/docs/current/spring-framework-reference/web.html#mvc-ann-controller>

## `@ResponseBody`はいつ使うか？
* リクエストハンドラメソッドの戻り値をViewResolverではなく、MessageConverterに渡したい場合

## GET/POST/PUT/DELETEが成功した場合のHTTPステータスは何を返すか？
* 設計次第！
* 200/201/204等

## `@ResponseStatus`はいつ使うか？
* 特定のHTTPステータスでレスポンスしたい場合に使う

## `@ResponseBody`はどこに使う必要があるか？`@RequestBody`は何か？
* @ResponseBodyはメソッドアノテーションとして付与する
* @RequestBodyはメソッド引数のアノテーションとして付与する
    * リクエストボディ全体を引数としたい場合や、Javaオブジェクトを引数でしてすれば、自動的にリクエストボディをJavaオブジェクトにパースしてくれる
* 補足
    * @RequestParam
        * メソッド引数のアノテーション
        * リクエストURIのクエリパラメータを引数に使いたい場合に利用
            * /xxx?yyy=zzz&aaa=bbb
    * @PathValiable
        * メソッド引数のアノテーション
        * リクエストURIのパスパラメータを引数に使いたい場合に利用
            * /xxx/{yyy}/zzz

## コントローラのコード例を見たときにそれが何をしているか理解できますか？アノテーションが適切に付与されているか説明できますか？

## SpringMVCをクラスパス上に含める必要がありますか？
* 必要あり

## `RestTemplate`の利点は何か？
* RESTクライアントを簡単に記述することができる
* ただし、Spring5の時点ではWebClientの利用が推奨されている

## `RestTemplate`のコード例を見た際に何をしているのか理解できますか？
