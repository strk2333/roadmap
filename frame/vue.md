# Vue
## 1. 简介

MVVM模式

- View - ViewModel - Model 双向绑定
- ViewModel
  - DomListeners 监视页面上DOM元素的变化，有变化则更改Modal中的数据
  - DataBindings 更新Model中的数据时，更新DOM元素

```
// 双向绑定
new Vue({
	el: '#app',
	data: exampleData,
})

<input type="text" v-model="message"/>
```



### 1.1 指令
以v开头，作用于HTML元素



#### 1.1.1 v-if
执行元素的插入或者删除

```
v-if="isFirst" 
v-if="age > 20"

var vm = new Vue({
	el: '#app',
	data: {
		isFirst: true,
		age: 28,
		name: 'keepfool'
	}
})
```



#### 1.1.2 v-show
同v-if，不同在于使用css的display: none来隐藏元素



#### 1.1.3 v-else
必须跟在v-if或v-show的后面
```
<h1 v-if="age >= 25">Age: {{ age }}</h1>
		<h1 v-else>Name: {{ name }}</h1>
```



#### 1.1.4 v-for
```
<tr v-for="person in people">
	<td>{{ person.name  }}</td>
	<td>{{ person.age  }}</td>
	<td>{{ person.sex  }}</td>
</tr>

v-for="n in pageCount" // pageCount为整数，n范围为[0, pageCount - 1]
```



#### 1.1.5 v-bind
可缩写为 :
```
v-bind:argument="expression"

<a href="javascripit:void(0)" v-bind:class="activeNumber === n + 1 ? 'active' : ''">{{ n + 1 }}</a>
```



#### 1.1.6 v-on
监听DOM事件
可缩写为 @

```
两种调用方法：
1. 绑定方法
<button v-on:click="greet">Greet</button>
2. 内联语句
<button v-on:click="say('Hi')">Hi</button>
```



### 1.2 组件

#### 1.2.1 创建和注册

1. 调用Vue.extends()方法创建组件构造器

   template属性定义渲染的HTML

2. 调用Vue.components()方法注册组件

   两个参数: 组件标签名，组件构造器

3. 在Vue实例的作用范围内使用组件

```
<div id="app">
	<!-- 3. #app是Vue实例挂载的元素，应该在挂载元素范围内使用组件-->
	<my-component></my-component>
</div>

<script>
	// 1.创建一个组件构造器
	var myComponent = Vue.extend({
		template: '<div>This is my first component!</div>'
	})
	
	// 2.注册组件，并指定组件的标签，组件的HTML标签为<my-component>
	Vue.component('my-component', myComponent)
	
	new Vue({
		el: '#app'
	});
</script>
```


注册语法糖
```
Vue.component('my-component1',{
	template: '<div>This is the first component!</div>'
})
```



#### 1.2.2 全局注册和局部注册
- 全局注册
调用Vue.component()注册组件是全局的，可以在任意Vue实例下使用

- 局部注册
用选项对象的components属性实现局部注册



#### 1.2.3 父组件和子组件
子组件只能在父组件的**template**中使用。

常见错误：
- 直接在父组件中使用子标签

- 在父组件标签外使用子组件



#### 1.2.4 script, template标签
```
// type指定为text/x-template，告诉浏览器这不是js脚本，浏览器在解析HTML文档时会忽略script标签的内容
<script type="text/x-template" id="myComponent">
	<div>This is a component!</div>
</script>

Vue.component('my-component',{
	template: '#myComponent'
})

// 如果使用<template>标签，则不需要指定type属性。
<template id="myComponent">
	<div>This is a component!</div>
</template>
```

#### 1.2.5 组件的el和data选项
在定义组件的选项时，data和el选项必须使用函数



### 1.3 props
组件实例的作用域是孤立的，使用 props 传递数据

```
var vm = new Vue({
	el: '#app',
	data: {
		name: 'keepfool',
		age: 28
	},
	components: {
		'my-component': {
			template: '#myComponent',
			props: ['myName', 'myAge'], // camelCase
			// props验证
			props: {
				data: Array,
				columns: Array,
				filterKey: String
			},
		}
	}
})

// kebab-case
<div id="app">
	<my-component v-bind:my-name="name" v-bind:my-age="age"></my-component>
</div>

<child-component v-bind:子组件prop="父组件数据属性"></child-component>
```



#### 1.3.1 props绑定类型
```
// 单向绑定 父组件影响子组件
<tr>
	<td>my name</td>
	<td>{{ myName }}</td>
	<td><input type="text" v-model="myName" /></td>
</tr>

// 双向绑定 父组件影响子组件 子组件影响父组件
<my-component v-bind:my-name.sync="name" v-bind:my-age.sync="age"></my-component>

// 单次绑定 父组件不影响子组件
<my-component v-bind:my-name.once="name" v-bind:my-age.once="age"></my-component>
```



## 2. 路由

### 2.1 vue-router基础

<https://router.vuejs.org/zh/>

效仿H5, window.history

```html
<router-link to="/bar">Go to Bar</router-link>
```

#### 2.1.1 动态路由匹配

```
routes: [
  // 动态路径参数 以冒号开头
  { path: '/user/:id', component: User }
]

<div>User {{ $route.params.id }}</div>
```

