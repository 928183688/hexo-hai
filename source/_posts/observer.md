---
title: 观察者模式
cover: https://upload.dianshangdanao.com/202001101225058296.jpg
---
## 描述
Mutation Observer（变动观察器）是监视DOM变动的接口。当DOM对象树发生任何变动时，Mutation Observer会得到通知。

要概念上，它很接近事件。可以理解为，当DOM发生变动会触发Mutation Observer事件。但是，它与事件有一个本质不同：事件是同步触发，也就是说DOM发生变动立刻会触发相应的事件；Mutation Observer则是异步触发，DOM发生变动以后，并不会马上触发，而是要等到当前所有DOM操作都结束后才触发。
```html
MutationObserver()
创建并返回一个新的 MutationObserver 它会在指定的DOM发生变化时被调用。
```
```js
function callback(mutationList, observer) {
  mutationList.forEach((mutation) => {
    switch(mutation.type) {
      case 'childList':
        /* 从树上添加或移除一个或更多的子节点；参见 mutation.addedNodes 与
           mutation.removedNodes */
        break;
      case 'attributes':
        /* mutation.target 中某节点的一个属性值被更改；该属性名称在 mutation.attributeName 中，
           该属性之前的值为 mutation.oldValue */
        break;
    }
  });
}

var targetNode = document.querySelector("#someElement");
var observerOptions = {
  childList: true, // 观察目标子节点的变化，添加或者删除
  attributes: true, // 观察属性变动
  subtree: true // 默认为 false，设置为 true 可以观察后代节点
}

var observer = new MutationObserver(callback);
observer.observe(targetNode, observerOptions);
```

使用 ID “someElement” 来获取目标节点树。 observerOptions 中设定了观察者的选项，通过设定 childList 和 attributes 为 true  来获取所需信息。
当 observer 实例化时，指定 callback() 函数。之后指定目标节点与记录选项，我们开始观察使用 observe() 指定的 DOM 节点。
从现在开始直到调用 disconnect() ，每次以 targetNode 为根节点的 DOM 树添加或移除元素时，以及这些元素的任意属性改变时，callback() 都会被调用。

