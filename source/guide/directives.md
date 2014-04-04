title: ディレクティブの詳細
type: guide
order: 3
---

## 概要

_If you have not used AngularJS before, you probably don't know what a directive is. Essentially, a directive is some special token in the markup that tells the library to do something to a DOM element. In Vue.js, the concept of directive is drastically simpler than that in Angular. A Vue.js directive can only appear in the form of a prefixed HTML attribute that takes the following format:_

あなたがもし以前にAngularJSを使った事がない場合、おそらくディレクティブとは何か知らないでしょう。本来、ディレクティブはライブラリに対してDOMエレメントをどう扱うのか指示するための、マークアップ上の何か特別な印です。Vue.jsでのディレクティブのコンセプトは、Angularでのそれよりも大幅にシンプルにすることです。Vue.jsのディレクティブは、次に示す書式のように接頭語付きのHTML属性として表現されます。

``` html
<element prefix-directiveId="[arg:] ( keypath | expression ) [filters...]"></element>
```

## 簡単な例

``` html
<div v-text="message"></div>
```

_Here the prefix is `v` which is the default. The directive ID is `text` and the the keypath is `message`. This directive instructs Vue.js to update the div's `textContent` whenever the `message` property on the ViewModel changes._

ここでの接頭語`v`がデフォルトです。ディレクティブIDは`text`、そしてそのキーパスは`message`です。このディレクティブはViewModelの`message`プロパティが変更されるたびにdivの`textContent`を変更します。

## インライン式

``` html
<div v-text="'hello ' + user.firstName + ' ' + user.lastName"></div>
```

_Here we are using a computed expression instead of a single property key. Vue.js automatically tracks the properties an expression depends on and refreshes the directive whenever a dependency changes. Thanks to async batch updates, even when multiple dependencies change, an expression will only be updated once every event loop._

ここでは、単純なプロパティではなく計算式を使ってみます。Vue.jsは式が依存しているプロパティを自動的に追跡しています。そして、依存先が変更されるたびにディレクティブをリフレッシュします。非同期バッチ処理のおかげで、複数の依存先が変更されたとしても、式は1回のイベントループのみで変更されます。

_You should use expressions wisely and avoid putting too much logic in your templates, especially statements with side effects (with the exception of event listener expressions). To discourage the overuse logic inside templates, Vue.js inline expressions are limited to **one statement only**. For bindings that require more complicated operations, use [Computed Properties](/guide/computed.html) instead._

あなたは式を賢く使う必要があります。特に（イベントリスナー式を除く）副作用のある構文の場合、テンプレートの中に多くのロジックを詰め込むことを避けなければなりません。テンプレートの中にロジックを詰め込みすぎることを防ぐため、Vue.jsのインライン式は**1構文のみ**に制限されています。バインディングのため、さらに複雑な操作が必要な場合、[Computed Properties](/guide/computed.html)を代わりに使用してください。

<p class="tip">_For security reasons, in inline expressions you can only access properties and methods present on the current context ViewModel and its parents._
インライン式の内部ではセキュリティ上の理由により、現在のコンテキストもしくは、その親のViewModelに存在するプロパティと関数しかアクセスできないように制限しています。</p>

## 引数

``` html
<div v-on="click : clickHandler"></div>
```

_Some directives require an argument before the keypath or expression. In this example the `click` argument indicates we want the `v-on` directive to listen for a click event and then call the `clickHandler` method of the ViewModel instance._

いくつかのディレクティブはキーパスか式の前に引数を必要としています。この例の中での`click`引数は、`v-on`ディレクティブはクリックイベントをリッスンしたいいうことを示しています。その時、ViewModelの`clickHandler`関数を呼び出します。

## フィルタ

_Filters can be appended to directive keypaths or expressions to further process the value before updating the DOM. Filters are denoted by a single pipe (`|`) as in shell scripts. For more details see [Filters in Depth](/guide/filters.html)._

フィルタはDOMを更新する前の値を処理するために、ディレクティブのキーパスや式にプロセスを追加します。フィルタは、shellスクリプトのようにパイプ(`|`)にて表現されます。詳細についてはこちらを参照してください。[Filters in Depth](/guide/filters.html)

## 複数式

_You can create multiple bindings of the same directive in a single attribute, separated by commas:_

カンマで区切る事で、ディレクティブ内の一つの属性に複数のバインディングをつくることができます。

``` html
<div v-on="
    click   : onClick,
    keyup   : onKeyup,
    keydown : onKeydown
">
</div>
```

