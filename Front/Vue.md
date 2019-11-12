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
```
v-if="isFirst" 
v-if="age > 20"

var vm = new Vue({
	el: '#app',
	data: {
		yes: true,
		no: false,
		age: 28,
		name: 'keepfool'
	}
})
```


- v-show指令
- v-else指令
- v-for指令
- v-bind指令
- v-on指令
