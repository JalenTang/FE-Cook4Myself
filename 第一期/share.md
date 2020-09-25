# 分享笔记

## 我阅读 `Vue` 源码的方式

## `Vue` 中几个核心概念的源码

### 一个 Vue 到底是什么，实例化一个 Vue 的时候做了什么

Vue 是一个构造函数，一个 Vue 文件实际上就是 Vue 的一个实例
实例化一个 Vue，或者说`new Vue()`的时候，Vue 会调用`this._init()`方法做一系列的初始化操作

```javascript
initLifecycle(vm); // 初始化生命周期
initEvents(vm); // 初始化事件
initRender(vm); // 初始化渲染相关
callHook(vm, 'beforeCreate'); // 生命周期钩子函数beforeCreate
initInjections(vm); // inject相关
initState(vm); // 初始化数据，data/prop/watch/computed
initProvide(vm); // provide相关
callHook(vm, 'created'); // 生命周期钩子函数created
```

在`initRender()`中，会对实例的[$attrs](https://cn.vuejs.org/v2/api/#vm-attrs)和[$listeners](https://cn.vuejs.org/v2/api/#vm-listeners)做响应式处理，这2个属性主要是对***父作用域***中的属性和方法做

`initState()`是对实例中一系列数据相关属性的吃实话，包括`initProps`, `initData`, `initMethods`, `initComputed`, `initWatch`等等

2. 响应式数据/双向绑定

``` javascript
function defineReactive() {

}
```

`initComputed()`的逻辑：

```javascript
function initComputed() {
	
}
```

3. 虚拟 DOM 相关

## `Vue` 几个有趣的面试题

### 1.为什么用了`Object.freeze()`之后能优化 Vue 的性能

`Object.freeze()`用于冻结对象，修改对象的描述符 **_configurable_** 为 **_false_**

```javascript
/* example1 */
const obj = { a: 1 };
Reflect.getOwnPropertyDescriptor(obj, 'a');
// { value: 1, writable: true, enumerable: true, configurable: true }
Object.freeze(obj);
// { value: 1,  writable: false, enumerable: true, configurable: false }
```

Vue 中的 `defineReactive` 函数在给对象响应式的时候会判断 configurable，

```javascript
function defineReactive(obj, key, val) {
  const property = Object.getOwnPropertyDescriptor(obj, key);
  if (property && property.configurable === false) {
    return;
  }
}
```

### 2.一个 Vue 文件到底是不是

1. Vue 是怎么检测数组变化的
2. Vue 中 nextTick 的实现
3. Vue.use 做了什么事
4. keep-alive 组件的实现原理
