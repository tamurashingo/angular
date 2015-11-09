<!---
# Templates
--->
# テンプレート

<!---
Templates are markup which is added to HTML to declaratively describe how the application model should be
projected to DOM as well as which DOM events should invoke which methods on the controller. Templates contain
syntaxes which are core to Angular and allows for data-binding, event-binding, template-instantiation.
-->

テンプレートは、アプリケーションモデルがDOMにどう表現されるかを宣言的に記述するために、また、
DOMイベントがコントローラ上のメソッドをどう実行するかを記述するために、HTMLに追加されたマークアップです。

テンプレートは、Angularの中核となる構文が含まれており、データ・バインディング、イベント・バインディング、
テンプレートのインスタンス化を可能にします。


<!---
The design of the template syntax has these properties:
--->

テンプレート構文の設計は以下の性質があります。

<!---
* All data-binding expressions are easily identifiable. (i.e. there is never an ambiguity whether the value should be
  interpreted as string literal or as an expression.)
* All events and their statements are easily identifiable.
* All places of DOM instantiation are easily identifiable.
* All places of variable declaration are easily identifiable.
--->

* すべてのデータバインディング式を簡単に識別できます。
  (値が文字リテラルとして、または式として解釈されるべきであるかどうかは曖昧になりません)
* すべてのイベントと文を簡単に識別できます。
* すべてのDOMインスタンスの場所を簡単に識別できます。
* すべての変数宣言は簡単に識別できます。


<!---
The above properties guarantee that the templates are easy to parse by tools (such as IDEs) and reason about by people.
At no point is it necessary to understand which directives are active or what their semantics are in order to reason
about the template runtime characteristics.
--->

上記の性質のおかげで、
テンプレートは(IDEのような)ツールが簡単に解析できたり、人が簡単に理解できたりします。
どのディレクティブが有効であるか、またはテンプレート実行時の特性について判断するためのセマンティクスは何であるか、
理解することは不要です。


<!---
## Summary
--->
## 要約

<!---
Below is a summary of the kinds of syntaxes which Angular templating supports. The syntaxes are explained in more
detail in the following sections.
--->

以下はAunguarのtemplateがサポートしている構文の要約です。
構文は後続のセクションで詳しく説明ます。


<table>
  <thead>
    <tr>
<!---
      <th>Description</th><th>Short</th><th>Canonical</th>
--->
      <th>説明</th><th>省略</th><th>標準</th>
    </tr>
  </thead>
  <tbody>
    <tr>
<!---
      <th>Text Interpolation</th>
--->
      <th>テキストの挿入</th>
      <td>
<pre>
&lt;div&gt;{{exp}}&lt;/div&gt;
</pre>

Example:
<pre>
&lt;div&gt;
  Hello {{name}}!
  &lt;br&gt;
  Goodbye {{name}}!
&lt;/div&gt;
</pre>
      </td>
      <td>
<pre>
&lt;div [text|index]="exp"&gt;&lt;/div&gt;
</pre>

Example:
<pre>
&lt;div
  [text|0]=" 'Hello' + stringify(name) + '!' "
  [text|2]=" 'Goodbye' + stringify(name) + '!' "&gt;
  &lt;b&gt;x&lt;/b&gt;
&lt;/div&gt;
</pre>
      </td>
    </tr>
    <tr>
<!---
      <th>Property Interpolation</th>
--->
      <th>プロパティの挿入</th>
      <td>
<pre>
&lt;div name="{{exp}}"&gt;&lt;/div&gt;
</pre>

Example:

<pre>
&lt;div class="{{selected}}"&gt;&lt;/div&gt;
</pre>
      </td>
      <td>
<pre>
&lt;div [name]="stringify(exp)"&gt;&lt;/div&gt;
</pre>

Example:

<pre>
&lt;div [title]="stringify(selected)"&gt;&lt;/div&gt;
</pre>
      </td>
    </tr>
    <tr>
<!---
      <th>Property binding</th>
--->
      <th>プロパティのバインディング</th>
      <td>
