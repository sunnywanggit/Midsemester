
### Promise的用途？

#### promise出现的原因

在说promise的用途之前，我们先来说一下promise出现的原因，在promise出现之前，我们处理一个异步的网络请求，大概是这样子的：

```js
请求1(function(请求结果){
    处理请求结果
})
```

但是当你需要根据上一个请求的结果，再去执行第二个网络请求的时候，根据第二个请求的结果，再去执行第三个网络请求...，于是就出现了下面的代码：

```js
请求1(function(请求结果1){
    请求2(function(请求结果2){
        请求3(function(请求结果3){
            请求4(function(请求结果4){
                请求5(function(请求结果5){
                    请求6(function(请求结果3){
                        ...
                    })
                })
            })
        })
    })
})
```
这就是臭名昭著的回调地狱，更糟糕的是，我们基本上还要对每次请求的结果进行一些处理，如此一来，代码会更加臃肿。

回调地狱带来的负面作用主要有一下几点：
* 代码臃肿
* 可读性差
* 耦合度过高，可维护性差
* 代码复用性差
* 容易滋生bug
* 只能在回调中处理异常


出现了问题那总是要解决问题，于是就出现了promise

#### 什么是promise

promise 是一种异步编程的解决方案，比传统的异步解决方案【回调函数】和【事件】更加合理、更加强大。


```js
new Promise(请求1)
    .then(请求2(请求结果1))
    .then(请求3(请求结果2))
    .then(请求4(请求结果3))
    .then(请求5(请求结果4))
    .catch(处理异常(异常信息))
```

#### 如何创建一个 new Promise

第一步：
* return new Promise（(resolve,reject)=>{...}）
* 任务成功则调用 resolve(result)
* 任务失败则调用reject(error)
* resolve和reject会再去调用成功和失败函数

第二步：
* 使用.then(success,fail)传入成功和失败函数


```js
const promise = new Promise((resolve, reject) => {
   resolve('success'); 
});
promise.then(success => { 
    console.log('success'); 
    //成功时调用
}, fail => { 
    console.log('fail');
    //失败时调用
})

```

#### 如何使用 Promise.prototype.then

then() 方法返回一个 Promise。它最多需要有两个参数：Promise 的成功和失败情况的回调函数。


```js
const promise = new Promise(function(resolve, reject) {
  resolve('Success!');
});
promise.then(function(value) {
  console.log(value);
  // expected output: "Success!"
});

```
> 注意：then方法是异步执行的

#### 如何使用Promise.all

MDN给出的定义是：Promise.all(iterable) 方法返回一个 Promise 实例，此实例在 iterable 参数内所有的 promise 都“完成（resolved）”或参数中不包含 promise 时回调完成（resolve）；如果参数中  promise 有一个失败（rejected），此实例回调失败（reject），失败原因的是第一个失败 promise 的结果。

简单点说就是：只有全部为resolve才会调用 通常会用来处理 多个并行异步操作。


```js
const p1 = new Promise((resolve, reject) => {
    resolve(1);
});

const p2 = new Promise((resolve, reject) => {
    resolve(2);
});

const p3 = new Promise((resolve, reject) => {
    resolve(3);
});

Promise.all([p1, p2, p3]).then(data => { 
    console.log(data); // [1, 2, 3] 结果顺序和promise实例数组顺序是一致的
}, err => {
    console.log(err);
});
```

#### 如何使用Promise.race

Promise.race 接收一个promise对象数组为参数

Promise.race 只要有一个promise对象进入 FulFilled 或者 Rejected 状态的话，就会继续进行后面的处理。


```js
const promise1 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 500, 'one');
});

const promise2 = new Promise(function(resolve, reject) {
    setTimeout(resolve, 100, 'two');
});

Promise.race([promise1, promise2]).then(function(value) {
  console.log(value);
  // Both resolve, but promise2 is faster
});
// expected output: "two"
```
