## リテラルディレクティブ

_Some directives don't create data bindings - they simply take the attribute value as a literal string. For example the `v-component` directive:_

いくつかのディレクティブはデータバインディングを作成しないでください。それらはリテラル文字列のような値を引数に持つシンプルな属性です。例えば`v-component`ディレクティブ。

``` html
<div v-component="my-component"></div>
```

_Here `"my-component"` is not a data property - it's a string ID that Vue.js uses to lookup the corresponding Component constructor._

ここでの`"my-component"`はデータプロパティではありません。これはVue.jsが概要するコンポーネントのコンストラクタを検索する際に利用する文字列のIDです。

_Since Vue.js 0.10, you can also use mustache expressions inside literal directives. This allows you to dynamically resolve the type of component you want to use:_

Vue.js0.10以降、リテラルディレクティブの内部でmustache式も使う事ができます。これにより、使用したいコンポーネントの型を動的に解決することができます。

``` html
<div v-component="{&#123; isOwner ? 'owner-panel' : 'guest-panel' &#125;}"></div>
```

_However, note that mustache expressions inside literal directives are evaluated **only once**. After the directive has been compiled, it will no longer react to value changes. To dynamically instantiate different components at run time, use the [v-view](/api/directives.html#v-view) directive._

ただし、リテラルディレクティブ内部のmustache式の評価は**1回限り**ですので、注意してください。ディレクティブがコンパイルされた後は、値の変更に反応することはありません。実行時に動的に異なるコンポーネントをインスタンス化する場合は[v-view](/api/directives.html#v-view)を使用してください。

_A full list of literal directives can be found in the [API reference](/api/directives.html#literal-directives)._

リテラルディレクティブのすべての一覧は、[API reference](/api/directives.html#literal-directives)で見る事ができます。

## 空ディレクティブ

_Some directives don't even expect an attribute value - they simply do something to the element once and only once. For example the `v-pre` directive:_

いくつかのディレクティブは属性値を必要としていません。それらは単に一度だけ要素に対して何かを行います。例えば、`v-pre`ディレクティブ。

``` html
<div v-pre>
    <!-- markup in here will not be compiled -->
</div>
```

_A full list of empty directives can be found in the [API reference](/api/directives.html#empty-directives)._

空ディレクティブのすべての一覧は、[API reference](/api/directives.html#empty-directives)で見る事ができます。


## カスタムディレクティブを作成する

_You can register a global custom directive with the `Vue.directive()` method, passing in a **directiveID** followed by a **definition object**. A definition object can provide several hook functions (all optional):_

`Vue.directive()`メソッドに**directiveID**と次に**定義するオブジェクト**を渡すことで、グローバルなカスタムディレクティブを登録することができます。

- **bind**: _called only once, when the directive is first bound to the element._ ディレクティブが初めてバインドされる際に一度だけ呼び出されます。
- **update**: _called when the binding value changes. The new value is provided as the argument._ バインディングされている値が変更される際に呼び出されます。新しい値は引数のように渡されます。
- **unbind**: _called only once, when the directive is unbound from the element._ ディレクティブがアンバインドされる際に一度だけ呼び出されます。

**Example**

``` js
Vue.directive('my-directive', {
    bind: function (value) {
        // do preparation work
        // e.g. add event listeners or expensive stuff
        // that needs to be run only once
        // `value` is the initial value
        // 準備作業を行います
        // 例）イベントリスナや一度だけ実行すれば良いコストの高い処理
        // `value`は初期値です
    },
    update: function (value) {
        // do something based on the updated value
        // this will also be called for the initial value
        // 値の変更を元に何か行います
        // 初期値を渡して呼び出すこともできます
    },
    unbind: function () {
        // do clean up work
        // e.g. remove event listeners added in bind()
        // 事後処理を行います
        // 例）bind()にて追加されたイベントリスナーを削除する
    }
})
```

_Once registered, you can use it in Vue.js templates like this (you need to add the Vue.js prefix to it):_

ひとたび登録すると、Vue.jsのテンプレートにて以下のように使う事ができます。（それにVue.jsの接頭語を付ける必要がありますが。

``` html
<div v-my-directive="someValue"></div>
```

_When you only need the `update` function, you can pass in a single function instead of the definition object:_

`update`関数のみ必要な場合は、定義するオブジェクトの代わりに一つの関数を渡してください。

``` js
Vue.directive('my-directive', function (value) {
    // this function will be used as update()
})
```

All the hook functions will be copied into the actual **directive object**, which you can access inside these functions as their `this` context. The directive object exposes some useful properties:

すべてのフック用の関数は実際の**ディレクティブオブジェクト**にコピーされます。それらは関数内部の`this`コンテキスにてアクセスできるようになります。ディレクティブオブジェクトの便利なプロパティをいくつか挙げます。

- **el**: _the element the directive is bound to._ ディレクティブがバインドされている要素
- **key**: _the keypath of the binding, excluding arguments and filters._ 引数とフォルタを除く、バインドされたキーパス
- **arg**: _the argument, if present._ もし存在する場合は、その引数
- **expression**: _the raw, unparsed expression._ パースされる前の生の値
- **vm**: _the context ViewModel that owns this directive._ このディレクティブを所有しているコンテキストViewModel
- **value**: _the current binding value._ 現在のバインドされている値

<p class="tip">_You should treat all these properties as read-only and refrain from changing them. You can attach custom properties to the directive object too, but be careful not to accidentally overwrite existing internal ones._
これらすべてのプロパティはReadOnlyとして扱い、これらの変更は控えなければなりません。ディレクティブオブジェクトに対してカスタムプロパティを追加することもできますが、誤って既存の内部プロパティを上書きしないよう注意しなければなりません。</p>

_An example of a custom directive using some of these properties:_

これらいくつかのプロパティを使用したカスタムディレクティブの例です。

``` html
<div id="demo" v-demo="LightSlateGray : msg"></div>
```

``` js
Vue.directive('demo', {
    bind: function () {
        this.el.style.color = '#fff'
        this.el.style.backgroundColor = this.arg
    },
    update: function (value) {
        this.el.innerHTML =
            'argument - ' + this.arg + '<br>' +
            'key - ' + this.key + '<br>' +
            'value - ' + value
    }
})
var demo = new Vue({
    el: '#demo',
    data: {
        msg: 'hello!'
    }
})
```

**Result**

<div id="demo" v-demo="LightSlateGray : msg"></div>
<script>
Vue.directive('demo', {
    bind: function () {
        this.el.style.color = '#fff'
        this.el.style.backgroundColor = this.arg
    },
    update: function (value) {
        this.el.innerHTML =
            'argument - ' + this.arg + '<br>' +
            'key - ' + this.key + '<br>' +
            'value - ' + value
    }
})
var demo = new Vue({
    el: '#demo',
    data: {
        msg: 'hello!'
    }
})
</script>

### リテラルディレクティブと空ディレクティブを作成する

_If you pass in `isLiteral: true` or `isEmpty: true` when creating a custom directive, all data-binding work will be skipped for that directive, and only `bind()` and `unbind()` will be called. In literal directives the expression will still be parsed, so you can still access information such as `this.expression`, `this.key` and `this.arg`. For empty directives, Vue.js will never parse the expression even if it exists._

カスタムディレクティブを作成する際に、`isLiteral: true`もしくは`isEmpty: true`を渡すことで、ディレクティブのためのすべてのデータバインディング処理はスキップされ、`bind()`と`unbind()`のみ呼び出されます。リテラルディレクティブでは、まだ式が解析されていないため、`this.expression`、`this.key`、 `this.arg`などの情報にまだアクセスすることができます。空ディレクティブに於いては、式が渡されてもVue.jsが解析することはありません。

Example:

``` html
<div v-literal-dir="foo"></div>
```

``` js
Vue.directive('literal-dir', {
    isLiteral: true,
    bind: function () {
        console.log(this.expression) // 'foo'
    }
})
```

### 関数ディレクティブを作成する

_Vue.js encourages the developer to separate data from behavior, so instance methods are expected to be contained in the `methods` option and not inside data objects. As a result, functions inside data objects are ignored and normal directives will not be able to bind to them._

Vue.jsはデータと振る舞いとの分離を開発者に推奨しています。インスタンスメソッドは`methods`オプション内で定義され、データオブジェクト内に存在してはいけません。

To gain access to functions inside `methods` in your custom directive, you need to pass in the `isFn` option:

``` js
Vue.directive('my-handler', {
    isFn: true, // important!
    bind: function () {
        // ...
    },
    update: function (handler) {
        // the passed in value is a function
    },
    unbind: function () {
        // ...
    }
})
```

Passing in `isFn:true` also enables your custom directive to accept inline expressions like `v-on` does. For more comprehensive examples, checkout `src/directives/` in the source code.

Next: [Filters in Depth](/guide/filters.html).
