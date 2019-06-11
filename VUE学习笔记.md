---
title: vue学习笔记
date: 2019-05-06 10:13:50
tags: vue 笔记

---

#### ES6中字符串的新方法

给字符串补足位数

padStart（2，'0'）

padEnd（，''）

# vue指令

### v-if

`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。

### v-for

当 Vue.js 用 `v-for` 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。这个类似 Vue 1.x 的 `track-by="$index"` 。

这个默认的模式是高效的，但是**只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出**。

为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 `key` 属性：

``` html
<div v-for="item in items" v-bind:key="item.id">
  <!-- 内容 -->
</div>
```

### v-model

你可以用 `v-model` 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 `v-model` 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

**`v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` 特性的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 `data` 选项中声明初始值。**

`v-model` 在内部使用不同的属性为不同的输入元素并抛出不同的事件：

- text 和 textarea 元素使用 `value` 属性和 `input` 事件；
- checkbox 和 radio 使用 `checked` 属性和 `change` 事件；
- select 字段将 `value` 作为 prop 并将 `change` 作为事件。

**对于需要使用[输入法](https://zh.wikipedia.org/wiki/输入法) (如中文、日文、韩文等) 的语言，你会发现 `v-model` 不会在输入法组合文字过程中得到更新。如果你也想处理这个过程，请使用 `input` 事件。**

#### VUE自定义指令的方法

Vue中所有的指令在调用时，都以v-开头

##### 全局定义

方法：

**参数1：指令名称，定义时指令名称不需要加v-，调用时才加上**

**参数2：是一个对象，对象身上有指令相关的函数，函数在特定阶段执行相关操作**

钩子函数内的参数，（

> 第一个为el，原生dom对象,
>
> 第二个为指令传入的参数如：v-focus(200)    ）

```javascript
 Vue.directive('focus',{
    bind: function(el){  //当指令绑定元素上，执行bind函数，只执行一次
       // el.focus()  无效，因为此时dom还没有解析完成，绑定先行
    },
    inserted: function(){ //当元素插入DOM中时，执行该函数，只触发一次
    	el.focus();  //插入dom时调用
    },
    updated: function(){  //当Vnode更新时，执行updated ，可能触发多次
    }
})
```



##### 私有定义

在实例中直接添加directives对象即可

```javascript
directives:　{

	'fontweight': {
        bind: function(el,binding){
            el.style.fontWeight = bingding.value;
        },
        inserted: function(el,b){
                
        }

	}
}
```

##### 简写方式

如果只需要对bing和update钩子，可以简写如下

```javascript
directives:　{//等同于同时写了bind和updated两个钩子

	'fontsize': function(el,binding){
            el.style.fontsize = parseInt(binding.value)+'px';
}
```

# 

#### 变异方法和非变异方法

变异方法 (mutation method)，顾名思义，会改变被这些方法调用的原始数组。

Vue 包含一组观察数组的变异方法，所以它们也将会触发视图更新。这些方法如下：

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

相比之下，也有非变异 (non-mutating method) 方法，例如：`filter()`, `concat()` 和 `slice()` 。这些不会改变原始数组，但**总是返回一个新数组**。当使用非变异方法时，可以用新数组替换旧数组：

# VUE实例的生命周期

##### 主要的生命周期函数分类：![lifecycle](C:\Users\92530\Desktop\vue\lifecycle.png)

###### 1.创建期间的生命周期函数：

- beforeCreate：实例刚在内存中被创建出来，此时，还没有初始化好 data 和 methods 属性
- created：实例已经在内存中创建OK，此时 data 和 methods 已经创建OK，此时还没有开始 编译模板
- beforeMount：此时已经完成了模板的编译，但是还没有挂载到页面中
- mounted：此时，已经将编译好的模板，挂载到了页面指定的容器中显示

###### **2.运行期间的生命周期函数：**

- beforeUpdate：状态更新之前执行此函数， 此时 data 中的状态值是最新的，但是界面上显示的 数据还是旧的，因为此时还没有开始重新渲染DOM节点
- updated：实例更新完毕之后调用此函数，此时 data 中的状态值 和 界面上显示的数据，都已经完成了更新，界面已经被重新渲染好了！

###### **3.销毁期间的生命周期函数：**

- beforeDestroy：实例销毁之前调用。在这一步，实例仍然完全可用。
- destroyed：Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

# VUE组件

#### 模块化和组件化的区别

模块化：从代码逻辑的角度进行划分；方便代码分层开发，保证每个功能模块的职能单一

组件化：从UI界面的角度进行划分；前端的组件化方便UI组件的重用

VUE组件：为了拆分Vue实例的代码量，能够以不同的组件，来划分不同的功能模块

组件有的都是vue实例有的，**除了仅有的例外是像 `el`这样根实例特有的选项。**

#### 6.Vue组件化创建方式

**每个组件必须只有一个根元素**

##### 1.全局组件

1.第一种

```javascript
Vue.component('组件名称',{

template:  "这里放标签"

})
```

第二种（字面量类型的模板组件）

```javascript
Vue.component('组件名称',{

template:  ‘#这里放id’

})
```

然后在实例控制区域之外定义template标签，直接在里面写结构

使用方法都是直接：<组件名称></组件名称>

注意：**这里的组件名称如果使用驼峰命名法的话，那么在html结构中要使用-分隔开，而不能使用驼峰**

##### 2.私有组件

components属性定义内部私有组件

```javascript
components:{
	'com1':{
		template：''
}
```



#### 7.组件中的data属性

组件里的data要定义为一个函数，且函数要返回一个对象

```javascript
data:function(){
	return { }
}
//可以简写为 
	data(){
		return {}
	},
```

使用方法和实例中的是一样的

**问：为什么组件中的data 必须是一个函数呢？**

​	为了不让复用的组件之间的数据相互影响，所以使用函数来创建，且返回一个内部对象，这样子每次创建一个复用的组件时，之间的数据是相互独立的。

#### 8.组件切换（常用）

如登陆和注册窗口场景

方法1：使用v-if属性和v-else属性。

```html
	<login v-if="flag"></login>
    <register v-else="flag"></register>
    <script>
    Vue.component('login', {
      template: '<h3>登录组件</h3>'
    })

    Vue.component('register', {
      template: '<h3>注册组件</h3>'
    })

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        flag: false
      },
      methods: {}
    });
  </script>