<pre>
&lt;div [prop]="exp"&gt;&lt;/div&gt;
</pre>

Example:

<pre>
&lt;div [hidden]="true"&gt;&lt;/div&gt;
</pre>
      </td>
      <td>
<pre>
&lt;div bind-prop="exp"&gt;&lt;/div&gt;
</pre>

Example:

<pre>
&lt;div bind-hidden="true"&gt;&lt;/div&gt;
</pre>
      </td>
    </tr>
    <tr>
<!---
      <th>Event binding (non-bubbling)</th>
--->
      <th>イベントバインディング(non-bubbling(訳注:イベントが上方に連鎖しない))</th>
      <td>
<pre>
&lt;div (event)="statement"&gt;&lt;/div&gt;
</pre>

Example:

<pre>
&lt;div (click)="doX()"&gt;&lt;/div&gt;
</pre>
      </td>
      <td>
<pre>
&lt;div on-event="statement"&gt;&lt;/div&gt;
</pre>

Example:

<pre>
&lt;video #player&gt;
  &lt;button (click)="player.play()"&gt;play&lt;/button&gt;
&lt;/video&gt;
</pre>

Or:

<pre>
&lt;div def="symbol"&gt;&lt;/div&gt;
</pre>

Example:

<pre>
&lt;video def="player"&gt;
  &lt;button on-click="player.play()"&gt;play&lt;/button&gt;
&lt;/video&gt;
</pre>
      </td>
    </tr>
    <tr>
<!---
      <th>Inline Template</th>
--->
      <th>インラインテンプレート</th>
      <td>
<pre>
&lt;div template="..."&gt;...&lt;/div&gt;
</pre>

Example:

<pre>
&lt;ul&gt;
  &lt;li template="for: #item of items"&gt;
    {{item}}
  &lt;/li&gt;
&lt;/ul&gt;
</pre>
      </td>
      <td>
<pre>
&lt;template&gt;...&lt;/template&gt;
</pre>

Example:
<pre>
&lt;ul&gt;
  &lt;template def-for:"item"
            bind-for-in="items"&gt;
    &lt;li&gt;
      {{item}}
    &lt;/li&gt;
  &lt;/template&gt;
&lt;/ul&gt;
</pre>
      </td>
    </tr>
    <tr>
<!---
      <th>Explicit Template</th>
--->
      <th>明確なテンプレート</th>
      <td>
<pre>
&lt;template&gt;...&lt;/template&gt;
</pre>

Example:

<pre>
&lt;template #for="item"
          [for-in]="items"&gt;
  _some_content_to_repeat_
&lt;/template&gt;
</pre>
      </td>
      <td>
<pre>
&lt;template&gt;...&lt;/template&gt;
</pre>

Example:

<pre>
&lt;template def-for="item"
          bind-for-in="items"&gt;
  _some_content_to_repeat_
&lt;/template&gt;
</pre>
      </td>
    </tr>
  </tbody>
</table>


<!---
## Property Binding
--->
## プロパティバインディング

<!---
Binding application model data to the UI is the most common kind of bindings in an Angular application. The bindings
are always in the form of `property-name` which is assigned an `expression`. The generic form is:
--->


AngularアプリケーションにおいてアプリケーションモデルをUIにバインディングすることは最も一般的なバインディングです。
バインディングは常に`式`で指定された`プロパティ名`という形で存在します。
一般的な形は次の通りです。

<table>
  <tr>
<!---
    <th>Short form</th>
--->
    <th>省略形</th>
    <td><pre>&lt;some-element [some-property]="expression"&gt;</pre></td>
  </tr>
  <tr>
<!---
    <th>Canonical form</th>
--->
    <th>標準系</th>
    <td><pre>&lt;some-element bind-some-property="expression"&gt;</pre></td>
  </tr>
</table>


<!---
Where:
* `some-element` can be any existing DOM element.
* `some-property` (escaped with `[]` or `bind-`) is the name of the property on `some-element`. In this case the
  dash-case is converted into camel-case `someProperty`.
* `expression` is a valid expression (as defined in section below).
--->

