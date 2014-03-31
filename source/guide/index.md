title: Getting Started
type: guide
order: 2
---

## 導入

_Vue.js is a library for building interactive web interfaces._

Vue.jsはインタラクティブなWebインターフェースを構築するためのライブラリです。

_Technically, Vue.js is focused on the [ViewModel](#ViewModel) layer of the MVVM pattern. It connects the [View](#View) and the [Model](#Model) via two way data bindings. Actual DOM manipulations and output formatting are abstracted away into [Directives](#Directives) and [Filters](#Filters)._

技術的に、Vue.jsはMVVMパターンで言う[ViewModel](#ViewModel)層にフォーカスしてます。ViewModelは[View](#View)と[Model](#Model)をTwoway（双方向）データバインディングを介して繋いでいます。実際のDOM操作や出力フォーマットについては、[Directives](#Directives)や[Filters](#Filters)にて抽象化しています。

_hilosophically, the goal is to provide the benefits of MVVM data binding with an API that is as simple as possible. Modularity and composability are also important design considerations. It is not a full-blown framework - it is designed to be simple and flexible. You can use it alone for rapid prototyping, or mix and match with other libraries for a custom front-end stack. It's also a natural fit for no-backend services such as Firebase._

哲学的には、可能な限りシンプルなAPIを通じて、MVVMデータバインディングがもたらす恩恵を提供することがゴールです。もちろんモジュール化や部品化もデザイン上の重要な検討事項です。Vue.jsは華やかなフレームワークではありません（シンプルでフレキシブルになるようにデザインされています）。高速プロトタイピングを行ったり、他のライブラリと組み合わせて独自のフロントエンド基盤を構築するために、単独で利用できます。また、Firebaseのようなバックエンドがないサービスと組み合わせると自然にフィットするでしょう。

_Vue.js' API is heavily influenced by [AngularJS], [KnockoutJS], [Ractive.js] and [Rivets.js]. Despite the similarities, I believe Vue.js offers a valuable alternative to these existing libraries by finding a sweetspot between simplicity and functionality._

Vue.jsのAPIは[AngularJS], [KnockoutJS], [Ractive.js] そして [Rivets.js]の強い影響を受けています。似ているにも関わらず、機能性とシンプルさのちょうど良いとところを見つける事によって、Vue.jsがこれら既存のライブラリに対する貴重な代替手段となると信じています。

_Even if you are already familiar with some of these terms, it is recommended that you go through the following concepts overview because your notion of these terms might be different from what they mean in the Vue.js context._

既にあなたがこれらの用語のいくつか良く知っていたとしても、次に書かれているコンセプトの概要について、一読することをお勧めします。なぜなら、これらの用語についての概念は、Vue.jsの中では異なっている場合があるからです。

## コンセプトの概要

### ViewModel

_An object that syncs the Model and the View. In Vue.js, ViewModels are instantiated with the `Vue` constructor or its sub-classes:_

ViewとModelを同期させるオブジェクト。Vue.jsでは、ViewModelは `Vue` コンストラクタ、もしくはサブクラスにてインスタンス化します。

```js
var vm = new Vue({ /* options */ })
```

_This is the primary object that you will be interacting with as a developer when using Vue.js. For more details see [Class: Vue](/api/)._

Vue.jsを使う場合、開発者がもっとも良く対話する機会があるオブジェクトです。詳細についてはこちらを参照してください。[Class: Vue](/api/)

### View

_The actual HTML/DOM that the user sees._

ユーザーが見る実際のHTML/DOM。

```js
vm.$el // The View
```

_When using Vue.js, you rarely have to touch the DOM yourself except in custom directives (explained later). View updates will be automatically triggered when the data changes. These view updates are highly granular with the precision down to a textNode. They are also batched and executed asynchronously for greater performance._

Vue.jsを使った場合、（後述する）カスタムディレクティブを除いて、DOMに触れることはほとんどありません。Viewの更新はデータの変化をトリガーに自動化されています。これらViewの更新は、TextNodeにまで至る非常にきめ細かいものです。そして、一連の動作は高いパフォーマンスのためバッチ化されており非同期で実行されます。

### Model

_A slightly modified plain JavaScript object._

少し変更されたプレーンなJavascriptのオブジェクト。

```js
vm.$data // The Model
```

_In Vue.js, models are simply plain JavaScript objects, or **data objects**. You can manipulate their properties and ViewModels that are observing them will be notified of the changes. Vue.js converts the properties on data objects into ES5 getter/setters, which allows direct manipulation without the need for dirty checking._

Vue.jsの中でのModelは、Javascriptのシンプルなオブジェクトか、**データオブジェクト**です。これらのプロパティは操作することができ、監視しているViewModelに対して変更が通知されます。View.jsではプロパティをデータオブジェクトへ変換する際に、ES5のgetter/settersを使っています。そのため、煩わしいチェックをする必要がなく、安心して直接操作することが可能です。

_The data objects are mutated in place, so modifying it by reference has the same effects as modifying `vm.$data`. This also makes it easy for multiple ViewModel instances to observe the same piece of data._

The data objects are mutated in place, so modifying it by reference has the same effects as modifying `vm.$data`.またこの仕組みは、複数のViewModelのインスタンスが同じ場所のデータオブジェクトを監視することを容易にしています。

_For technical details see [Instantiation Options: data](/api/instantiation-options.html#data)._

技術的な詳細はこちらを参照してください。[インスタンス生成オプション: data](/api/instantiation-options.html#data)

### Directives（ディレクティブ）

_Prefixed HTML attributes that tell Vue.js to do something about a DOM element._

DOMエレメントについて何を行うか、Vue.jsに命令するための接頭語付きHTML要素。

```html
<div v-text="message"></div>
```

Here the div element has a `v-text` directive with the value `message`. What it does is telling Vue.js to keep the div's textContent in sync with the ViewModel's `message` property.

ここに、`message`という値の`v-text`ディレクティブを持つdivエレメントがあります。どのような意味かというと、ViewModelの`message`プロパティとdivのテキスト内容とを同期し続けるよう、Vue.jsに命じています。

_Directives can encapsulate arbitrary DOM manipulations. For example `v-attr` manipulates an element's attributes, `v-repeat` clones an element based on an Array, `v-on` attaches event listeners... we will cover them later._

ディレクティブは任意のDOM操作をカプセル化することができます。例えば、`v-attr`はエレメントの属性を操作します。`v-repeat`は、配列に基づくエレメントの複製を行います。`v-on`は、イベントにアタッチするなど。。。後ほどカバーします。

### Mustache Bindings

_You can also use mustache-style bindings, both in text and in attributes. They are translated into `v-text` and `v-attr` directives under the hood. For example:_

また、テキストと属性の双方にて、mustache形式のバインディングも利用することができます。これらは、`v-text`と`v-attr`ディレクティブに暗黙的に変換されます。例えば、

```html
<div id="person-&#123;&#123;id&#125;&#125;">Hello &#123;&#123;name&#125;&#125;!</div>
```

_Although it is convenient, there are a few things you need to be aware of:_

これは便利ではありますが、いくつか留意事項があります。

<p class="tip">_The `src` attribute on an `<image>` element makes an HTTP requests when a value is set, so when the template is first parsed it will result in a 404. In this case `v-attr` is preferred._
`<image>`エレメントの`src`属性は値が設定された時にHTTPリクエストを要求します。そのため、テンプレートを初めて解析するタイミングでは、結果が404になります。このケースでは`v-attr`を使用してください。
</p>

<p class="tip">_Internet Explorer will remove invalid inline `style` attributes when parsing HTML, so always use `v-style` when binding inline CSS if you want to support IE._
Internet ExplorerはHTML解析する際に無効なインライン`style`要素を削除します。そのため、インラインCSSをバインディングしてIEをサポートする場合、常に`v-style`を使用してください。
</p>

<p class="tip">_You can use triple mustaches &#123;&#123;&#123; like this &#125;&#125;&#125; for unescaped HTML, which translates to `v-html` internally. However, this can open up windows for potential XSS attacks, therefore it is suggested that you only use triple mustaches when you are absolutely sure about the security of the data source, or pipe it through a custom filter that sanitizes untrusted HTML._
HTMLエスケープしない場合は、3連中括弧 &#123;&#123;&#123; このような &#125;&#125;&#125; を使用してください。内部で`v-html`に変換されます。しかし、これば潜在的なXSS攻撃に対する門戸を開くことになります。そのため、データソースに関するセキュリティについては万全であるか、この後に信頼できないHTMLをサニタイズするようなカスタムフィルタを通る場合で無い限り、3連中括弧は使用するべきではないと言えます。
</p>

### Filters

Functions that are used to process the raw values before updating the View. They are denoted by a "pipe" inside directives or bindings:

```html
<div>&#123;&#123;message | capitalize&#125;&#125;</div>
```

Now before the div's textContent is updated, the `message` value will first be passed through the `capitalize` function. For more details see [Filters in Depth](/guide/filters.html).

### Components

In Vue.js, a component is simply a ViewModel constructor registered with an ID using `Vue.component(ID, constructor)`. By having an associated ID, they can be nested in other ViewModel's templates with the `v-component` directive. This simple mechanism enables declarative reuse and composition of ViewModels in a fashion similar to [Web Components](http://www.w3.org/TR/components-intro/), without the need for latest browsers or heavy polyfills. By breaking an application into smaller components, the result is a highly decoupled and maintainable codebase. For more details, see [Composing ViewModels](/guide/composition.html).

## A Quick Example

``` html
<div id="demo">
    <h1>&#123;&#123;title | uppercase&#125;&#125;</h1>
    <ul>
        <li
            v-repeat="todos"
            v-on="click: done = !done"
            class="&#123;&#123;done ? 'done' : ''&#125;&#125;">
            &#123;&#123;content&#125;&#125;
        </li>
    </ul>
</div>
```

``` js
var demo = new Vue({
    el: '#demo',
    data: {
        title: 'todos',
        todos: [
            {
                done: true,
                content: 'Learn JavaScript'
            },
            {
                done: false,
                content: 'Learn vue.js'
            }
        ]
    }
})
```

**Result**

<div id="demo"><h1>&#123;&#123;title | uppercase&#125;&#125;</h1><ul><li v-repeat="todos" v-on="click: done = !done" class="&#123;&#123;done ? 'done' : ''&#125;&#125;">&#123;&#123;content&#125;&#125;</li></ul></div>
<script>
var demo = new Vue({
    el: '#demo',
    data: {
        title: 'todos',
        todos: [
            {
                done: true,
                content: 'Learn JavaScript'
            },
            {
                done: false,
                content: 'Learn vue.js'
            }
        ]
    }
})
</script>

Also available on [jsfiddle](http://jsfiddle.net/yyx990803/yMv7y/).

You can click on a todo to toggle it, or you can open your Browser's console and play with the `demo` object - for example, change `demo.title`, push a new object into `demo.todos`, or toggle a todo's `done` state.

You probably have a few questions in mind now - don't worry, we'll cover them soon. Next up: [Directives in Depth](/guide/directives.html).

[AngularJS]: http://angularjs.org
[KnockoutJS]: http://knockoutjs.com
[Ractive.js]: http://ractivejs.org
[Rivets.js]: http://www.rivetsjs.com