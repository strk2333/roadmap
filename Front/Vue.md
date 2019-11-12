# Vue

## 1.1 基础

### 1.1.1 MVVM

- View - ViewModel - Model 双向绑定
- ViewModel
  - DomListeners 监视页面上DOM元素的变化，有变化则更改Modal中的数据
  - DataBindings 更新Model中的数据时，更新DOM元素

```
// 双向绑定
new Vue({
	el: '#app',
	data: exampleData
})

<input type="text" v-model="message"/>
```



### 1.1.2 常用指令

以v开头，作用于HTML元素

- v-if指令
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

- v-show指令
同v-if，不同在于使用css的display: none来隐藏元素

- v-else指令
必须跟在v-if或v-show的后面
```
<h1 v-if="age >= 25">Age: {{ age }}</h1>
		<h1 v-else>Name: {{ name }}</h1>
```

- v-for指令
```
<tr v-for="person in people">
	<td>{{ person.name  }}</td>
	<td>{{ person.age  }}</td>
	<td>{{ person.sex  }}</td>
</tr>

v-for="n in pageCount" // pageCount为整数，n范围为[0, pageCount - 1]
```

- v-bind指令
可缩写为 :
```
v-bind:argument="expression"

<a href="javascripit:void(0)" v-bind:class="activeNumber === n + 1 ? 'active' : ''">{{ n + 1 }}</a>
```
- v-on指令
监听DOM事件
可缩写为 @
```
两种调用方法：
1. 绑定方法
<button v-on:click="greet">Greet</button>
2. 内联语句
<button v-on:click="say('Hi')">Hi</button>
```