ここで:
* `some-element` は既存のDOM要素にできます
* `some-property` (`[]`または`bind-`でエスケープ)は`some-element`プロパティの名前です。
  この場合、ダッシュケースはキャメルケースに変換されて `someProperty` となります。
* `expression` は有効な式です(次で定義されています)。


<!---
Example:
--->

例:
```
<div [title]="user.firstName">
```


<!---
In the above example the `title` property of the `div` element will be updated whenever the `user.firstName` changes
its value.
--->


上記の例では`div`要素の`title`プロパティは`user.firstName`の値が変わったタイミングで更新されます。


<!---
Key points:
* The binding is to the element property not the element attribute.
* To prevent custom element from accidentally reading the literal `expression` on the title element, the attribute name
  is escaped. In our case the `title` is escaped to `[title]` through the addition of square brackets `[]`.
* A binding value (in this case `user.firstName`) will always be an expression, never a string literal.
--->


キーポイント:
* バインディングは要素のプロパティに対するもので、属性に対するものではありません。
* カスタム要素を誤ってリテラルの式と読み込むことを防止するため、属性名をエスケープしています。
  この例では`title`に`[]`を追加し、`[title]`にエスケープしています。
* バインド値(この場合`user.firstName`)は常に式であって、リテラル文字ではありません。


<!---
NOTE: Unlike Angular v1, Angular v2 binds to properties of elements rather than attributes of elements. This is
done to better support custom elements, and to allow binding for values other than strings.
--->

注: Angular v1と違い、Angular v2は要素の属性ではなくプロパティにバインドします。
これはより優れたカスタム要素をサポートするため、また文字以外の値のバインディングを許可するためです。


<!---
NOTE: Some editors/server side pre-processors may have trouble generating `[]` around the attribute name. For this
reason Angular also supports a canonical version which is prefixed using `bind-`.
--->

注: 一部のエディタ/サーバサイドのプリプロセッサは、属性名の周囲に`[]`を生成させるのに問題があるかもしれません。
そのためAngularは先頭に`bind-`を追加したバージョンも正式にサポートしています。


<!---
### String Interpolation
--->
### 文字の挿入

<!---
Property bindings are the only data bindings which Angular supports, but for convenience Angular supports an interpolation
syntax which is just a short hand for the data binding syntax.
--->

プロパティのバインディングはAngularがサポートしている唯一のデータバインディングですが、
便宜上Angularは簡潔にデータバインディングができる挿入構文をサポートしています。

```
<span>Hello {{name}}!</span>
```

<!---
is a short hand for:
--->
これは以下の省略形です。

```
<span [text|0]=" 'Hello ' + stringify(name) + '!' "></span>
```

<!---
The above says to bind the `'Hello ' + stringify(name) + '!'` expression to the zero-th child of the `span`'s `text`
property. The index is necessary in case there are more than one text nodes, or if the text node we wish to bind to
is not the first one.
--->

上記は`'Hello ' + stringfy(name) + '!'`式が`span`の`text`プロパティの0番目の子にバインドしていることをあらわします。
textが一つ以上あった場合、もしくは先頭以外のtextにバインドしたい場合、インデックスが必要となります。

<!---
Similarly the same rules apply to interpolation inside attributes.
--->

同様に、要素への挿入にも同じルールが適用されます。

```
<span title="Hello {{name}}!"></span>
```

<!---
is a short hand for:
--->
これは以下の省略系です。

```
<span [title]=" 'Hello ' + stringify(name) + '!' "></span>
```

<!---
NOTE: `stringify()` is a built in implicit function which converts its argument to a string representation, while
keeping `null` and `undefined` as empty strings.
--->

注: `stringify()`は暗黙的な組み込み関数であり、引数を文字列表現に変換します。
`null`と`undefined`は空文字になります。


<!---
## Local Variables
--->
## ローカル変数


<!---
## Inline Templates
--->
## インラインテンプレート

