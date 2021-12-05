####  虚拟DOM
虚拟DOM(Virtual DOM), 即虚拟DOM节点. 它通过JavaScript的Object对象模拟DOM中的节点, 然后再通过特定的	```render```方法将其渲染成真实的DOM节点


#### Virtual DOM算法
DOM操作是很慢的, 如果我们把一个简单的div元素的属性打印出来:
<img src="https://z3.ax1x.com/2021/12/01/oGRHmD.png" width="300" height="300" />
可以看到, 真正的DOM元素非常庞大, 当我们频繁的去做DOM更新, 会产生一定的性能问题. 而VirtualDOM就是用一个原生的JavaScript对象去描述一个DOM节点, 相对于DOM对象, 原生的JavaScript对象处理起来更快, 而且更简单. DOM树上的结构、属性信息我们都可以很容易地用JavaScript对象表示出来

```javascript 
let element = {
  tagName: "ul",		// 节点标签名
  props: {				// DOM的属性, 用一个对象存储键值对
    id: "list"
  },
  children: [			// 该节点的子节点
    {tagName: "li", props: {class: "item"}, children: ["Item 1"]},
    {tagName: "li", props: {class: "item"}, children: ["Item 2"]},
    {tagName: "li", props: {class: "item"}, children: ["Item 3"]},
  ]
}

// 对应的HTML写法是
<ul id="list">
  <li class="item">Item 1</li>
  <li class="item">Item 2</li>
  <li class="item">Item 3</li>
</ul>
```
原来DOM树的信息可以用JavaScript对象来表示, 反之, 可以根据这个用JavaScript对象表示的树结构来创建一颗真正的DOM树

具体来说, 所谓的Virtual DOM算法用JavaScript对象标识DOM信息和结构, 当状态变更的时候, 重新渲染这个JavaScript的对象结构, 然后用新渲染的对象树去和旧的树进行对比, 记录这两颗树的差异. 记录下来的不同就是我们需要对页面真正的DOM操作, 然后把它们应用再真正的DOM树上, 页面就变更了. 这样就可以做到: 视图的结构确实是整个全新渲染了, 但是最后操作的DOM的时候确实只变更了不同的地方
具体步骤:
  1. 用JavaScript对象结构标识DOM树的结构, 然后用这个树构建一个真正的DOM树, 插到文档中
  2. 当状态变更的时候, 重新构造一棵新的对象树, 然后用新的树和旧的树进行比较, 记录两棵树的差异
  3. 把步骤2所记录的差异应用到步骤1所构建的真正DOM树上, 更新视图

虚拟DOM本质上就是JavaScript和DOM之间做了一个缓存. 可以类比CPU和硬盘, 既然硬盘这么慢, 我们就在它们之间加个缓存: 既然DOM这么慢, 我们就在JavaScript和DOM之间加个缓存. CPU(JavaScript)只操作内存(虚拟DOM), 最后的时候再把变更写入硬盘(DOM)

#### 算法实现
1. 用JavaScript对象模拟DOM树

   用JavaScript来表示一个DOM节点是很简单的事情, 只需要记录它的节点类型, 属性和子节点
  ```javascript
  function Element (tagName, props, children) {
    this._tagName = tagName;
    this._props = props;
    this._children = children;
  }
  
  module.exports = function (tagName, props, children) {
    return new Element(tagName, props, children);
  }
  ```

  // 上面的DOM结构可以表示为
  ```javascript
  let el = require("./element");
  
  let ul = el("ul", {id: "list"}, [
    el("li", {class: "item"}, ["Item 1"]),
    el("li", {class: "item"}, ["Item 2"]),
    el("li", {class: "item"}, ["Item 3"])
  ]);
  ```
  现在ul只是一个JavaScript对象表示的DOM结构, 页面上并没有这个结构. 我们可以根据这个ul构建真正的<ul>
  ```javascript
  Element.prototype.render = () => {
    let el = document.createElement(this.tagName);		// 根据tagName构建
    let props = this._props;
    
    for (let propName in props) {					// 设置节点的DOM属性
      let propValue = props[propName];
      el.setAttribute(propName, propValue);
    }
    
    let children = this.children || [];
    
    children.forEach( () => {
      let childEl = (child instanceof Element)
        ? child.render()		// 如果子节点也是虚拟DOM, 递归构建DOM节点
        : document.createTextNode(child);		// 如果是字符串, 则只构建文本节点
      el.appendChild(childEl);
    })
  }
  ```

  render方法会根据tagName构建一个真正的DOM节点, 然后设置这个节点的属性, 最后递归地把自己的子节点也构建起来
  ```javascript
  let ulROot = ul.render();
  document.body.appendChild(ulRoot);
  ```

  UlRoot是真正的DOM节点, 把它塞入文档中, 这样body里面就有了真正的<ul>的DOM结构


