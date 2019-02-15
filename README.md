# 箭头函数

**箭头函数表达式**的语法比[函数表达式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/function)更短，并且没有自己的[this](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this)，[arguments](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments)，[super](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/super)或 [new.target](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new.target)。这些函数表达式更适用于那些本来需要匿名函数的地方，并且它们不能用作构造函数。

箭头函数相当于匿名函数，并且简化了函数定义。箭头函数有两种格式，一种像上面的，只包含一个表达式，连`{ ... }`和`return`都省略掉了。还有一种可以包含多条语句，这时候就不能省略`{ ... }`和`return`：

```javascript
// 一条语句：(参数1, 参数2, …, 参数N) => 表达式（单一）
x => x * x
// 相当于：(参数1, 参数2, …, 参数N) =>{ return 表达式; }
function (x) {
    return x * x;
}

// 当只有一个参数时，圆括号是可选的：
(单一参数) => {函数声明}
单一参数 => {函数声明}

// 两个参数:
(x, y) => x * x + y * y

// 无参数: () => {函数声明}
() => 3.14
```

```javascript
// 多条语句：(参数1, 参数2, …, 参数N) => { 函数声明 }
x => {
    if (x > 0) {
        return x * x;
    }
    else {
        return - x * x;
    }
}

// 可变参数:
(x, y, ...rest) => {
    var i, sum = x + y;
    for (i=0; i<rest.length; i++) {
        sum += rest[i];
    }
    return sum;
}

//支持剩余参数和默认参数
(参数1, 参数2, ...rest) => {函数声明}
(参数1 = 默认值1,参数2, …, 参数N = 默认值N) => {函数声明}

// 加括号的函数体返回对象字面表达式，即返回对象：参数=> ({foo: bar})
x => ({ foo: x })

//同样支持参数列表解构
let f = ([a, b] = [1, 2], {x: c} = {x: a + b}) => a + b + c;
f();  // 6
```

# VUE的基本认识

1） VUE核心库+插件==》渐进式JavaScript框架

2） 作用：动态构建用户界面

3） 作者：尤雨溪（尤大大）华裔前Google工程师，如今在阿里

## VUE的特点

- 遵循MVVM模式
- 编码简洁，体积小，运行效率高，适合移动/PC端开发
- 它本身只关注UI，可以轻松引入vue插件或其他第三库开发项目

## 与其他前端js框架的关联

- 借鉴angular的模板和数据绑定技术
- 借鉴react的组件化和虚拟DOM技术

## VUE扩展插件

- vue-cli：vue脚手架
- vue-resource(axios): ajax请求
- vue-router: 路由
- vuex：状态管理
- vue-lazyload: 图片懒加载
- vue-scroller: 页面滑动相关
- mint-ui: 基于vue的ui组件库（移动端）
- element-ui: 基于vue的ui组件库（PC端）

## MVVM

- model: data，模型，数据对象（data）
- view: DOM，试图，模板页面
- viewModel: DOM Listeners 和 Data Bindings ，视图模型（Vue的实例）

## 模板语法

- 双大括号表达式
- 指令：
  - 强制数据绑定：v-bind（缩写“:”）
  - 绑定事件监听：v-on:click（缩写"@click"）

## 计算属性和监视

计算关键字computed

监视关键字watch

## 数组更新检测

### 变异方法

Vue 包含一组观察数组的变异方法，所以它们也将会触发视图更新。这些方法如下：

- `push()` 向数据的末尾添加一个或多个元素，并返回新的长度
- `pop()` 删除并返回数组的最后一个元素
- `shift()` 删除并返回数组的第一个元素
- `unshift()` 向数组的开头添加一个或多个元素，并返回新的长度
- `splice()` 增删改，删除元素，并想数组添加新元素
- `sort()` 对数组的元素进行排序
- `reverse()` 颠倒数组中元素的顺序

这些方法把原生方法重新包装过，加了更新界面的重写

### 替换数组

非变异 (non-mutating method) 方法，例如：`filter()`, `concat()` 和 `slice()` 。这些不会改变原始数组，但**总是返回一个新数组**。当使用非变异方法时，可以用新数组替换旧数组，也能触发视图更新。

### 注意事项

由于 JavaScript 的限制，Vue 不能检测以下变动的数组：

1. 当你利用索引直接设置一个项时，例如：`vm.items[indexOfItem] = newValue`
2. 当你修改数组的长度时，例如：`vm.items.length = newLength`

为了解决第一类问题，以下两种方式都可以实现和 `vm.items[indexOfItem] = newValue` 相同的效果，同时也将触发状态更新：

```js
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)
```