<!---
Data binding allows updating the DOM's properties, but it does not allow for changing of the DOM structure. To change
DOM structure we need the ability to define child templates, and then instantiate these templates into Views. The
Views than can be inserted and removed as needed to change the DOM structure.
--->

データバインディングはDOMのプロパティを更新できますが、DOM構造は変更できません。
DOM構造を変更するため、子テンプレートを定義でき、Viewにインスタンス化できる必要があります。
これはViewにDOM構造変更のための挿入、削除ができることよりも望まれます。

<table>
  <tr>
<!---
    <th>Short form</th>
--->
    <th>省略形</th>
    <td>
<pre>
parent template
&lt;element&gt;
  &lt;some-element template="instantiating-directive-microsyntax"&gt;child template&lt;/some-element&gt;
&lt;/element&gt;
</pre>
    </td>
  </tr>
  <tr>
<!---
    <th>Canonical form</th>
--->
    <th>正式系</th>
    <td>
<pre>
parent template
&lt;element&gt;
  &lt;template instantiating-directive-bindings&gt;
    &lt;some-element&gt;child template&lt;/some-element&gt;
  &lt;/template&gt;
&lt;/element&gt;
</pre>
    </td>
  </tr>
</table>

<!---
Where:
* `template` defines a child template and designates the anchor where Views (instances of the template) will be
  inserted. The template can be defined implicitly with `template` attribute, which turns the current element into
  a template, or explicitly with `<template>` element. Explicit declaration is longer, but it allows for having
  templates which have more than one root DOM node.
* `viewport` is required for templates. The directive is responsible for deciding when
  and in which order should child views be inserted into this location. Such a directive usually has one or
  more bindings and can be represented as either `viewport-directive-bindings` or
  `viewport-directive-microsyntax` on `template` element or attribute. See template microsyntax for more details.
--->


ここで:
* `template`は子テンプレートを定義し、View(テンプレートのインスタンス)が挿入される位地を指定します。
  テンプレートは`template`属性(現在の要素をテンプレートに変換します)を用いて暗黙的に、
  または`<template>`要素を使って明示的に定義することができます。
  明示的な宣言は長いですが、複数のrootDOMノードを持つテンプレートを持つことができます。
* `viewport`はテンプレートのために必要となります。
  ディレクティブはいつどの順番で子ビューがこの場所に挿入されるかを決定する責任があります。
  このようなディレクティブは通常一つまたは複数のバインディングがあり、`viewport-directive-bindings`または
  `template`要素/属性の`viewport-directive-microsyntax`で表現することができます。
  詳しくはテンプレートMicrosyntaxを参照してください。


<!---
Example of conditionally included template:
--->
条件付テンプレートの例:

```
Hello {{user}}!
<div template="ng-if: isAdministrator">
  ...administrator menu here...
</div>
```

<!---
In the above example the `ng-if` directive determines whether the child view (an instance of the child template) should be
inserted into the root view. The `ng-if` makes this decision based on if the `isAdministrator` binding is true.
--->

上記の例の`ng-if` ディレクティブは、子ビュー(子テンプレートのインスタンス)がrootビューに挿入されるべきかどうかを決定しています。
`ng-if`は`isAdministrator`バインディングが正かどうかを判断します。

<!---
The above example is in the short form, for better clarity let's rewrite it in the canonical form, which is functionally
identical.
--->

上記の例は省略形です。より明確にするため、機能的に等価な正式系に書き換えてみましょう。


```
Hello {{user}}!
<template [ng-if]="isAdministrator">
  <div>
    ...administrator menu here...
  </div>
</template>
```


<!---
### Template Microsyntax
--->
### テンプレートMicrosyntax

<!---
Often times it is necessary to encode a lot of different bindings into a template to control how the instantiation
of the templates occurs. One such example is `ng-for`.
--->

しばしば、テンプレートのインスタンス化で生じることをコントロールするため、
テンプレートにたくさんのバインディングをエンコードすることが必要になってきます。

```
<form #foo=form>
</form>
<ul>
  <template [ng-for] #person [ng-for-of]="people" #i="index">
    <li>{{i}}. {{person}}<li>
  </template>
</ul>
```