2. 比较两颗虚拟DOM树的差异
    Vitrual DOM的diff算法. 两个树的完全的diff算法是一个时间复杂度为O(n^3)的问题. 但是在前端中, 很少会跨越层级地移动DOM元素, 所以Virtual DOM只会对同一个层级的元素进行对比

  在实际的代码中, 会对新旧两棵树进行一个深度优先的遍历, 这样每一个节点都会有一个唯一的标记. 在深度优先遍历的时候, 每遍历到一个节点就把该节点和新的树进行对比. 如果有差异的话就记录到一个对象里面
  ```javascript
  // diff函数, 对比两棵树
  function diff (oldTree, newTree) {
    let index = 0;
    let patche = {};
    dfsWalk(oldTree, newTree, index, patches)
    return patches;
  }
  
  // 对两棵树进行深度优先遍历
  function dfsWalk (oldNode, newNode, patches) {
    // 对比oldNode和newNode的不同, 记录下来
    patches[index] = [...];
    
    diffChildren(old.children, newNode.children, index, patches)
  }
  
  // 遍历子节点
  function diffChildren (oldChildren, newChildren, index, patches) {
    let leftNode = null;
    let currentNodeIndex = index;
    oldChildren.forEach(function (child, i) {
      let newChild = newChildren[i];
      currentNodeIndex = (leftNode && leftNode.count)	// 计算节点的标识
        ? currentNodeIndex + leftNode.count + 1
        : currentNodeIndex + 1;
      dfsWalk(child, newChild, currentNodeIndex, patches);		// 深度遍历子节点
      leftNode=  child;
    })
  }
  ```

3. 把差异应用到真正的DOM树上
  
   因为步骤一所构建的JavaScript对象树和render出来真正的DOM树的信息、结构是一样的. 所以我们可以对那棵DOM树也进行深度优先的遍历, 遍历的时候从步骤二生成的patches对象中找出前遍历的节点差异, 然后进行DOM操作
   ```javascript
   function patch (node, patches) {
     let walker = {index: 0};
     dfsWalk(node, walker, patches);
   }
   
   function dfsWalk (node, walker, patches) {
     let currentPatches = patches[walker.index];	// patches拿出当前节点的差异
     
     let len = node.childNodes;
       ? node.childNodes.length
       : 0;
       
     for (let i = 0; i < len; i++) {
       let child = node.childNodes[i];
       walker.index++;
       dfsWalk(child, walker, patches);
     }
     
     if (currentPatches) {
       applyPatches(node, currentPatches);		// 对当前节点进行DOM操作
     }
   }
   ```
   
   applyPatches, 根据不同类型的差异对当前节点进行DOM操作
   ```javascript
   function applyPatches (node, currentPatches) {
     currentPatches.forEach(function (currentPatch) {
       switch (currentPatch.type) {
         case REPLACE:
           node.parentNode.replaceChild(currentPatch.node.render(), node);
           break;
         case REORDER:
           reorderChildren(node, currentPatch.moves);
           break;
         case PROPS:
           setProps(node, currentPatch.props);
           break;
         case TEXT:
           node.textContent = currentPatch.content;
           break;
         default:
           throw new Error("Unknow patch type " + currentPatch.type)
       }
     })
   }
   ```
   
   Virtual DOM主要是实现步骤的三个函数: element, diff, patch. 然后就可以进行实际使用
   ```javascript
   // 构建虚拟DOM
   let tree = el("div", {"id": "container"}, [
     el("h1", {style: "color: blue"}, ["simple virtal dom"]),
     el("p", ["Hello, virtual-dom"]),
     el("ul", [el("li")])
   ]);
   
   // 通过虚拟DOM构建真正的DOM
   let root = tree.render();
   document.body.appendChild(root);
   
   // 生成新的虚拟DOM
   let newTree = el("div", {"id": "container"}, [
     el("h1", {style: "color: red"}, ["simple virtual dom"]),
     el("p", ["Hello, virtual-dom"]),
     el("ul", [el("li"), el("li")])
   ]);
   
   // 比较两颗虚拟DOM树的不同
   let patches = diff(tree, newTree);
   
   // 在真正的DOM元素上应用变更
   patch(root, patches);
   ```