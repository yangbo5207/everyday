#### 如何理解javascript中call 和 apply 方法

call 和 apply 都是为了改变某个函数运行时的 context 即上下文而存在的，换句话说，就是为了改变函数体内部 this 的指向。因为 JavaScript 的函数存在「定义时上下文」和「运行时上下文」以及「上下文是可以改变的」这样的概念。

二者的作用完全一样，只是接受参数的方式不太一样

call和apply的应用包括如下场景

引用别人的方法
```js
//domList 为一个 nodeList 对象, 类似数组但是不能使用数组的方法
var domList = document.getElementsByTagName('li');

// 将nodeList转换为数组
Array().prototype.slice.call(domList);
```