<!---
Where:
* `[ng-for]` triggers the for directive.
* `#person` exports the implicit `ng-for` item.
* `[ng-for-of]="people"` binds an iterable object to the `ng-for` controller.
* `#i=index` exports item index as `i`.
--->

ここで:
* `[ng-for]`はディレクティブを起動します。
* `#person`は`ng-for`の暗黙的アイテムをエクスポートします。
* `[ng-for-of]="people"`は`ng-for`コントローラへのイテレートオブジェクトをバインドします。
* `#i=index`は`i`というインデックスをエクスポートします。


<!---
The above example is explicit but quite wordy. For this reason in most situations a short hand version of the
syntax is preferable.
--->

上記の例は明確ですが、非常に長いです。
こういった理由のため、構文の省略形が好ましい場合が多くあります。

```
<ul>
  <li template="ng-for; #person; of=people; #i=index;">{{i}}. {{person}}<li>
</ul>
```

<!---
Notice how each key value pair is translated to a `key=value;` statement in the `template` attribute. This makes the
repeat syntax a much shorter, but we can do better. Turns out that most punctuation is optional in the short version
which allows us to further shorten the text.
--->


`template`属性でキーと値のペアが`key=value;`文で変換されているのが分かります。
これは繰り返し構文のとても短いものになりますが、もっと短くできます。
テキストをより短くために句読点を省略することができます。


```
<ul>
  <li template="ng-for #person of people #i=index">{{i}}. {{person}}<li>
</ul>
```

<!---
We can also optionally use `var` instead of `#` and add `:` to `for` which creates the following recommended
microsyntax for `ng-for`.
--->

また、`#`のかわりに`var`を使い、`for`のあとに`:`を追加することで、以下の推奨する`ng-for`のmicrosyntaxが作れます。


```
<ul>
  <li template="ng-for: var person of people; var i=index">{{i}}. {{person}}<li>
</ul>
```

<!---
Finally, we can move the `ng-for` keyword to the left hand side and prefix it with `*` as so:
--->

最後に、`ng-for`キーワードを左側に移動させ、プレフィックスに`*`をつけます。

```
<ul>
  <li *ng-for="var person of people; var i=index">{{i}}. {{person}}<li>
</ul>
```

<!---
The format is intentionally defined freely, so that developers of directives can build an expressive microsyntax for
their directives. The following code describes a more formal definition.
--->

フォーマットは意図的に自由に定義されています。
そのためディレクティブの開発者は表現豊かなmicrosyntaxを構築することができます。
以下のコードは正式な定義を説明したものです。


<!---
```
expression: ...                     // as defined in Expressions section
local: [a-zA-Z][a-zA-Z0-9]*         // exported variable name available for binding
internal: [a-zA-Z][a-zA-Z0-9]*      // internal variable name which the directive exports.
key: [a-z][-|_|a-z0-9]]*            // key which maps to attribute name
keyExpression: key[:|=]?expression  // binding which maps an expression to a property
varExport: [#|var]local(=internal)? // binding which exports a local variable from a directive
microsyntax: ([[key|keyExpression|varExport][;|,]?)*
```
--->

```
expression: ...                     // 式セクションで定義
local: [a-zA-Z][a-zA-Z0-9]*         // バインディングで使うためにエクスポートされた変数名
internal: [a-zA-Z][a-zA-Z0-9]*      // ディレクティブがエクスポートする内部変数名
key: [a-z][-|_|a-z0-9]]*            // 属性名にマップするキー
keyExpression: key[:|=]?expression  // プロパティへの式にマップ
varExport: [#|var]local(=internal)? // ディレクティブで使用する内部変数
microsyntax: ([[key|keyExpression|varExport][;|,]?)*
```


<!---
Where
* `expression` is an Angular expression as defined in section: Expressions.
* `local` is a local identifier for local variables.
* `internal` is an internal variable which the directive exports for binding.
* `key` is an attribute name usually only used to trigger a specific directive.
* `keyExpression` is an property name to which the expression will be bound to.
* `varExport` allows exporting of directive internal state as variables for further binding. If no `internal` name
  is specified, the exporting is to an implicit variable.
