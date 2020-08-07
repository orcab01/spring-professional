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