| 模式                          | 匹配路径            | $route.params                        |
| ----------------------------- | ------------------- | ------------------------------------ |
| /user/:username               | /user/evan          | { username: 'evan' }                 |
| /user/:username/post/:post_id | /user/evan/post/123 | { username: 'evan', post_id: '123' } |



- 响应路由参数的变化
当使用路由参数时，例如从 /user/foo 导航到 /user/bar，原来的组件实例会被复用
复用组件时，想对路由参数的变化作出响应，可以: 
  - watch
  - beforeRouteUpdate 导航守卫

```
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // 对路由变化作出响应...
    }
  },
  beforeRouteUpdate (to, from, next) {
    // react to route changes...
    // don't forget to call next()
  },
}
```



- 捕获所有路由或404

```js
{
  // 会匹配所有路径
  path: '*'
}
{
  // 会匹配以 `/user-` 开头的任意路径
  path: '/user-*'
}
```

- 含有*通配符*的路由应该放在最后
- 当使用一个*通配符*时，`$route.params` 内会自动添加一个名为 `pathMatch` 参数。它包含了 URL 通过*通配符*被匹配的部分：$route.params.pathMatch

```
// params are denoted with a colon ":"
{ path: '/params/:foo/:bar' },

// a param can be made optional by adding "?"
{ path: '/optional-params/:foo?' },

// a param can be followed by a regex pattern in parens
// this route will only be matched if :id is all numbers
{ path: '/params-with-regex/:id(\\d+)' },

// asterisk can match anything
{ path: '/asterisk/*' },

// make part of the path optional by wrapping with parens and add "?"
{ path: '/optional-group/(foo/)?bar' }
```

- 匹配优先级
谁先定义的，谁的优先级就最高。

#### 2.1.2 嵌套路由

```js
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User,
      children: [
        // 当 /user/:id 匹配成功，
        // UserHome 会被渲染在 User 的 <router-view> 中
        { path: '', component: UserHome },
        {
          // 当 /user/:id/profile 匹配成功，
          // UserProfile 会被渲染在 User 的 <router-view> 中
          path: 'profile',
          component: UserProfile
        },
        {
          // 当 /user/:id/posts 匹配成功
          // UserPosts 会被渲染在 User 的 <router-view> 中
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})
```



#### 2.1.3 编程式导航
- router.push(location, onComplete?, onAbort?)
```
// 在 Vue 实例内部，你可以通过 $router 访问路由实例。因此你可以调用 this.$router.push

// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
// 如果提供了 path，params 会被忽略
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})

// 取而代之的是下面例子的做法，你需要提供路由的 name 或手写完整的带有参数的 path
const userId = '123'
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user

// 同样的规则也适用于 router-link 组件的 to 属性。

// 注意：如果目的地和当前路由相同，只有参数发生了改变 (比如从一个用户资料到另一个 /users/1 -> /users/2)，你需要使用 beforeRouteUpdate 来响应这个变化 (比如抓取用户信息)。
```



- router.replace(location, onComplete?, onAbort?)

不会向 history 添加新记录



- router.go(n)

在 history 记录中向前或者后退多少步，类似 `window.history.go(n)`



#### 2.1.4 命名路由

配置路由的name，可以在导航时使用name，不加path



#### 2.1.5 命名视图

需求：同时 (同级) 展示多个视图

```
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>

const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```



#### 2.1.6 重定向和别名

```
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' },
    { path: '/a', redirect: { name: 'foo' }},
    { path: '/a', component: A, alias: '/b' },
  ]
})
```



#### 2.1.7 路由组件传参

在组件中使用 $route 会使之与其对应路由形成高度耦合，从而使组件只能在某些特定的 URL 上使用，限制了其灵活性，使用 props 将组件和路由解耦。

```
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>' // { this.$router.params.id }
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})
```



- 布尔模式
  - 如果 props 被设置为 true，route.params 将会被设置为组件属性。
- 对象模式
  - 如果 props 是一个对象，它会被按原样设置为组件属性。当 props 是静态的时候有用。
- 函数模式
  - 你可以创建一个函数返回 props。这样你便可以将参数转换成另一种类型，将静态值与基于路由的值结合等等。



#### 2.1.8 HTML5 History模式



### 2.2 vue-router进阶

#### 2.2.1 导航守卫



##### 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用离开守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 用创建好的实例调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数。



#### 2.2.2 路由元信息

定义路由的时候可以配置 `meta` 字段

一个路由匹配到的所有路由记录会暴露为 `$route` 对象 (还有在导航守卫中的路由对象) 的 `$route.matched` 数组。因此，我们需要遍历 `$route.matched` 来检查路由记录中的 meta 字段。





#### 2.2.3 过渡动效

`<router-view>` 是基本的动态组件，可以用 `<transition>` 组件给它添加一些过渡效果：

```
<transition name="slide"> // fade
  <router-view></router-view>
</transition>
```



#### 2.2.4 数据获取

- 导航完成后获取数据





- 导航完成前获取数据





#### 2.2.5 滚动行为







#### 2.2.6 路由懒加载







## 3. 状态管理







- more

vue-property-decorator