* `microsyntax` allows you to build a simple microsyntax which can still clearly identify which expressions bind to
  which properties as well as which variables are exported for binding.
--->

ここで:
* `expression`は式セクションで定義されているAngularの式です。
* `local`はローカル変数の識別子です。
* `internal`はディレクティブがバインディングで使用するためにエクスポートした内部変数です。
* `key`は通常は特定のディレクティブを起動するためだけに使われる属性名です。
* `keyExpression`は最終的にプロパティ名になる式です。
* `varExport`はバインディングのための変数としてディレクティブの内部状態をエクスポートできます。
  もし`internal`が定義されていない場合は、暗黙的な変数がエクスポートされます。
* `microsyntax`は、どの式がどのプロパティをバインドするか、どの変数がエクスポートされるか、
  明確に識別することができる簡単なmicrosyntaxを構築することができます。


<!---
NOTE: the `template` attribute must be present to make it clear to the user that a sub-template is being created. This
goes along with the philosophy that the developer should be able to reason about the template without understanding the
semantics of the instantiator directive.
--->

注: `template`属性はサブテンプレートが作成されていることをユーザに分かるようにするため、存在する必要があります。
これは開発者がインスタンスディレクティブの意味を理解しなくてもにテンプレートについて判断できるべきである、
という哲学があるためです。


<!---
## Binding Events
--->
## バインディングイベント

<!---
Binding events allows wiring events from DOM (or other components) to the Angular controller.
--->
イベントのバインディングはDOM(もしくは他のコンポーネント)からAngularのコントローラへイベントを張ることができます。

<table>
  <tr>
<!---
    <th>Short form</th>
--->
    <th>省略形</th>
    <td><pre>&lt;some-element (some-event)="statement"&gt;</pre></td>
  </tr>
  <tr>
<!---
    <th>Canonical form</th>
--->
    <th>正式系</th>
    <td><pre>&lt;some-element on-some-event="statement"&gt;</pre></td>
  </tr>
</table>

<!---
Where:
* `some-element` Any element which can generate DOM events (or has an angular directive which generates the event).
* `some-event` (escaped with `()` or `on-`) is the name of the event `some-event`. In this case the
  dash-case is converted into camel-case `someEvent`.
* `statement` is a valid statement (as defined in section below).
If the execution of the statement returns `false`, then `preventDefault`is applied on the DOM event.
--->

ここで
* `some-element` DOMイベントが生成できる(もしくはイベントを生成できるAngularのディレクティブを持つ)全要素にできます。
* `some-event` (`()`もしくは`on-`でエスケープ) は `some-event` イベントの名前です。
  この場合、ダッシュケースはキャメルケースに変換されて`someEvent`となります。
* `statement`は有効な文です(次で定義されています)。
もし文の実行結果が`false`の場合、そのDOMイベントで`preventDefault`が適用されます。


<!---
Angular listens to bubbled DOM events (as in the case of clicking on any child), as shown below:
--->

Angularは以下に示すように、(子をクリックした場合のように)バブルDOMイベントを監視します


<table>
  <tr>
<!---
    <th>Short form</th>
--->
    <th>省略形</th>
    <td><pre>&lt;some-element (some-event)="statement"&gt;</pre></td>
  </tr>
  <tr>
<!---
    <th>Canonical form</th>
--->
    <th>正式系</th>
    <td><pre>&lt;some-element on-some-event="statement"&gt;</pre></td>
  </tr>
</table>


<!---
Example:
--->
例:
```
@Component(...)
class Example {
  submit() {
    // do something when button is clicked
  }
}

<button (click)="submit()">Submit</button>
```

<!---
In the above example, when clicking on the submit button angular will invoke the `submit` method on the surrounding
component's controller.
--->

上記の例は、submitボタンがクリックされたとき、Angularは周囲のコンポーネントのコントローラ上の`submit`メソッドを実行します。


