---
layout:     post

title:      "Vue 组件（上）"

date:       2017-11-14 20:02:00

author:     "Shinya"

tags:
    - 前端
    - Vue
    - 学习
    - 组件
---

> 和React一样，Vue也提供了组件化的页面开发方式，组件化让前端的代码有清晰树状结构，也方便了代码的复用


#### 创建组件

```js
new Vue({
el: '#element'
})
``` 
#### 组件注册
```js
Vue.component('tag-name', {
    options,
    template: '<span>element</span>'
})
```

NOTE: 组件的注册必须再创建根实例之前。

#### 局部注册

```js
new Vue({
    components: {
        'tag-name': {
            template: '<span>child</span>'
        } 
    }
})
```
这样的封装也适用于directive
#### DOM模板解析
一些HTML元素的子元素会有限制，（比如`<table>`内限制有`<tr>`），使用自定义的元素会出现问题，解决办法是用`is`特性：
```html
<table>
    <tr is="my-rpw"></tr>
</table>
```
使用以下的字符串
模板没有限制：
1. `<script type="text/x-template">`
2. JavaScript的inline模板
3. `.vue`组件

官方建议尽量使用字符串模板

#### `data`必须是函数
注册组件的时候传入。

根本目的是为了初始化多个实例的时候，她们使用的`data`是不同的对象的引用

#### 组件的组合
父组件通过`props`向子组件传递数据

子组件通过`$emit()`向父组件传递实践

#### `props`传递数据
子组件注册时，显示用`props`声明需要接收的参数，例如：

```js
Vue.component('child', {
    props: ['msg'],
    template: '<span>{{msg}}</span>'
})
```
使用方法：

```html
<child msg="hello"></child>
```

#### camelCase（驼峰命名）和kebab-case（烤肉串命名）

原因是HTML不区分大小写，Vue在处理`非字符串模板`时，自动转为kebab风格
```js
Vue.component('ele', {
    props: ['myMsg'],
    template: '<span>{{msg}}</span>'
})
```
使用的时候:
```html
<ele my-msg='hello'></ele>
```
#### 动态props
使用`v-bind`指令来动态绑定数据
支持解构：
```js
todo: {
    text: 'learn',
    isDone: false
}

<todo-item v-bind="todo"><todo-item>
```
```js
<todo-item 
    v-bind:text="todo.text"
    v-bind:is-done="todo.isDone"
></todo-item>
```

注意区分：常量字符串传递和动态props绑定
```js
<comp some-prop="1"></comp>
```
```js
<comp v-bind:some-prop="1"></comp>
```

#### 单向数据流
1.通过传递data，作为子组件的初始值
2.通过传递原始数据，子组件使用computed转化数据

#### prop验证
```js
Vue.component('example', {
  props: {
    // basic type check (`null` means accept any type)
    propA: Number,
    // multiple possible types
    propB: [String, Number],
    // a required string
    propC: {
      type: String,
      required: true
    },
    // a number with default value
    propD: {
      type: Number,
      default: 100
    },
    // object/array defaults should be returned from a
    // factory function
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    // custom validator function
    propF: {
      validator: function (value) {
        return value > 10
      }
    }
  }
})
```
#### Non-prop 属性

```js
<bs-date-input data-3d-date-picker="true"></bs-date-input>
```

属性的合并

子组件：
```js
<input type="date" class="form-control">
```
父组件：
```js
<bs-date-input
  data-3d-date-picker="true"
  class="date-picker-theme-dark"
></bs-date-input>
```

class属性将会自动合并

####  自定义事件：

使用```v-on```和```$emit```来监听和发送事件

#### 绑定本地事件

```js
<my-component v-on:click.native="doTheThing"></my-component>
```

#### ```.sync``` 修饰
需要“双向绑定”的时候使用：
```js
<comp :foo.sync="bar"></comp>
```

```js
<comp :foo="bar" @update:foo="val => bar = val">
</comp>
```

需要改变数据的时候：
```js
    this.$emit('update:foo', newValue)
```

#### 使用自定义事件的表单输入栏

```js
<input v-model="something">
```
这是一个语法糖
```js
<input
  v-bind:value="something"
  v-on:input="something = $event.target.value">
```

#### 自定义v-model组件

```js
Vue.component('my-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean,
    // this allows using the `value` prop for a different purpose
    value: String
  },
  // ...
})
```

```js
<my-checkbox v-model="foo" value="some value"></my-checkbox>
```

等同于：
```js
<my-checkbox
  :checked="foo"
  @change="val => { foo = val }"
  value="some value">
</my-checkbox>
```

#### 事件总线
这提供非父子组件之间的通信：

```js
var bus = new Vue()
// in component A's method
bus.$emit('id-selected', 1)
// in component B's created hook
bus.$on('id-selected', function (id) {
  // ...
})
```

#### 使用```Slot```进行内容分发

如果一个slot都没有内容，父组件将会被丢弃
`<slot>`里的原始内容被称为`回退内容`，他在子组件内编译，并且该组件是空的才展示

`<slot>`可以带有`name`属性。