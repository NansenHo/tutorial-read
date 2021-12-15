### 计算属性

基本用法：

```javascript
<p> {{ c }} </p>

data(){
	return {
		a: 2,
		b: 3
	}
},
computed: {
	c: function(){
		return this.a * this.b
	}
}
```

以上代码注意点：

1. computed 里面不能使用箭头函数；
   因为箭头函数绑定了父级作用域的上下文，所以 this 将不会按照期望指向 Vue 。
2. c 不需要在 data 中声明，computed 里直接声明，并且通过计算得到 c 的值；
3. c 可以直接像 data 里的数据一样在 template 和 script 里使用；
4. 计算属性的结果会被缓存，除非 a 和 b 这两个数据改变了，否则 c 不会重新计算，会直接用缓存的结果。

计算属性和方法分别的使用场景：

```
<p> {{ c() }} </p>

data(){
	return {
		a: 2,
		b: 3
	}
},
methods: {
	c: function(){
		return this.a * this.b
	}
}
```

用 methods 也能实现和 computed 一样的效果。不过 methods 不会对计算的结果做缓存，我们每次使用到 `c()` 都会重新执行函数；

但是做缓存也会有问题，比如以下代码：

```javascript
computed: {
	now: function(){
		return Date()
	}
}
```

`Date()` 不是响应式依赖，一旦 computed 缓存了当天的时间戳之后，计算属性将不再更新。这当然不是我们想要的。

所以，**性能消耗大的计算 **推荐使用计算属性；**没有缓存且不可避免要多次计算的数据** 推荐使用函数。