```

方法2：使用component标签的is属性

定义一个变量名comName，用注册点击事件来更改展示组件的名称

``` html
<a href="" @click.prevent="comName='login'">登录</a>
<a href="" @click.prevent="comName='register'">注册</a>
<component *:is*="comName"></component>
```

#### 9.组件传值

##### 1.父组件向子组件传值（传递数据）

默认情况下子组件无法直接访问父组件中的data数据和methods方法

通过属性绑定的形式v-bind自定义属性可以传递给子属性

**props属性是唯一一个数组类型的，专门用来存储父组件传递来的数据，且该数据是只读的，无法重新赋值，**

**实际可修改，但是会报错**

```html
<com1 v-bind：parentmsg=“msg”></com1>
<!--子组件定义 -->     
com1: {
	template:  '<h1>子组件</h1>' ,
	props:   ['parentmsg'],//将父组件传递过来的属性在props数组中定义一下才能使用该数据
}
```

##### 2.传递方法可以让子组件向父组件传值（使用v-on）

实质：$emit()子组件向父组件传参

使用自定义事件的系统来解决这个问题。父级组件可以像处理 native DOM 事件一样通过 `v-on` 监听子组件实例的任意事件：

``` html
<blog-post
  ...
  v-on:enlarge-text="postFontSize += 0.1"
></blog-post>
```

同时子组件可以通过调用内建的 [**$emit** 方法](https://cn.vuejs.org/v2/api/#vm-emit) 并传入事件名称来触发一个事件：

```html
<button v-on:click="$emit('enlarge-text')">
  Enlarge text
