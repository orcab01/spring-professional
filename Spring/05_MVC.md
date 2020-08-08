# Spring MVC and the Web Layer
## MVCはデザインパターンの略語ですが、何を意味し、どのようなアイデアに基づいていますか？
* Model-View-Controllerの略語
    * Model
        * アプリケーションデータやビジネスロジック
    * View
        * ユーザへの表示
    * Controller
        * ユーザの入力に基づいたModel/Viewの制御
* MVCに分けることにより、関心事を分離し、ModelやControllerの再利用性を上げる

## DispatcherServletとは何か？何に利用されるか？
* リクエストに応じて適切なコントローラにリクエストをディスパッチする
* また、ビジネスロジック終了後にビュー名に対応するViewにレンダリング処理を渡す

## WebApplicationContextとはなにか？どのような追加スコープを提供するか？
* Webアプリケーション用のApplicationContextである
* WebApplicationContextは通常のApplicationContextのすべてのプロパティを所有し、追加でサーブレットコンテキストにアクセス可能
* 通常のスコープに加えて、以下のスコープを提供
    * request
        * リクエスト毎にBean生成
    * session
        * セッション毎にBean生成
    * application
        * ServletContext毎にBean生成
    * websocket
        * WebSocketのセッション毎にBean生成

## `@Controller`アノテーションは何のために利用されるか？
* そのControllerがDispatcherServletによってビジネスロジックの実行を委譲されて良いことをマークする

## どのようにしてリクエストをコントローラやメソッドにマッピングするか？
* @RequestMappingを付与する
* クラス・メソッドそれぞれに付与することができ、付与した内容はHandlerMappingによって読み込まれる
* DispatcherServletにリクエストがあった場合には、HandlerMappingにどのコントローラを利用すればよいかを確認し、HandlerMapppingはRequestMappingに応じてコントローラを返却する

## `@RequestMapping`と`@GetMapping`の違いはなにか？
* @GetMappingは`@RequestMapping(method = RequestMethod.GET)`と同義
* @GetMappingはメソッドにしか付与できないアノテーション

## `@RequestParam`は何のために利用されるか？
* リクエストのクエリパラメータやフォームのリクエストパラメータを取得したい場合に利用する

## `@RequestParam`と`@PathVariable`の違いは何か？
* パラメータの取得箇所が異なる
* @RequestParam
    * リクエストのクエリパラメータ
        * ex) http://xxx/yyy?zzz=aaa&bbb=ccc
* @PathVariable
    * リクエストのパスパラメータ
        * ex) http://xxx/{yyy}/zzz

## コントローラメソッドに設定可能な引数の型はなにか？
* WebRequest
    * 汎用インタフェース
* HttpMethod
* ServletRequest
* ServletResponse
* MultipartRequest
* HttpSession
* Principal

## コントローラメソッドの引数に付与されるアノテーションは何か？
* @RequestParam
* @PathVariable
* @RequestHeader
* @RequestBody
* @ModelAttribute
    * Modelに格納されている値
* @SessionAttribute
    * セッションに格納されている値
* @CookieValue
* @MatrixVariable
    * URL行列パラメータで利用

## コントローラメソッドの適切な戻り値の型は何か？
* String/ModelAndView/Model/void等
    * コントローラメソッドの戻り値ではViewを解決するための情報を渡す
    * 戻り値をvoidとし、HttpResponseを直接書き込むことも可能
    * コントローラメソッドに@ResponseBodyを指定した場合、直接HTTPの応答を記載することが可能
