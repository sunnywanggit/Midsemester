


### 事件流

要理解DOM相关事件，我们先要理解“事件流”这个概念，事件流描述的是从页面中接收事件的顺序。

事件冒泡：事件开始由最具体的元素接收，然后逐级向上传播到较为不具体的节点或文档。

事件捕获：事件开始由不太具体的节点接收，然后逐级向下传播到最具体的节点。它与事件冒泡是个相反的过程。

DOM2 级事件规定的事件流包括三个阶段：事件捕获、目标阶段、事件冒泡。

### 事件委托

事件委托，通俗的说就是将元素的事件委托给它的父级或者更外级的元素处理，它的实现机制就是事件冒泡。

假设有一个列表，要求点击列表项弹出对应的字段：


```html
<ul id="myLink">
  <li id="1">aaa</li>
  <li id="2">bbb</li>
  <li id="3">ccc</li>
</ul>
```

**不使用事件委托**

```js
var myLink = document.getElementById('myLink');
var li = myLink.getElementsByTagName('li');

for(var i = 0; i < li.length; i++) {
  li[i].onclick = function(e) {
    var e = event || window.event;
    var target = e.target || e.srcElement;
    alert(e.target.id + ':' + e.target.innerText);  
  };
}
```

存在问题：
* 给每一个列表都绑定事件，消耗内存
* 当有动态添加的元素时，需要重新给元素绑定事件

**使用事件委托**

```js
var myLink = document.getElementById('myLink');

myLink.onclick = function(e) {
  var e = event || window.event;
  var target = e.target || e.srcElement;
  if(e.target.nodeName.toLowerCase() == 'li') {
    alert(e.target.id + ':' + e.target.innerText);
  }
};
```

**事件委托的优点**

* 只需要将同类元素的事件委托给父级或者更外级的元素，不需要给所有的元素都绑定事件，减少内存占用空间，提升性能。
* 动态新增的元素无需重新绑定事件

**需要注意的点**

* 事件委托的实现依靠的冒泡，因此不支持事件冒泡的事件就不适合使用事件委托。
* 不是所有的事件绑定都适合使用事件委托，不恰当使用反而可能导致不需要绑定事件的元素也被绑定上了事件。

### 阻止默认事件

**阻止默认事件的三种方式**

* e.preventDefault() //谷歌及ie8以上
* window.event.returnValue = false //ie8及以下
* return false //无兼容问题，但不能用于节点直接 onclick 绑定函数


```js
<a id="xxx" href="https://fanyi.baidu.com">阻止默认事件</a>

    xxx.onclick = function (e) {
        if (e && e.preventDefault()) {
            e.preventDefault()
        } else {
            window.event.returnValue = false
        }

        return false // 或者使用此方法

    }
```

**使用return false 的注意点**


```js
<a href="www.baidu.com" onclick="defaultEvent()">阻止默认事件</a>

function defaultEvent(){
    return false;
}
```

以上代码完全无效，无法通过这个函数阻止a标签的跳转，return false 不能适用于直接用onclick绑定的事件。

### 阻止事件冒泡


```js
function bubbles(e){
  var ev = e || window.event;
  if(ev && ev.stopPropagation) {
    //非IE浏览器
    ev.stopPropagation();
  } else {
    //IE浏览器(IE11以下)
    ev.cancelBubble = true;
  }
}
```







