</button>
```

有了这个 `v-on:enlarge-text="postFontSize += 0.1"` 监听器，父级组件就会接收该事件并更新 `postFontSize` 的值。

`<blog-post>` 组件决定它的文本要放大多少。这时可以使用 `$emit` 的第二个参数来提供这个值：

```html
//子组件
<button v-on:click="$emit('enlarge-text', 0.1)">
  Enlarge text
</button>
```

然后当在父级组件监听这个事件的时候，我们可以通过 `$event` 访问到被抛出的这个值：

```php+HTML
//父组件
<blog-post
  ...
  v-on:enlarge-text="postFontSize += $event"
></blog-post>
```

如果这个事件处理函数是一个方法：

```html
<blog-post
  ...
  v-on:enlarge-text="onEnlargeText"
></blog-post>
```

那么这个值将会作为第一个参数传入这个方法：

```javascript
methods: {
  onEnlargeText: function (enlargeAmount) {
    this.postFontSize += enlargeAmount
  }
}
```

# VUE计算属性

**计算属性与绑定方法的区别：计算属性具有缓存，只有相关依赖发生变化，才会重新进行求值。绑定方法则是在每次渲染都重新进行计算，当计算量大时，可以避免重新运算**

**计算属性是基于它们的响应式依赖进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值。

tip：侦听属性：watch

```javascript
 watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  }
```

## vue的类切换

**对象语法**

我们可以传给 `v-bind:class` 一个对象，以动态地切换 class

绑定的数据对象不必内联定义在模板里：

```html
<div v-bind:class="classObject"></div>
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

**数组语法**

我们可以把一个数组传给 `v-bind:class`，以应用一个 class 列表：

``` html
<div v-bind:class="[activeClass, errorClass]"></div>
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

渲染为：

```html
<div class="active text-danger"></div>
```

根据条件切换列表中的 class，可以用三元表达式

**在数组语法中也可以使用对象语法：**

```html
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

### VUE的$set解决检测数组变动的问题

**由于 JavaScript 的限制，Vue 不能检测以下变动的数组：**

1. 当你利用索引直接设置一个项时，例如：`vm.items[indexOfItem] = newValue`
2. 当你修改数组的长度时，例如：`vm.items.length = newLength`

举个例子：

```javascript
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的
```

为了解决第一类问题，以下两种方式都可以实现和 `vm.items[indexOfItem] = newValue` 相同的效果，同时也将触发状态更新：

```javascript
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)
```

