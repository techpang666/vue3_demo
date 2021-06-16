
# 一些vue3记录

```html
    <button @click="update">{{count}}</button>
```

```js
  setup(){
    const count = ref(1)
    console.log(count);
    function update () {
      count.value = count.value + 1
    }
    return {
      count,
      update
    }
  }
```

setup()只在初始化得时候 执行一次

如果返回的是对象 对象中的属性和方法可以直接模板使用 比如update方法 count都可以直接使用

ref()定义一个数据的响应式

js中操作数据.value

模板中不需要.value

基本类型的响应式数据(下面将是对象的响应式)

```html
    <h1>{{state.name}}</h1>
```

```js
		const state = reactive({
			name: 'tom',
			age: 25,
			wife: {
				name: 'tom',
				age: 25,
			},
		});
    console.log(state, state.wife);
		function update() {
      state.name += 'ddd'
    }
```

reactive()用于多个数据的响应式

接受一个普通对象 并返回它的响应式代理器对象

响应式转换是深层的 会影响对象内部的所有属性

内部基于es6的proxy实现的

通过代理对象操作源对象内部的数据 都是响应式的

```js
// vue3响应式原理

const user = {
	name: 'tom',
	age: 18,
};

// proxyUser代理对象 user是被代理对象 都是通过代理对象操作被代理对象内部的属性

// 通过Reflect动态对被代理对象的相应属性进行特定操作

const proxyUser = new Proxy(user, {
	// 拦截读取属性值
	get(target, prop) {
		return Reflect.get(target, prop);
	},
	// 拦截设置/添加属性值
	set(target, prop, value) {
		return Reflect.set(target, prop, value);
	},
	// 拦截删除属性
	deleteProperty(target, prop) {
		return Reflect.deleteProperty(target, prop);
	},
});
// 比较及读取属性值
console.log(proxyUser === user); /* false */
console.log(proxyUser.name, proxyUser.age); /* tom 18 */
// 设置属性值
proxyUser.name = 'cat';
proxyUser.age = 19;
console.log(user); /* { name: 'cat', age: 19 } */
// 添加属性
proxyUser.gender = 'male';
console.log(user); /* { name: 'cat', age: 19, gender: 'male' } */
// 删除属性
delete proxyUser.gender;
console.log(user); /* { name: 'cat', age: 19 } */
```










