<!---
# Directives
--->
# ディレクティブ

<!---
Directives are classes which get instantiated as a response to a particular DOM structure. By controlling the DOM structure, what directives are imported, and their selectors, the developer can use the [composition pattern](http://en.wikipedia.org/wiki/Object_composition) to get a desirable application behavior.
--->

ディレクティブは特殊なDOM構造へのレスポンスとしてインスタンス化されたクラスです。
ディレクティブがインポートしているDOM構造やセレクタを制御することで、
開発者は望ましい動作をさせるため[コンポジットパターン](http://en.wikipedia.org/wiki/Object_composition)を使うことができます。


<!---
Directives are the cornerstone of an Angular application. We use Directives to break complex problems into smaller more reusable components. Directives allow the developer to turn HTML into a DSL and then control the application assembly process.
--->

ディレクティブはAngularアプリケーションの基礎です。
複雑な問題を小さくより再利用可能なコンポーネントに分解するためにディレクティブを使います。
開発者はディレクティブを使うことでHTMLをDSLとして扱うことができるようになり、
アプリケーションの開発をコントロールできるようになります。

<!---
Angular applications do not have a main method. Instead they have a root Component. Dependency Injection then assembles the directives into a working Angular application.
--->

Angularアプリケーションはメインメソッドを持ちません。
かわりに、rootコンポーネントを持ちます。
その結果、依存性注入(Dependency Injection)はディレクティブ集めて動くAngularアプリケーションを作ります。


<!---
Directives with an encapsulated view and an optional injector are called *Components*.
--->

カプセル化されたビューと任意のインジェクターを持つディレクティブを*コンポーネント*と呼びます。


<!---
## CSS Selectors
--->
## CSSセレクタ

<!---
Directives are instantiated whenever the CSS selector matches the DOM structure.
--->

CSSセレクタがDOM構造にマッチすると、ディレクティブはインスタンス化されます。

<!---
Angular supports these CSS selector constructs:
* Element name: `name`
* Attribute: `[attribute]`
* Attribute has value: `[attribute=value]`
* Attribute contains value: `[attribute*=value]`
* Class: `.class`
* AND operation: `name[attribute]`
* OR operation: `name,.class`
* NOT operation: `:not(.class)`
--->

Angularは以下のCSSセレクタをサポートします:
* 要素名: `name`
* 属性: `[attribute]`
* 属性の値: `[attribute=value]`
* 属性が値を含む: `[attribute*=value]`
* クラス: `.class`
* AND: `name[attribute]`
* OR: `name,.class`
* NOT: `:not(.class)`

<!---
Angular does not support these (and any CSS selector which crosses element boundaries):
* Descendant: `body div`
* Direct descendant: `body > div`
* Adjacent: `div + table`
* Sibling: `div ~ table`
* Wildcard: `*`
* ID: `#id`
* Pseudo selectors: `:pseudo` other than `:not`
--->

Angularは以下をサポートしません(要素の境界をまたぐCSSセレクタ)
* 子孫: `body div`
* 直径の子孫: `body > div`
* 隣接: `div + table`
* 兄弟: `div ~ table`
* ワイルドカード: `*`
* ID: `#id`
* 擬似セレクタ: `:擬似`(`:not`以外)


<!---
Given this DOM:
--->

前提:
```<input type="text" required class="primary">```

<!---
These CSS selectors will match:
* `input`: Triggers whenever element name is `input`.
* `[required]`: Triggers whenever element contains a required attribute.
* `[type=text]`: Triggers whenever element contains attribute `type` whose value is `text`.
* `.primary`: Triggers whenever element class contains `primary`.
--->

これらのCSSセレクタはマッチします:
* `input`: 要素名が`input`
* `[required]`: required 属性を含んでいる
* `[type=text]`: `type`属性の値が`text`のものを含んでいる
* `.primary`: クラスが`primary`を含んでいる

<!---
CSS Selectors can be combined:
* `input[type=text]`: Triggers on element name `input` which is of `type` `text`.
* `input[type=text], textarea`: triggers on element name `input` which is of `type` `text` or element name `textarea`.
--->

CSSセレクタは結合可能です
* `input[type=text]`: 要素名が`input`かつ`type`が`text`
* `input[type=text], textarea`: 要素名が`input`かつ`type`が`text`、または要素名が`textarea`


<!---
## Directives
--->
## ディレクティブ

<!---
The simplest kind of directive is a decorator. Directives are useful for encapsulating behavior.
--->

最もシンプルなディレクティブはデコレータです。
ディレクティブは振る舞いをカプセル化するのに有効です。


<!---
* Multiple decorators can be placed on a single element.
* Directives do not introduce new evaluation context.
* Directives are registered through the `@Directive` meta-data annotation.
--->

* 複数のデコレータを一つの要素として配置することができます。
* ディレクティブは新しい評価コンテキストを提供しません。
* ディレクティブは`@Directive`メタデータアノテーションによって登録されます。



<!---
Here is a trivial example of a tooltip decorator. The directive will log a tooltip into the console on every time mouse enters a region:
--->

ツールチップデコレータの簡単な例です。
ディレクティブは、毎回マウスが入るたびにツールチップのログをコンソールに出力します。

<!---
```
@Directive({
  selector: '[tooltip]',     | CSS Selector which triggers the decorator
  properties: [              | List which properties need to be bound
    'text: tooltip'          |  - DOM element tooltip property should be
  ],                         |    mapped to the directive text property.
  host: {                    | List which events need to be mapped.
    '(mouseover)': 'show()'  |  - Invoke the show() method every time
  }                          |    the mouseover event is fired.
})                           |
class Form {                 | Directive controller class, instantiated
                             | when CSS matches.
  text:string;               | text property on the Directive Controller.
                             |
  show() {                   | Show method which implements the show action.
    console.log(this.text);  |
  }
}
```
--->

```
@Directive({
  selector: '[tooltip]',     | デコレータを起動するCSSセレクタ
  properties: [              | バインドが必要なプロパティのリスト
    'text: tooltip'          |  - DOM要素のtooltipプロパティは
  ],                         |    ディレクティブのtextプロパティにマップされる
  host: {                    | マッピングが必要なイベントのリスト
    '(mouseover)': 'show()'  |  - mouseoverイベントが起動されるたびに
  }                          |    show()メソッドが起動される
})                           |
class Form {                 | CSSがマッチしたときにインスタンス化される
                             | ディレクティブコントローラクラス。
  text:string;               | ディレクティブコントローラにおけるtextプロパティ
                             |
  show() {                   | showを実装したshowメソッド
    console.log(this.text);  |
  }
}
```

<!---
Example of usage:
--->
使い方の例:

<!---
```<span tooltip="Tooltip text goes here.">Some text here.</span>```
--->

```<span tooltip="ツールチップのテキストはこちら">なにかテキスト</span>```

<!---
The developer of an application can now freely use the `tooltip` attribute wherever the behavior is needed. The code above has taught the browser a new reusable and declarative behavior.
--->

アプリケーション開発者はその動きが必要な場合はいつでも`tooltip`属性を使うことができます。
上記のコードによって、ブラウザは新しく再利用可能で宣言的な振る舞いを得ることができます。

<!---
Notice that data binding will work with this decorator with no further effort as shown below.
--->

データバインディングはなんの労力の必要もなくデコレータと同時に使える。
以下参照。

<!---
```<span tooltip="Greetings {{user}}!">Some text here.</span>```
--->

```<span tooltip="こんにちは {{user}}!">なにかテキスト</span>```


<!---
## Components
--->
## コンポーネント


<!---
Component is a directive which uses shadow DOM to create encapsulate visual behavior. Components are typically used to create UI widgets or to break up the application into smaller components.
--->

コンポーネントはディレクティブであり、視覚的な振る舞いをカプセル化するためにシャドウDOMを使います。
コンポーネントは特に、UIウィジェットを作るため、アプリケーションをより小さいコンポーネントに分割するために使われます。


<!---
* Only one component can be present per DOM element.
* Component's CSS selectors usually trigger on element names. (Best practice)
* Component has its own shadow view which is attached to the element as a Shadow DOM.
* Shadow view context is the component instance. (i.e. template expressions are evaluated against the component instance.)
--->

* 一つのDOM要素には一つのコンポーネントしか存在できません。
* 通常、コンポーネントのCSSセレクタは要素名で起動するようにします。(ベストプラクティス)
* コンポーネントはそれ自身のシャドウビューを持ち、シャドウDOMとしてアタッチされます。
* シャドウビューのコンテキストはコンポーネントのインスタンスです。(例:テンプレートの式はコンポーネントのインスタンスに対して評価されます)

>> TODO(misko): Configuring the injector

<!---
Example of a component:
--->

コンポーネントの例:

<!---
```
@Component({                      | Component annotation
  selector: 'pane',               | CSS selector on <pane> element
  properties: [                   | List which property need to be bound
    'title',                      |  - title mapped to component title
    'open'                        |  - open attribute mapped to component's open property
  ],                              |
})                                |
@View({                           | View annotation
  templateUrl: 'pane.html'        |  - URL of template HTML
})                                |
class Pane {                      | Component controller class
  title:string;                   |  - title property
  open:boolean;

  constructor() {
    this.title = '';
    this.open = true;
  }

  // Public API
  toggle() => this.open = !this.open;
  open() => this.open = true;
  close() => this.open = false;
}
```
--->

```
@Component({                      | コンポーネントアノテーション
  selector: 'pane',               | <pane>に対するCSSセレクタ
  properties: [                   | バインドされるプロパティのリスト
    'title',                      |  - titleはコンポーネントのtitleにマップ
    'open'                        |  - open属性はコンポーネントのopenプロパティにマップ
  ],                              |
})                                |
@View({                           | ビューアノテーション
  templateUrl: 'pane.html'        |  - テンプレートHTMLのURL
})                                |
class Pane {                      | コンポーネントコントローラクラス
  title:string;                   |  - titleプロパティ
  open:boolean;

  constructor() {
    this.title = '';
    this.open = true;
  }

  // Public API
  toggle() => this.open = !this.open;
  open() => this.open = true;
  close() => this.open = false;
}
```



`pane.html`:
```
<div class="outer">
  <h1>{{title}}</h1>
  <div class="inner" [hidden]="!open">
    <ng-content></ng-content>
  </div>
</div>
```

`pane.css`:
```
.outer, .inner { border: 1px solid blue;}
.h1 {background-color: blue;}
```

<!---
Example of usage:
```
<pane #pane title="Example Title" open="true">
  Some text to wrap.
</pane>
<button (click)="pane.toggle()">toggle</button>
```
--->

使い方:
```
<pane #pane title="タイトル例" open="true">
  ラップするテキスト
</pane>
<button (click)="pane.toggle()">切り替え</button>
```

<!---
## Directives that use a ViewContainer
--->
## ビューコンテナを使うディレクティブ

Directives that use a ViewContainer can control instantiation of child views which are then inserted into the DOM. (Examples are `ng-if` and `ng-for`.)

ビューコンテナを使うディレクティブは、DOMに挿入される子ビューのインスタンス化を制御可能です。
(例:`ng-if`と`ng-for`)

* Every `template` element creates a `ProtoView` which can be used to create Views via the ViewContainer.
* The child views show up as siblings of the directive in the DOM.

* `template`要素は、ビューコンテナにビューが作られるときに使用する`ProtoView`を作成します。
* 子ビューはDOMの中でディレクティブの兄弟として表れます。


>> TODO(misko): Relationship with Injection
>> TODO(misko): Instantiator can not be injected into child Views


```
@Directive({
  selector: '[if]',
  properties: [
    'condition: if'
  ]
})
export class If {
  viewContainer: ViewContainerRef;
  protoViewRef: ProtoViewRef;
  view: View;

  constructor(viewContainer: ViewContainerRef, protoViewRef: ProtoViewRef) {
    this.viewContainer = viewContainer;
    this.protoViewRef = protoViewRef;
    this.view = null;
  }

  set condition(value) {
    if (value) {
      if (this.view === null) {
        this.view = this.viewContainer.create(protoViewRef);
      }
    } else {
      if (this.view !== null) {
        this.viewContainer.remove(this.view);
        this.view = null;
      }
    }
  }
}
```

<!---
## Dependency Injection
--->
## 依存性注入

<!---
Dependency Injection (DI) is a key aspect of directives. DI allows directives to be assembled into different [compositional](http://en.wikipedia.org/wiki/Object_composition) hierarchies. Angular encourages [composition over inheritance](http://en.wikipedia.org/wiki/Composition_over_inheritance) in the application design (but inheritance based approaches are still supported).
--->

依存性注入(DI)はディレクティブのキーとなる面があります。
DIを使うことで、開発者は違ったコンポジションの階層をまとめることができます。
Angnularはアプリケーション設計において[継承よりコンポジション](http://en.wikipedia.org/wiki/Composition_over_inheritance)を勧めます。
(しかし継承ベースのやり方も以前サポートします)


<!---
When Angular directives are instantiated, the directive can ask for other related directives to be injected into it. By assembling the directives in different order and subtypes the application behavior can be controlled. A good mental model is that the DOM structure controls the directive instantiation graph.
--->

Angularのディレクティブがインスタンス化されたとき、
そのディレクティブは注入すべき他の関連するディレクティブを求めることができます。
異なる順序や異なるサブタイプのディレクティブをまとめることで、
アプリケーションの振る舞いをコントロールすることができます。
DOM構造でインスタンス化のグラフをコントロールすることが、よいメンタルモデルです。

<!---
Directive instantiation is triggered by the directive CSS selector matching the DOM structure. In a directive's constructor, it can ask for other directives or application services. When asking for directives, the dependency is attempted to be located by its DOM hierarchy first, then if not found, by using the application level injector.
--->

ディレクティブのインスタンス化はディレクティブのCSSセレクタがDOM構造にマッチすることで発生します。
ディレクティブのコンストラクタでは、他のディレクティブやアプリケーションサービスを要求することができます。
ディレクティブを要求したとき、依存性は最初DOM階層から検索され、見つからなかった場合、アプリケーションレベルのインジェクタが使われます。


<!---
To better understand the kinds of injections which are supported in Angular we have broken them down into use case examples.
--->

Angularでサポートされたインジェクションの種類をより理解するために、
使用例を分解しました。


<!---
### Injecting Services
--->
### サービスの注入

<!---
Service injection is the most straight forward kind of injection which Angular supports. It involves a component configuring the `bindings` or `viewBindings` and then letting the directive ask for the configured service.
--->

サービスの注入はAngularがサポートするインジェクションのなかでもっとも素直なものです。
`bindings`または`viewBindings`で構成されるコンポーネントを起動し、
ディレクティブが構成済みのサービスを要求できるようになります。


<!---
This example illustrates how to inject `MyService` into `House` directive.
--->

この例では`MyService`が`House`ディレクティブにインジェクトされるのを表しています。

<!---
```
class MyService {}                   | Assume a service which needs to be injected
                                     | into a directive.
                                     |
@Component({                         | Assume a top level application component which
  selector: 'my-app',                | configures the services to be injected.
  viewBindings: [MyService]          |
})                                   |
@View({                              | Assume we have a template that needs to be
  templateUrl: 'my_app.html',        | configured with directives to be injected.
  directives: [House]                |
})                                   |
class MyApp {}                       |
                                     |
@Directive({                         | This is the directive into which we would like
  selector: '[house]'                | to inject the MyService.
})                                   |
class House {                        |
  constructor(myService:MyService) { | Notice that in the constructor we can simply
  }                                  | ask for MyService.
}                                    |


```
--->

```
class MyService {}                   | ディレクティブに注入されるサービス
                                     |
                                     |
@Component({                         | 注入されるサービスで構成された
  selector: 'my-app',                | トップレベルアプリケーションコンポーネント
  viewBindings: [MyService]          |
})                                   |
@View({                              | 注入されるディレクティブで構成された
  templateUrl: 'my_app.html',        | テンプレート
  directives: [House]                |
})                                   |
class MyApp {}                       |
                                     |
@Directive({                         | MyServiceをインジェクトしたいディレクティブ
  selector: '[house]'                |
})                                   |
class House {                        |
  constructor(myService:MyService) { | 単純にMyServiceを要求するコンストラクタ
  }                                  |
}                                    |


```


<!---
Assume the following DOM structure for `my_app.html`:
```
<div house>     | The house attribute triggers the creation of the House directive.
</div>          | This is equivalent to:
                |   new House(injector.get(MyService));
```
--->
`my_app.html`のDOMは以下のとおり:
```
<div house>     | house属性でHouseディレクティブが作成される。
</div>          | これは以下と等価:
                |   new House(injector.get(MyService));
```


<!---
### Injecting other Directives
--->
### 他のディレクティブの注入

<!---
Injecting other directives into directives follows a similar mechanism as injecting services into directives, but with added constraint of visibility governed by DOM structure.
--->

ディレクティブに他のディレクティブを注入することは、ディレクティブにサービスを注入するメカニズムに似ていますが、
DOM構造によって制御される視界制約が追加されます。

> (訳注)視界制約: どの要素が見えるか


<!---
There are five kinds of visibilities:
--->
5種類の視界制約です:

<!---
4種類しかないように見える...
--->

<!---
* (no annotation): Inject dependent directives only if they are on the current element.
* `@SkipSelf()`: Inject a directive if it is at any element above the current element.
* `@child`: Inject a list of direct children which match a given type. (Used with `Query`)
* `@descendant`: Inject a list of any children which match a given type. (Used with `Query`)
--->

* (アノテーションなし): 現在の要素と同じ位置にある場合のみ依存性のあるディレクティブを注入します。
* `@SkipSelf()`: 現在の要素より外側に要素がある場合、ディレクティブを注入します。
* `@child`: 指定された型にマッチした直下の子のリストを注入します。(`Query`を使います)
* `@descendant`: 指定された型にマッチした直下のすべての子のリストを注入します。(`Query`を使います)

<!---
NOTE: if the injection constraint can not be satisfied by the current visibility constraint, then it is forwarded to the normal injector which either provides a default value for the directive or throws an error.
--->

注: 注入の制約が現在の視界制約によって満たされない場合、通常のインジェクタ（ディレクティブにデフォルト値を提供する、またはエラーを投げる）に転送されます。

<!---
Here is an example of the kinds of injections which can be achieved:
--->

到達可能な注入の例:


```
@Component({                         |
  selector: 'my-app'                 |
})                                   |
@View({                              |
  templateUrl: 'my_app.html',        |
  directives: [Form, FieldSet,       |
    Field, Primary]                  |
})                                   |
class MyApp {}                       |
                                     |
@Directive({ selector: 'form' })     |
class Form {                         |
  constructor(                       |
    @descendant sets:Query<FieldSet> |
  ) {                                |
  }                                  |
}                                    |
                                     |
@Directive({ selector: 'fieldset' }) |
class FieldSet {                     |
  constructor(                       |
    @child sets:Query<Field>         |
  ) { ... }                          |
}                                    |
                                     |
@Directive({ selector: 'field' })    |
class Field {                        |
  constructor(                       |
    @SkipSelf() field:Form,          |
    @SkipSelf() field:FieldSet,      |
  ) { ... }                          |
}                                    |
                                     |
@Directive({ selector: '[primary]'}) |
class Primary {                      |
  constructor(field:Field ) { ... }  |
}                                    |
```

<!---
Assume the following DOM structure for `my_app.html`:
--->
`my_app.html`のDOM構造:
```
<form>                         |
  <div>                        |
    <fieldset>                 |
       <field primary></field> |
       <field></field>         |
    </fieldset>                |
  </div>                       |
</form>                        |
```

<!---
#### Shadow DOM effects on Directive DI
--->
#### ディレクティブIDに影響するシャドウDOM

<!---
Shadow DOM provides an encapsulation for components, so as a general rule it does not allow directive injections to cross the shadow DOM boundaries. To remedy this, declaratively specify the required component as an injectable.
--->

シャドウDOMはコンポーネントにカプセル化を提供しますが、原則として、ディレクティブの注入はシャドウDOMの境界を超えることができません。
これを改善するため、注入可能なコンポーネントを宣言的に指定します。

```
@Component({
  selector: '[kid]'
})
@View({
  templateUrl: 'kid.html',
  directives: []
})
class Kid {
  constructor(
    @SkipSelf() dad:Dad,
    @Optional() grandpa:Grandpa
  ) {
    this.name = 'Billy';
    this.dad = dad.name;
    this.grandpa = grandpa.name;
  }
}

@Component({
  selector: '[dad]'
})
@View({
  templateUrl: 'dad.html',
  directives: [Kid]
})
class Dad {
  constructor(@SkipSelf() dad:Grandpa) {
    this.name = 'Joe Jr';
    this.dad = dad.name;
  }
}

@Component({
  selector: '[grandpa]',
  viewBindings: []
})
@View({
  templateUrl: 'grandpa.html',
  directives: [Dad]
})
class Grandpa {
  constructor() {
    this.name = 'Joe';
  }
}
```

<!---
Assume the following DOM structure for `grandpa.html`: The Dad has access to the Grandpa.
--->
`grandpa.html`のDOMは以下のとおり: Dad は Grandpa へアクセスすることができます。
```
Name: {{name}}: <br> Children: <div dad></div>
```

<!---
Assume the following DOM structure for `dad.html`: Here the rendered Kid will also have access to Grandpa.
--->
`dad.html`のDOMは以下のとおり: ここでレンダリングされた Kid もまた Grandpa へアクセスすることができます。
```
Name: {{name}}: <br> Dad: {{dad}} <br> Children: <div kid></div>
```

<!---
## Further Reading
--->
## 参考文献

* [Composition](http://en.wikipedia.org/wiki/Object_composition)
* [Composition over Inheritance](http://en.wikipedia.org/wiki/Composition_over_inheritance)