<!---
NOTE: Unlike Angular v1, Angular v2 treats event bindings as core constructs not as directives. This means that there
is no need to create an event directive for each kind of event. This makes it possible for Angular v2 to easily
bind to custom events of Custom Elements, whose event names are not known ahead of time.
--->


注: Angular v1と違い、Angular v2はイベントバインディングを中核構造として処理し、ディレクティブとしては処理しません。
これは各種イベントのためにイベントディレクティブを作る必要がないことを意味します。
このためAngular v2ではカスタム要素のカスタムイベントに、事前にイベント名が分からなくても、簡単にバインドできるようになります。

<!---
## Expressions, Statements and Formatters
--->
## 式、文、フォーマッタ

<!---
Angular templates contain expressions for binding to data and statements for binding to events. Expressions and statements
have different semantics.
--->

Angularのテンプレートはデータや文へのバインディングのための式を含みます。
式と文は違う意味を持ちます。

<!---
### Expressions
--->
### 式

<!---
Expressions can be used to bind to properties only. Expressions represent how data should be projected to the View.
Expressions should not have any side effect and should be idempotent. Examples of where expressions can be used in
Angular are:
--->
式はプロパティへのバインディングのみに使用されます。
式はデータがViewにどう表現されるかを表します。
式は副作用を持つべきではなく、冪等(訳注:毎回結果が同じ)であるべきです。
Angularで式を使うことができる場所の例:
```
<div title="{{expression}}">{{expression}}</div>
<div [title]="expression">...</div>
<div bind-title="expression">...</div>
<div template="ng-if: expression">...</div>
```


<!---
Expressions are simplified version of expression in the language in which you are writing your application. (i.e.
expressions follow JS syntax and semantics in JS and Dart syntax and semantics in Dart). Unlike expressions in the
language, binding expressions behave differently in following ways:
--->

式は、アプリケーションを書いている言語の中における簡易版の式となります。
(例:JSで書いているならJSの構文やセマンティクス、Dartで書いているならDartの構文やセマンティクス)
言語の式と違い、バインディングの式は以下の違う振る舞いがあります。

<!---
よく分からないけど、プログラミング言語の式よりも単純だよ、っていうことを言いたいっぽい
--->


<!---
* *Must be defined*: Unlike Angular v1, Angular v2 will throw an error on dereferencing fields which are not defined.
  For example: `user.name` will throw an error if `user` is defined but it does not have `name` property. If the `name`
  property is not known, it must be declared and set to some value such as empty string, `null` (or `undefined` in JS).
  This is done to allow early detection of errors in the templates.
* *Safe dereference*: Expressions `user.name` where `user` is null will throw `NullPointerException` in the language.
  In contrast Angular will silently ignore `null` on `user`. This is done because Views often have to wait for the data
  to arrive from the backend and many fields will be `null` until the data arrives. Safe dereference is so common in the
  Views that we have made it the default.
* *Single expression*: An expression must be a single statement. (i.e. no `;`)
* *No assignments*: Binding expressions can not contain assignments.
* *No keywords*: Binding expressions can not contain keywords such as: `var`, `if`, and so on.
* *Formatters*: Angular expressions can be piped through formatters to further transform the binding value.
  (See: Formatters)
--->
  

* *定義済み*: Angular v1と違い、Angular v2は定義されていないフィールドから取得しようとした際にエラーを投げます。
  例: `user`が定義されていて、`name`プロパティを持たない場合、`user.name`はエラーを投げます。
  `name`プロパティが不明な場合、宣言して何か空文字か`null`(またはJSなら`undefined`)のような値をセットするべきです。
  これはテンプレート内のエラーを早い段階で検出するためです。
* *安全な参照*: 言語上は`user`がnullである式`user.name`は`NullPointerException`を投げます。
  対照的に、Angularは黙って`user`の`null`を無視します。
  これは、Viewはバックエンドからのデータの到着を待つ必要があり、データが到着するまで多くのフィールドが`null`であるためです。
  安全な参照はViewにおいて一般的であるため、デフォルトで動作するようにしてあります。
