## jquery源码解析

#### jquery对象
```
// jQuery的写法
var $jQuery = function(selector, context) {
  //JQ对象根本就是init函数的实例对象，而init则是jQuery原型上的一个对象，它本身是没有什么方法的，全靠从jQuery原型上拿
  return new $jQuery.fn.init(selector, context);
}
$jQuery.fn = $jQuery.prototype = {
  init: function() {
    this.name = 'test'
    return this;
  },
  constructor: $jQuery,
  sayName : function(){
  	console.log('ccc');
  }
}
//为了调用$jQuery.fn = $jQuery.prototype里面的方法
$jQuery.fn.init.prototype = $jQuery.fn;
var $a = $jQuery();
console.log('$jQuery的调用')
console.log($a);
console.log($jQuery('').sayName())
```
#### 链式操作
实质是就是返回对象自己，往jquery里面塞操作或者属性方法
```
$jQuery.fn.setName = function(myName) {
  this.myName = myName
  return this;
}

$jQuery.fn.getName = function() {
  $("#test").html(this.myName)
  return this;
}
```
#### 加扩展(继承)
```
$jQuery.extend = $jQuery.fn.extend = function() {
  var options, src, copy,
    target = arguments[0] || {},
    i = 1,
    length = arguments.length;

  //只有一个参数，就是对jQuery自身的扩展处理
  //extend,fn.extend
  if (i === length) {
    target = this; //调用的上下文对象jQuery/或者实例
    i--;
  }
  for (; i < length; i++) {
    //从i开始取参数,不为空开始遍历
    if ((options = arguments[i]) != null) {
      for (name in options) {
        copy = options[name];
        //覆盖拷贝
        target[name] = copy;
      }
    }
  }
  return target;
}
```

