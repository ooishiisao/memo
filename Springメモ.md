# DI

## Bean
DIコンテナに格納されるオブジェクト。JavaBeansというわけではない。
暗黙的にインタフェース。
## Beanの定義
クラス定義の前にアノテーションを使う。
@Controller, @Service, @Repository, @Componentなど
## Beanのインスタンス化
DIする側、される側の両方がBeanである必要がある。
```java
// @AutoWiredのバターン1（推奨）
@Component //など　↓もどこかでDIされる側になる。
public class SampleClass {
    private final ExampleClass exampleClass; //finalつき

    @Autowired
    public SampleClass(ExampleClass exampleClass){
        //ExampleClassも@Componentなど付きで定義されたもの
        //Framework側でnewしてくれたものがもらえる。(new不要)
        this.exampleClass = exampleClass
    }
		
// @AutoWiredのバターン2
@Component //など　↓もどこかでDIされる側になる。
public class SampleClass {
    private ExampleClass exampleClass;

    @Autowired
    public void setExampleClass(ExampleClass exampleClass){
        //ExampleClassも@Componentなど付きで定義されたもの
        //Framework側でnewしてくれたものがもらえる。(new不要)
        this.exampleClass = exampleClass
    }

// @AutoWiredのバターン3
@Component //など　↓もどこかでDIされる側になる。
public class SampleClass {
    @Autowired
    private ExampleClass exampleClass;
    //ExampleClassも@Componentなど付きで定義されたもの
    //Framework側でnewしてくれたものがもらえる。(new不要)

// @RequiredArgsConstructorのバターン（やってることは@AutoWiredのバターン1と同じ）
import lombok.RequiredArgsConstructor;
@RequiredArgsConstructor
public class SampleClass {
    private final ExampleClass exampleClass;
    //↑private finalのメンバ変数がインスタンス化対象
    //コンストラクタ定義不要
```

# MVC

```mermaid
classDiagram

class 要求 {
	URL
	GETパラメータ
	POSTパラメータ
}
class Model {
	応答で使うデータ
	
}
class ユーザデータ
class コントローラ {
	+ハンドラ()
}
class 応答 {
	テンプレートHTML
}

要求 --> ユーザデータ : URLやパラメータを解釈して格納
Model o-- ユーザデータ : Modelに取り込まれる
Model <-- 応答 : 参照して各種置き換え
要求 --> コントローラ : URLのマッピングに\n紐づいた\nハンドラ呼び出し
コントローラ <-- ユーザデータ : 引数で渡す
コントローラ -- Model : 応答で追加で使いたい\n情報があれば\n適宜追加
コントローラ --> 応答 : 応答で使うテンプレートを返す
note for コントローラ "@Controller\n(クラス)\n要求を\n受け付ける\nクラス"
note for コントローラ "@RequstMapping\n(メソッド)\nURLや\nメソッド(GETなど)\nの紐づけ"
note for コントローラ "@ModelAttribute\n(引数)\nユーザデータ\nとの紐づけ"
note for ユーザデータ "@Data\n(クラス)"

```