* *単一の式*: 式は単一の文であること。(例: `;`がないこと)
* *代入禁止*: バインド式は代入を含めることができません。
* *キーワード禁止*: バインド式は次のようなキーワードを含めることができません: `var`、`if`、など
* *フォーマッタ*: Angularの式はバインドした値を変形させるため、フォーマッタへパイプ渡しすることができます。
  (フォーマッタを参照)


<!---
Examples of some expressions and their behavior:
--->

ある式とその振る舞いの例:

<!---
Given:
--->
前提:
```
class Greeter {
  name:string;
}
```

<!---
* `name` : Will result in the value of the `name` property on the `Greeter` class.
* `name.length`: Will result in either the length of the `name` string or `undefined` (`null` in Dart) if `name`
  property is `null` or `undefined`. Example of: safe dereference.
* `foo`: Will throw an error because `foo` is not declared on the `Greeter` class. Example of: Must be defined
* `name=1`: Not allowed because of assignment.
* `name; name.length`: Not allowed because of multiple statements.
--->


* `name`: `Greeter`クラスの`name`プロパティの値を得る
* `name.length`: `name`文字列の長さまたは`name`プロパティが`null`または`undefined`であれば`undefined`(Dartの場合`null`)。
  (安全な参照の例)
* `foo`: `Greeter`クラスで`foo`が定義されていないためエラーを投げる。(定義済みの例)
* `name=1`: 代入のため許可されていない。
* `name; name.length`: 複数の文があるため許可されていない。


<!---
### Statements
--->
### 文

<!---
Statements can be used to bind to events only. Statements represent actions to trigger as a response to an event.
Examples of where statements can be used in Angular are:
--->

文はイベントへのバインドのみに使われます。
文はイベントに反応して起動するアクションを表します。
Angularで文を使うことができる場所の例:
```
<div (click)="statements">...</div>
<div on-click="statements">...</div>
```

<!---
Statements are similar to statements in the language in which you are writing your application. (i.e.
statements follow JS syntax and semantics in JS and Dart syntax and semantics in Dart). Unlike statements in the
language, binding expressions behave differently in the following ways:
--->

文は、アプリケーションを書いている言語の中における文と似ています。
(例:JSで書いているならJSの構文やセマンティクス、Dartで書いているならDartの構文やセマンティクス)
言語の文と違い、バインディングの式は以下の違う振る舞いがあります。


<!---
* *Unsafe dereference*: Expressions `user.verify()` where `user` is `null` will throw `NullPointerException` in the
  language as well as in statements. (In contrast to Safe dereference in Angular expressions.) While Angular protects
  you from null dereferencing in expressions due to lazy loading of data, no such protection is required for statements,
  and doing so would make it harder to detect typos in statements.
* *Multiple statements OK*: Statements can be composed from more than one statement. (i.e. no `doA(); doB()`)
* *Assignments OK*: Event bindings can have side effects and hence assignments are allowed.
* *No keywords*: Statements can not contain keywords such as: `var`, `if`, and so on.
* *No Formatters*: Angular statements can not contain formatters. (Formatters are only useful for data binding)
--->

* *安全でない参照*: `user`が`null`のときの`user.verify()`という表現は言語におけるそれと同様に式においても`NullPointerException`を投げます。
  (Angularの式における安全な参照とは対照的)
  Angularは式での遅延読み込み時のnull参照は保護しますが、文の中のtypoを見つけやすくするため、文ではそのような保護は必要としていません。
* *複数の式OK*: 文は複数の文から構成することができます。(例:`doA(); doB()`) (訳注:原文が間違っている)
* *キーワード禁止*: 文は次のようなキーワードを含めることができません: `var`、`if`、など
* *フォーマッタなし*: Angularの式はフォーマッタを含めることができません。(フォーマッタはデータバインディングのみ有用)

<!---
## Further Reading
--->
## 参考文献

* [Template Syntax Constraints and Reasoning](https://docs.google.com/document/d/1HHy_zPLGqJj0bHMiWPzPCxn1pO5GlOYwmv-qGgl4f_s)
