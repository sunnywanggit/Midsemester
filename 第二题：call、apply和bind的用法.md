# 菜鸟学call、apply 和 bind
### 共同点
改变函数执行时的上下文，具体的说就是改变函数运行时的 this 指向。

### 不同点
从一个例子说起：

```js
let obj = {name:'wang'}

function Child(name) {
    this.name = name
}

Child.prototype = {
    constructor:Child,
    showName:function () {
        console.log(this.name);
    }
}


let child = new Child('zhen')
child.showName() //zhen

child.showName.call(obj)
child.showName.apply(obj)
let bind = child.showName.bind(obj)
bind() //wang

```

* call、apply 和 bind 的区别

> call  和 apply 改变了函数的上下文后，便立即执行函数，而 bind 则返回了改变上下文之后的一个函数。

* call 和 apply 的区别

> 他们之间的区别在于参数的区别，call 和 apply 的第一个参数都是要改变的上下文的对象，而 call 从第二个参数开始以参数列表的形式展现，apply 则是把除了改变上下文对象的参数放在一个数组里面作为它的第二个参数。


```js
// 求数组中的最值
let arr1 = [1,2,3,4]

console.log(Math.max.call(null, 1, 2, 3, 4));

console.log(Math.max.apply(null, arr1));
```


```js
function fn() {
    console.log(this);
}
// apply方法结果同下
fn.call(); // 普通模式下this是window，在严格模式下this是undefined
fn.call(null); // 普通模式下this是window，在严格模式下this是null
fn.call(undefined); // 普通模式下this是window，在严格模式下this是undefined
```

### 如何应用
* 将数组转化为伪数组（含有length属性的对象，dom节点，函数参数）


```js
<div class="div1">1</div>
<div class="div1">2</div>
<div class="div1">3</div>

let div = document.getElementsByTagName('div');
console.log(div); // 里面包含length属性

let arr2 = Array.prototype.slice.call(div);
console.log(arr2); // 数组 [div.div1, div.div1, div.div1]
```

**注意：**该方法在IE6-8会报错，这个时候我们可以使用遍历添加的方法


```js
function listToArray(likeAry) {
    var ary = [];
    try {
        ary = Array.prototype.slice.call(likeAry);
    } catch (e) {
        for (var i = 0; i < likeAry.length; i++) {
            ary[ary.length] = likeAry[i];
        }
    }
    return ary;
}
```

* 数组拼接

```js
let arr1 = [1,2]
let arr2 = [3,4]

Array.prototype.push.apply(arr1,arr2)

console.log(arr1);//(4) [1, 2, 3, 4]
```

* 判断变量类型

```js
let arr1 = [1,2,3];
let str1 = 'string';
let obj1 = {name: 'thomas'};
//
function isArray(obj) {
  return Object.prototype.toString.call(obj) === '[object Array]';
}
console.log(fn1(arr1)); // true

//  判断类型的方式，这个最常用语判断array和object，null(因为typeof null等于object)  
console.log(Object.prototype.toString.call(arr1)); // [object Array]
console.log(Object.prototype.toString.call(str1)); // [object String]
console.log(Object.prototype.toString.call(obj1)); // [object Object]
console.log(Object.prototype.toString.call(null)); // [object Null]

```

* 利用call和apply做继承

```js
function Animal(name){      
    this.name = name;      
    this.showName = function(){      
        console.log(this.name);      
    }      
}      

function Cat(name){    
    Animal.call(this, name);    
}      

// Animal.call(this) 的意思就是使用this对象代替Animal对象，那么
// Cat中不就有Animal的所有属性和方法了吗，Cat对象就能够直接调用Animal的方法以及属性了
var cat = new Cat("TONY");     
cat.showName();   //TONY

```
* 多继承


```js
  function Class1(a,b) {
    this.showclass1 = function(a,b) {
      console.log(`class1: ${a},${b}`);
    }
  }

  function Class2(a,b) {
    this.showclass2 = function(a,b) {
      console.log(`class2: ${a},${b}`);
    }
  }

  function Class3(a,b,c) {
    Class1.call(this);
    Class2.call(this);
  }

  let arr10 = [2,2];
  let demo = new Class3();
  demo.showclass1.call(this,1); // class1: 1,undefined
  demo.showclass1.call(this,1,2); // class1: 1,1
  demo.showclass2.apply(this,arr10); // class2: 1,2

```

















