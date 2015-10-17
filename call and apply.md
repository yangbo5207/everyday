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

又比如：
```js
var fn = {
  name: 'jake',
  showName: function() {
    console.log(this.name);
  }
},

// foo中没有showName方法，但是他可以通过call/apply来调用fn中的showName方法
var foo = {
  name: 'nicco'
}

fn.showName.call(foo);  // nicco
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

  // 继承父级的属性
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

在封装对象过程中保证this的正确传递
```js
// 定义一个简化的jQuery插件，使用call/applay来保证this的正确传递

// 先引入一个jQuery
<script src="jQuery.js"></script>

<script>
// 先定义一个对象，其中参数 box：jQuery对象，str：字符
var Nicco = function(box, str) {
  this.$box = box;
  this.str = str;
}
Nicco.prototype = {
  constructor: Nicco,
  
  message: function() {
    this.$box.on('mousedown', this.bindThis(this.fnDown, this));
  },
  
  // 因为fn.call(obj)会立即执行，因此需要加一层function套起来，还有一种写法
  message2: function() {
    var _this = this;
    this.$box.on(
      'mousedown', 
      function() {
        return _this.fnDown.call(_this);
      } 
    )
  },
  
  fnDown: function(e) {
    this.str += ', my name is nicco';
    this.$box.on('mouseup', this.bindThis(this.fnUp, this));
  }, 
  
  fnUp: function(e) {
    console.log('message:' +this.str);
  },
  
  // 定义传递this的参数，其实质是this来调用定义好的方法，这样可以保证在on的回调函数中，this的指向保持一致
  bindThis: function(fn, obj) {
    return function() {
      return fn.apply(fn, arguments);
    }
  };
}

// 定义jQuery插件
$.fn.mess = function(str) {
  var nic = new Nicco( $(this), str );
  return nic.message();
}

$(function() {
  $('.box').mess('hello'); // hello, my name is nicco.
});

</script>
```