你也可以使用 [`vm.$set`](https://cn.vuejs.org/v2/api/#vm-set) 实例方法，该方法是全局方法 `Vue.set` 的一个别名：

```javascript
vm.$set(vm.items, indexOfItem, newValue)
```

为了解决第二类问题，你可以使用 `splice`：

```javascript
vm.items.splice(newLength)
```

### 对象更改检测注意事项

还是由于 JavaScript 的限制，**Vue 不能检测对象属性的添加或删除**：

```
var vm = new Vue({
  data: {
    a: 1
  }
})
// `vm.a` 现在是响应式的

vm.b = 2
// `vm.b` 不是响应式的
```

对于已经创建的实例，Vue 不能动态添加根级别的响应式属性。但是，可以使用 `Vue.set(object, propertyName, value)` 方法向嵌套对象添加响应式属性。例如，对于：

```
var vm = new Vue({
  data: {
    userProfile: {
      name: 'Anika'
    }
  }
})
```

你可以添加一个新的 `age` 属性到嵌套的 `userProfile` 对象：

```
Vue.set(vm.userProfile, 'age', 27)
```

你还可以使用 `vm.$set` 实例方法，它只是全局 `Vue.set` 的别名：

```
vm.$set(vm.userProfile, 'age', 27)
```

有时你可能需要为已有对象赋予多个新属性，比如使用 `Object.assign()` 或 `_.extend()`。在这种情况下，你应该用两个对象的属性创建一个新的对象。所以，如果你想添加新的响应式属性，不要像这样：

```
Object.assign(vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```

你应该这样做：

```
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```

## 显示数组过滤/排序 副本

我们想要显示一个数组的过滤或排序副本，而不实际改变或重置原始数据。在这种情况下，可以创建返回过滤或排序数组的计算属性。

在计算属性不适用的情况下 (例如，在嵌套 `v-for` 循环中) 你可以使用一个 method 方法：

```javascript
<li v-for="n in even(numbers)">{{ n }}</li>

data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

# VUE事件

#### 事件修饰符

在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。尽管我们可以在方法中轻松实现这点，但更好的方式是：方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。

为了解决这个问题，Vue.js 为 `v-on` 提供了**事件修饰符**。之前提过，修饰符是由点开头的指令后缀来表示的。

- `.stop`

- `.prevent`

- `.capture`

- `.self`   

- `.once `  只触发一次

- `.passive`  Vue 还对应 [`addEventListener` 中的 `passive` 选项](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#Parameters)提供了 `.passive` 修饰符。

  ```
  <!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
  <!-- 而不会等待 `onScroll` 完成  -->
  <!-- 这其中包含 `event.preventDefault()` 的情况 -->
  <div v-on:scroll.passive="onScroll">...</div>
  ```

  这个 `.passive` 修饰符尤其能够提升移动端的性能。

- ```HTML
  <!-- 阻止单击事件继续传播 -->
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
  ```

**TIP:**使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止**所有的点击**，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。

#### VUE的按键修饰符：监听固定按键

**监听固定按键**

- #### `.enter`

- `.tab`

- `.delete` (捕获“删除”和“退格”键)

- `.esc`

- `.space`

- `.up`

- `.down`

- `.left`

- `.right`

也可使用键盘码

**自定义全局按键修饰符的方法**

Vue.config.keyCodes.f2 = 113;

```
//@keyup.enter="add()"
```

# VUE动画和过渡

Vue 提供了 `transition` 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡

- 条件渲染 (使用 `v-if`)
- 条件展示 (使用 `v-show`)
- 动态组件
- 组件根节点

这里是一个典型的例子：

``` html
<div id="demo">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
</div>
```

```javascript
new Vue({
  el: '#demo',
  data: {
    show: true
  }
})
```

```css
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
```

# VUE混入

# VUE渲染函数

1. render方法的实质就是生成template模板； 
2. 通过调用一个方法来生成，而这个方法是通过render方法的参数传递给它的； 
3. 这个方法有三个参数，分别提供标签名，标签相关属性，标签内部的html内容 
4. 通过这三个参数，可以生成一个完整的木模板



### 虚拟dom

Vue 通过建立一个**虚拟 DOM** 来追踪自己要如何改变真实 DOM。请仔细看这行代码：

```
return createElement('h1', this.blogTitle)
```

`createElement` 到底会返回什么呢？其实不是一个*实际的* DOM 元素。它更准确的名字可能是 `createNodeDescription`，因为它所包含的信息会告诉 Vue 页面上需要渲染什么样的节点，包括及其子节点的描述信息。我们把这样的节点描述为“虚拟节点 (virtual node)”，也常简写它为“**VNode**”。“虚拟 DOM”是我们对由 Vue 组件树建立起来的整个 VNode 树的称呼。

```javascript
// @returns {VNode}
createElement(
  // {String | Object | Function}
  // 一个 HTML 标签名、组件选项对象，或者
  // resolve 了上述任何一种的一个 async 函数。必填项。
  'div',

  // {Object}
  // 一个与模板中属性对应的数据对象。可选。
  {
    // (详情见下一节)
  },

  // {String | Array}
  // 子级虚拟节点 (VNodes)，由 `createElement()` 构建而成，
  // 也可以使用字符串来生成“文本虚拟节点”。可选。
  [
    '先写一些文字',
    createElement('h1', '一则头条'),
    createElement(MyComponent, {
      props: {
        someProp: 'foobar'
      }
    })
  ]
)
```

渲染函数里的v-model

jsx

# 虚拟dom详解（转）

#### 一、真实DOM和其解析流程？  

​    浏览器渲染引擎工作流程都差不多，大致分为5步，**创建DOM树——创建StyleRules——创建Render树——布局Layout——绘制Painting**

​    第一步，用HTML分析器，分析HTML元素，**构建一颗DOM树**(标记化和树构建)。

​    第二步，用CSS分析器，分析CSS文件和元素上的inline样式，生成页面的样式表。

​    第三步，将DOM树和样式表，关联起来，构建一颗Render树(这一过程又称为Attachment)。每个DOM节点都有**attach方法，接受样式信息**，返回一个render对象(又名renderer)。这些render对象最终会被构建成一颗Render树。

​    第四步，有了Render树，浏览器开始布局，为每个Render树上的节点确定一个在显示屏上出现的精确坐标。

​    第五步，Render树和节点显示坐标都有了，就调用每个节点**paint方法，把它们绘制**出来。 

​    **DOM树的构建是文档加载完成开始的？**构建DOM数是一个渐进过程，为达到更好用户体验，渲染引擎会尽快将内容显示在屏幕上。**它不必**等到整个HTML文档解析完毕之后才开始构建render数和布局。

​    **Render树是DOM树和CSSOM树构建完毕才开始构建的吗？**这三个过程在实际进行的时候又不是完全独立，而是会有交叉。会造成一边加载，一遍解析，一遍渲染的工作现象。

​    **CSS的解析是从右往左逆向解析的**(从DOM树的下－上解析比上－下解析效率高)，**嵌套标签越多，解析越慢。**



![img](https:////upload-images.jianshu.io/upload_images/4345378-b7ccad3bc808783f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/624/format/webp)

webkit渲染引擎工作流程

#### 二、JS操作真实DOM的代价！

​        用我们传统的开发模式，原生JS或JQ操作DOM时，浏览器会从构建DOM树开始从头到尾执行一遍流程。在一次操作中，我需要更新10个DOM节点，浏览器收到第一个DOM请求后并不知道还有9次更新操作，因此会马上执行流程，最终执行10次。例如，第一次计算完，紧接着下一个DOM更新请求，这个节点的坐标值就变了，前一次计算为无用功。计算DOM节点坐标值等都是白白浪费的性能。即使计算机硬件一直在迭代更新，操作DOM的代价仍旧是昂贵的，频繁操作还是会出现页面卡顿，影响用户体验。

#### 三、为什么需要虚拟DOM，它有什么好处?

​        Web界面由DOM树(树的意思是数据结构)来构建，当其中一部分发生变化时，其实就是对应某个DOM节点发生了变化，

​        虚拟DOM就是为了**解决浏览器性能问题**而被设计出来的。**如前**，若一次操作中有10次更新DOM的动作，虚拟DOM不会立即操作DOM，而是将这10次更新的diff内容保存到本地一个JS对象中，最终将这个JS对象一次性attch到DOM树上，再进行后续操作，避免大量无谓的计算量。**所以，**用JS对象模拟DOM节点的好处是，页面的更新可以先全部反映在JS对象(虚拟DOM)上，操作内存中的JS对象的速度显然要更快，等更新完成后，再将最终的JS对象映射成真实的DOM，交由浏览器去绘制。

#### 四、实现虚拟DOM

​        例如一个真实的DOM节点。



![img](https:////upload-images.jianshu.io/upload_images/4345378-12f4a7f96b346deb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/620/format/webp)

真实DOM

​        我们用JS来模拟DOM节点实现虚拟DOM。



![img](https:////upload-images.jianshu.io/upload_images/4345378-9aa021f6e7dc88fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/621/format/webp)

虚拟DOM

​        其中的Element方法具体怎么实现的呢？



![img](https:////upload-images.jianshu.io/upload_images/4345378-9a6ae2a0e3a4c776.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/616/format/webp)

Element方法实现

​        第一个参数是节点名（如div），第二个参数是节点的属性（如class），第三个参数是子节点（如ul的li）。除了这三个参数会被保存在对象上外，还保存了**key和count**。其相当于形成了虚拟DOM树。



![img](https:////upload-images.jianshu.io/upload_images/4345378-1486296905180b6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/712/format/webp)

虚拟DOM树

​        有了JS对象后，最终还需要将其映射成真实DOM



![img](https:////upload-images.jianshu.io/upload_images/4345378-a7d98ba2e9fb1bdc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/616/format/webp)

虚拟DOM对象映射成真实DOM

​        我们已经完成了创建虚拟DOM并将其映射成真实DOM，这样所有的更新都可以先反应到虚拟DOM上，如何反应？需要用到**Diff算法**。

​        两棵树如果完全比较时间复杂度是O(n^3)，但参照《深入浅出React和Redux》一书中的介绍，React的Diff算法的时间复杂度是O(n)。要实现这么低的时间复杂度，意味着只能平层的比较两棵树的节点，放弃了深度遍历。这样做，似乎牺牲掉了一定的精确性来换取速度，但考虑到现实中前端页面通常也不会跨层移动DOM元素，这样做是最优的。

#### 深度优先遍历，记录差异

​        。。。。

#### Diff操作

​        在实际代码中，会对新旧两棵树进行一个深度的遍历，每个节点都会有一个标记。每遍历到一个节点就把该节点和新的树进行对比，如果有差异就记录到一个对象中。

​        下面我们创建一棵新树，用于和之前的树进行比较，来看看Diff算法是怎么操作的。



![img](https:////upload-images.jianshu.io/upload_images/4345378-58f905e4f9049b2c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/621/format/webp)

old Tree



![img](https:////upload-images.jianshu.io/upload_images/4345378-e3aeff752058cd96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/620/format/webp)

new Tree

​        平层Diff，只有以下4种情况：

​        1、**节点类型变了**，例如下图中的P变成了H3。我们将这个过程称之为**REPLACE**。直接将旧节点卸载并装载新节点。旧节点包括下面的子节点都将被卸载，如果新节点和旧节点仅仅是类型不同，但下面的所有子节点都一样时，这样做效率不高。但为了避免O(n^3)的时间复杂度，这样是值得的。这也提醒了开发者，应该避免无谓的节点类型的变化，例如运行时将div变成p没有意义。

​        2、**节点类型一样，仅仅属性或属性值变了。**我们将这个过程称之为**PROPS**。此时不会触发节点卸载和装载，而是节点更新。



![img](https:////upload-images.jianshu.io/upload_images/4345378-6b717c34619c54b2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/620/format/webp)

查找不同属性方法

​        3、**文本变了**，文本对也是一个Text Node，也比较简单，直接修改文字内容就行了，我们将这个过程称之为**TEXT**。

​        4、移动／增加／删除 子节点，我们将这个过程称之为**REORDER**。看一个例子，在A、B、C、D、E五个节点的B和C中的BC两个节点中间加入一个F节点。



![img](https:////upload-images.jianshu.io/upload_images/4345378-4515ca8e797224a0.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700/format/webp)

例子

​        我们**简单粗暴的做法**是遍历每一个新虚拟DOM的节点，与旧虚拟DOM对比相应节点对比，在旧DOM中是否存在，不同就卸载原来的按上新的。这样会对F后边每一个节点进行操作。卸载C，装载F，卸载D，装载C，卸载E，装载D，装载E。**效率太低。**



![img](https:////upload-images.jianshu.io/upload_images/4345378-eb3f73c67d3ef57e.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700/format/webp)

粗暴做法

​        如果我们在JSX里为数组或枚举型元素增加上key后，它能够根据key，直接找到具体位置进行操作，效率比较高。常见的**最小编辑距离问题**，可以用Levenshtein Distance算法来实现，时间复杂度是O(M*N)，但通常我们只要一些简单的移动就能满足需要，降低精确性，将时间复杂度降低到O(max(M,N))即可。



![img](https:////upload-images.jianshu.io/upload_images/4345378-ec9c6737f4e4f1da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/622/format/webp)

最终Diff出来的结果

#### 映射成真实DOM

​        虚拟DOM有了，Diff也有了，现在就可以将Diff应用到真实DOM上了。深度遍历DOM将Diff的内容更新进去。



![img](https:////upload-images.jianshu.io/upload_images/4345378-73e7e6db57d95032.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/617/format/webp)

根据Diff更新DOM



![img](https:////upload-images.jianshu.io/upload_images/4345378-424038129a2f06f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/617/format/webp)

根据Diff更新DOM

我们会有两个虚拟DOM(js对象，new/old进行比较diff)，用户交互我们操作数据变化new虚拟DOM，old虚拟DOM会映射成**实际DOM(**js对象生成的DOM文档)通过**DOM fragment**操作给浏览器渲染。当修改new虚拟DOM，会把newDOM和oldDOM通过diff算法比较，得出diff结果数据表(用4种变换情况表示)。再把diff结果表通过**DOM** **fragment**更新到**浏览器DOM**中。

虚拟DOM的存在的意义？vdom 的真正意义是为了实现跨平台，服务端渲染，以及提供一个性能还算不错 Dom 更新策略。vdom 让整个 mvvm 框架灵活了起来

Diff算法只是为了虚拟DOM比较替换效率更高，通过Diff算法得到diff算法结果数据表(需要进行哪些操作记录表)。原本要操作的DOM在vue这边还是要操作的，只不过用到了js的**DOM** **fragment**来操作dom（统一计算出所有变化后统一更新一次DOM）进行浏览器DOM一次性更新。其实**DOM** **fragment**我们不用平时发开也能用，但是这样程序员写业务代码就用把DOM操作放到fragment里，这就是框架的价值，程序员才能专注于写业务代码**。**

# VUE过滤器

Vue.js 允许你自定义过滤器，可被用于一些常见的文本格式化。过滤器可以用在两个地方：**双花括号插值和 v-bind 表达式** (后者从 2.1.0+ 开始支持)。过滤器应该被添加在 JavaScript 表达式的尾部，由“**管道”符号**指示：

```html
<!-- 在双花括号中 -->
{{ message | capitalize }}

<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | formatId"></div>
```

# webpack学习

```javascript
var path = require("path");
//导入插件
var ExtractTextPlugin = require('extract-text-webpack-plugin');

var config = {
    // 入口
    entry: { 
        main: './main.js'
    },
    // 出口
    output: {
        path: path.join(__dirname ,"./dist"),  //输出的路径
        publicPath: '/dist/',  //资源文件引用的目录
        filename: 'main.js'    //输出文件的名称
    },
    //模块配置
    module: {
        rules: [  //指定loader，每一个lodaer必须包含test和use两个选项
            {
                test: /\.css$/,
                use: ExtractTextPlugin.extract({
                    use: 'css-loader',
                    fallback: 'style-loader'
                })
            }
        ]
    },
    //插件配置
    plugins: [
        //重命名提取后的css文件
        new ExtractTextPlugin("main.css")
    ],
};

module.exports = config;
```

入口

出口

加载器

vue-loader

vue-style-loader

babel-loader

...

插件



**箭头函数体内的this对象就是定义时所在的对象，而不是使用时所在的对象**



**对象字面量可以缩写，当对象的key和value名称一致时，可以缩写成一个**



# SPA单页面应用部署过程：

SPA只有一个html文件，一般将该html'挂在后端程序下，由后端路由渲染这个页面，将所有的静态资源（css,js,image,iconfont等）单独部署到CDN，当然也可以和后端程序部署在一起，实现前后端完全分离

# VUE插件

通过`install`注册插件，通过`Vue.use()`使用插件

vue的核心插件vue-router、vuex

## vue-router路由

什么是路由？通俗的讲就是网址，专业的讲：每次GET或POST等请求在服务端会有一个专门的正则配置列表，然后匹配到具体的一条路径后，分发到不同的Controller，进行操作，最终将html或数据返回给前端，完成了一次IO

前端路由

优点：页面持久性，如音乐网站切换音乐不会中断；前后端完全分离

缺点：需要加载css和js

后端路由

优点：seo优化好，不需要等待前端加载

缺点：单独由后端维护，前端要修改模板需安装整套后端服务，前后端混杂，不易于分离