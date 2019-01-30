# `Weex` 与 `Web` 平台的差异

## `Weex`

&emsp;&emsp;`Weex` 表面上是一个客户端技术，但实际上它串联起了从本地开发、云端部署到分发的整个链路。开发者首先可在本地像编写 `web` 页面一样编写一个 `app` 的界面，然后通过命令行工具将之编译成一段 `JavaScript` 代码，生成一个 `Weex` 的 `JS bundle`；同时，开发者可以将生成的 `JS bundle` 部署至云端，然后通过网络请求或预下发的方式加载至用户的移动应用客户端；在移动应用客户端里，`Weex SDK` 会准备好一个 `JavaScript` 执行环境，并且在用户打开一个 `Weex` 页面时在这个执行环境中执行相应的 `JS bundle`，并将执行过程中产生的各种命令发送到 `native` 端进行界面渲染、数据存储、网络通信、调用设备功能及用户交互响应等功能；同时，如果用户希望使用浏览器访问这个界面，那么他可以在浏览器里打开一个相同的 `web` 页面，这个页面和移动应用使用相同的页面源代码，但被编译成适合 `Web` 展示的 `JS Bundle`，通过浏览器里的 `JavaScript` 引擎及 `Weex SDK` 运行起来的。

&emsp;&emsp;`Weex` 是一个跨平台解决方案，`Web` 平台只是其一种运行环境，除此之外还可以在 `Android` 和 `iOS` 客户端中运行。原生开发平台和 `Web` 平台之间的差异，在功能和开发体验上都有一些差异。

## 平台差异

### 上下文

&emsp;&emsp;`Weex` 主要用于编写多页的应用程序，每一页相当于原生开发中的 `View` 或者 `Activity`，并且它有着自己的上下文。尤其 `Vue` 实例在每个页面都是不同的，甚至 `Vue` 的”全局”配置（`Vue.config.xxx`）也只会影响 `Weex` 上的单个页面。在此基础上，一些 `Vue` 的 `SPA`（单页面应用）技术，如 `Vuex` 和 `vue-router` 也将单页内生效。更通俗地说，“页面”概念在 `SPA` 技术中是虚拟的，但在 `Weex` 上却是真实的。无论如何，`Vuex` 和 `vue-router` 都是独立的库，它们都有自己的概念和使用场景，我们仍可以在 `Weex` 里使用 `Vuex` 和 `vue-router`。

### `Weex` 环境中没有 `DOM`

&emsp;&emsp; `DOM`（Document Object Model），即文档对象模型，是 `HTML` 和 `XML` 文档的编程接口，是 `Web` 中的概念。`Weex` 的运行环境以原生应用为主，在 `Android` 和 `IOS` 环境中渲染出来的是原生的组件，不是 `DOM Element`。因为在 `Android` 和 `iOS` 上没有 `DOM`（`Document Object Model`），如果你要手动操作和生成 `DOM` 元素的话可能会遇到一些兼容性问题。一些与 `DOM` 相关的特性，比如 `v-html，vm.$el，template` 选项，在不同的平台上可能无法获得相同的反应。准确来说，`vm.$el` 属性类型在 `web` 环境下是 `HTMLElement`，但是在移动端并没有这个类型。实际上，它是一个由 `Weex文档对象模型` 定义的特殊数据结构。

### `Weex` 环境中没有 `BOM`

&emsp;&emsp;`BOM`（Browser Object Model），即浏览器对象模型，是浏览器环境为 `javaScript` 提供的接口。`Weex` 在原生端没有并不基于浏览器运行，不支持浏览器提供的 `BOM` 接口。`Weex` 没有提供浏览器中的 `window` 和 `screen` 对象，不支持使用全局变量。如果想要获取设备的屏幕或环境信息，可以使用 `WXEnvironment` 变量。

&emsp;&emsp;没有 `history 、location 、navigator` 对象

- `history`：保存了当前页面的历史记录，并且提供了前进后退操作。
- `location`：记录了当前页面 URL 相关的信息。
- `navigator`：记录了当前浏览器中的信息。

&emsp;&emsp;这些接口与浏览器自身的实现有关，可以控制页面的前进后退并且获取状态信息。虽然在 `Android` 和 `iOS` 中也有“历史”和“导航”的概念，但是它是用于多个管理视图之间的跳转的。换句话说，在浏览器中执行“前进”、“后退”仍然会处于同一个页签中，在原生应用中“前进”、“后退”则会真实的跳转到其他页面。

### 样式

样式表和 `CSS` 规则是由 `Weex js` 框架和原生渲染引擎管理的。要实现完整的 `CSS` 对象模型（`CSSOM：CSS Object Model`）并支持所有的 `CSS` 规则是非常困难的，而且没有这个必要。出于性能考虑，
* `Weex` 目前只支持单个类选择器，并且只支持 CSS 规则的子集。
* 在 Weex 里， 每一个 Vue 组件的样式都是 scoped。

### 事件

目前在 `Weex` 里不支持事件冒泡和捕获，因此 `Weex` 原生组件不支持事件修饰符，例如`.prevent，.capture，.stop，.self`。此外，按键修饰符以及系统修饰键 例如 `.enter，.tab，.ctrl，.shift` 在移动端基本没有意义，在 `Weex` 中也不支持。

### `Weex` 能够调用移动设备原生 `API`

&emsp;&emsp;在 Weex 中能够调用移动设备原生 API，使用方法是通过注册、调用模块来实现。其中有一些模块是 Weex 内置的，如 clipboard 、 navigator 、storage 等。

## `Overview`

### 原生组件

尽管 `Weex` 中的组件看起来很像 `HTML` 标签，但你无法使用所有 `HTML` 标签，只能使用内置组件和自定义组件。

* `<div>` 和 `<text>` 在移动端上渲染出来的都是原生组件，而不是 `HTMLElement`。

### 原生模块

对于那些不依赖于 `UI` 的功能，`Weex` 推荐将它们包装到模块中，然后使用 `weex.requireModule('xxx')` 来引入。 这是使用 `javascript` 调用原生功能的一种方法，如网络，存储，剪贴板和页面导航等功能。