你也可以使用 [`vm.$set`](https://vuejs.org/v2/api/#vm-set) 实例方法，该方法是全局方法 `Vue.set` 的一个别名：

```js
vm.$set(vm.items, indexOfItem, newValue)
```

为了解决第二类问题，你可以使用 `splice`：

```javascript
vm.items.splice(newLength)
```

## 事件处理

### 事件修饰符

修饰符是由点开头的指令后缀来表示的。

- `.stop` 阻止单击事件继续传播(冒泡)
- `.prevent` 提交事件不再重载页面（阻止事件默认行为）
- `.capture` 修饰符可以串联
- `.self`
- `.once`
- `.passive`

```js
<!-- 阻止单击事件继续传播(冒泡) -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 点击事件将只会触发一次(2.14新增) -->
<a v-on:click.once="doThis"></a>

<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发（2.3.0新增） -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```

使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止**所有的点击**，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。

不要把 `.passive` 和 `.prevent` 一起使用，因为 `.prevent` 将会被忽略，同时浏览器可能会向你展示一个警告。请记住，`.passive` 会告诉浏览器你*不*想阻止事件的默认行为。

### 按键修饰符

Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符：

```javascript
<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
<input v-on:keyup.13="submit">
```

记住所有的 `keyCode` 比较困难，所以 Vue 为最常用的按键提供了别名：

```javascript
<!-- 同上 -->
<input v-on:keyup.enter="submit">

<!-- 缩写语法 -->
<input @keyup.enter="submit">
```

全部的按键别名：

- `.enter`
- `.tab`
- `.delete` (捕获“删除”和“退格”键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

可以通过全局 `config.keyCodes` 对象[自定义按键修饰符别名](https://cn.vuejs.org/v2/api/#keyCodes)：

```javascript
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```

### 系统修饰键

> 2.1.0 新增

可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`

> 注意：在 Mac 系统键盘上，meta 对应 command 键 (⌘)。在 Windows 系统键盘 meta 对应 Windows 徽标键 (⊞)。在 Sun 操作系统键盘上，meta 对应实心宝石键 (◆)。在其他特定键盘上，尤其在 MIT 和 Lisp 机器的键盘、以及其后继产品，比如 Knight 键盘、space-cadet 键盘，meta 被标记为“META”。在 Symbolics 键盘上，meta 被标记为“META”或者“Meta”。

例如：

```javascript
<!-- Alt + C -->
<input @keyup.alt.67="clear">

<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>
```

请注意修饰键与常规按键不同，在和 `keyup` 事件一起用时，事件触发时修饰键必须处于按下状态。换句话说，只有在按住 `ctrl` 的情况下释放其它按键，才能触发 `keyup.ctrl`。而单单释放 `ctrl` 也不会触发事件。如果你想要这样的行为，请为 `ctrl` 换用 `keyCode`：`keyup.17`。

### `.exact` 修饰符

> 2.5.0 新增

`.exact` 修饰符允许你控制由精确的系统修饰符组合触发的事件。

```
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button @click.exact="onClick">A</button>
```

### 鼠标按钮修饰符

> 2.2.0 新增

- `.left`
- `.right`
- `.middle`

这些修饰符会限制处理函数仅响应特定的鼠标按钮。

## 生命周期图示

下图展示了实例的生命周期。你不需要立马弄明白所有的东西，不过随着你的不断学习和使用，它的参考价值会越来越高。

![Vue 实例生命周期](https://cn.vuejs.org/images/lifecycle.png)

常用：mounted（）初始化显示之后立即调用，beforeDestroy（）死亡之前调用

## vue动画的理解

- 操作css的transition或animation
- vue会给目标元素添加/移除特定class
- 过渡的相关类名：
  xxx-enter-active: 指定显示的transition
  xxx-leave-active: 指定隐藏的transition
  xxx-enter/xxx-leave-to: 指定隐藏时的样式

### 基本过度动画的编码

- 在目标元素外包裹<transition name="xxx">
- 定义class样式
- 指定过渡样式：transition
- 指定隐藏时样式：opacity/其他

## 过滤器

功能：对要显示的数据进行特定格式化后再显示

注意：并没有改变原本的数据，可是产生新的对应的数据

## 指令

### 常用内置指令

v-text：更新元素的textContent

v-html：更新元素的innerHTML

v-if

v-else

v-show：通过控制display样式来控制显示/隐藏

v-for

v-on：绑定事件监听，一般写为@

v-bind：强制般的解析表达式，一般写为：

v-model：双向数据绑定

ref：为某个元素注册一个唯一标识，vue对象通过**$refs** 属性访问这个元素对象

v-cloak：使用它防止闪现表达式，与css配合：[v-cloak] { display: none }

