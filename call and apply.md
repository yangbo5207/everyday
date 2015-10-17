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
//[].slice.call(domList);
```

在继承的实现中继承父级的参数

```js

// 父级构造函数
var Person = function(name, age) {
  this.gender = ['men', 'women'];
  this.name = name;
  this.age  = age;
}

// 子类
var Student = function(name, age, high) {
  Person.call(this, name, age);
  this.high = high;
}
Student.prototype = {
  constructor: Student,
  message: function() {
    console.log('name:'+this.name+', age:'+this.age+', high:'+this.high+', gender:'+this.gender[0]);
  }
}

var xm = new Student('xiaom', '12', '150cm');
sm.message(); // name:xiaom, age:12, high:150cm, gender:men; [可以访问到从父级继承而来的属性]
```


