
## 基于原型的继承

### 什么是基于原型的继承
先看一段代码：

```js
    const Person = function (name,age) {
        this.name = name
        this.age = age
    } //1

    Person.prototype.getNaem = function () {
        return this.name
    }//2

    Person.prototype.getAge = function () {
        return this.age
    }//3

    const wangergou = new Person('wang',24)//4
    console.log(wangergou)//5
    console.log(wangergou.getNaem(), wangergou.getAge());//6
    // wang 24

```

解释一下执行的细节：
* 执行1，创建了一个构造函数`Persion`，要注意，此时`Persion.prototype`已经被自动创建，它包含`constructor`和`__proto__`这两个属性
* 执行2，给对象`Persion.prototype`增加了一个`getName()`方法
* 执行3，给对象`Persion.prototype`增加了一个`getAge()`方法
* 执行4，由构造函数`Persion`创建了一个实例`wangergou`，值得注意的是，一个构造函数在实例化时，一定会自动执行该构造函数
* 在浏览器得到5的输出，即`wangergou`应该是：

```js
{
    name:'wangergou',
    age:24,
    __proto__:Object //实际上就是 Persion.prototype
}
```
所以以下等式成立：
`wangergou.__proto__ === Persion.prototype`
* 执行6的时候，由于在`wangergou`中找不到`getName()`和`getAge()`这两个方法，就会继续沿着原型链向上查找，也就是通过`__proto__`向上查找，很快就在`wangergou.__proto__`中，即`Persion.prototype`中找到了这两个方法，于是停止查找并执行得到结果

这便是JS的原型链继承，准确的说，JS的原型链继承是通过`__proto__`并借助`prototype`实现的。

于是，我们可以得出下面的结论：
1. 函数对象的`__proto__`指向`Function.prototype`
2. 函数对象的`prototype`指向`instance.__proto__`
3. 普通对象的`__proto__`指向`Object.prototype`
4. 普通对象没有`prototype`属性
5. 在访问一个对象的属性或者方法时，如果在当前对象上找不到，则会尝试访问`obj.__proto__`，也就是访问该对象的构造函数的原型`obCtr.prototype`，如果还找不到，则会继续向上查找`obCtr.prototype.__proto__`，依次查找下去。如果在某一时刻找到了该属性则立刻返回值并停止对原型链的搜索，如果找不到则返回`undefined`

我们来画一个原型链图：
![-w667](media/15785806310606/15787471922367.jpg)


### 原型链

上一节，实际还存在一个问题，就是，原型链如果一直搜索下去，如果找不到，那何时会停止呢？也就是说，原型链的尽头是在哪里？

我们可以快速的利用下面的代码进行验证：

```js
function Persion() {}

const wangergou = new Persion()

console.log(wangergou.name);
```

很显然，上面的代码会输出 `undefinde`，下面简述查找过程：

```js
ulivz                // 是一个对象，可以继续 
ulivz['name']           // 不存在，继续查找 
ulivz.__proto__            // 是一个对象，可以继续
ulivz.__proto__['name']        // 不存在，继续查找
ulivz.__proto__.__proto__          // 是一个对象，可以继续
ulivz.__proto__.__proto__['name']     // 不存在, 继续查找
ulivz.__proto__.__proto__.__proto__       // null !!!! 停止查找，返回 undefined
```

js中的6大内置（函数）对象的原型继承图：
![-w665](media/15785806310606/15787474194192.jpg)
由此可见：
1. 任何内置函数对象（类）本身的`__proto__`都指向`Function`的原型对象
2. 除`Object`的原型对象的`_proto__`指向`null`，其他所有内置函数对象的原型对象的`__proto__`都指向`object`

### 总结

通过上面的分析，下面我们做一个总结：
1. 若A通过`new`创建了B，则`B.__proto__ === A.prototype`
2. `__proto__`是原型链查找的起点
3. 执行`B.a`如果在B中找不到a，则会在`B.__proto__`中，也就是`A.prototype`中查找，如果里面仍然没有，则会继续向上查找，最终一定会找到`Object.prototype`，如果还找不到，因为`Object.prototype.__proto__`指向null，因此会返回`undefined`
4. 为什么万物皆空？因为，原型链的顶端，一定会有`Object.prototype.__proto__ === null`

## 基于类的继承

直接用基于类的继承重写上述代码：

```js
class Persion{
    constructor(name,age){
        this.name = name
        this.age = age
    }

    getName(){
        return this.name
    }

     getAge(){
            return this.age
      }

 }

let wangergou = new Persion('wang',24)
```



















