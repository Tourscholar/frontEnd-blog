### HTML节点操作
```HTML```节点的基本操作: 添加节点, 替换节点, 删除节点, 绑定事件, 访问子节点, 访问父节点, 访问兄弟节点
文档对象模型```Document Object Model```, 简称```DOM```, 是```W3C```组织推荐的处理可扩展标记语言```XML```的标准编程接口, 是一种与平台和语言无关的应用程序接口API

#### 添加节点
```javascript
<div id="t1"></div>

<script type="text/kavascript">
  let d1 = document.createElement("div");		// 创建一个节点
  d1.style.color = "blue";		// 设置CSS样式
  d1.setAttribute("id", "d1");		// 设置属性
  d1.innerText = "innerText";		// 写入纯文本内容
  let tn1 = document.createTextNode("CreateTextNode");
  d1.appendChild(tn1);		// 将tn1节点添加到d1节点后
  document.getElementById("t1").appendChild(d1);		// 将d1节点追加到t1节点后
  
  let b1 = document.createElement("div");
  b1.innerText = "添加到d1前"
  document.getElementById("t1").insertBefore(b1, document.getElementById("d1"));		// 将b1节点添加到t1节点内的d1节点前
```

#### 替换节点
```javascript
<div id="t2">
  <div>被替换的节点</div>
</div>

<script type="text/javascript">
  let d2 = document.createElement("div");
  d2.style.color = "green";
  d2.innerText = "被我替换了";
  // 第一个参数是要替换的节点, 第二个参数是被替换的节点
  document.getElementById("t2").replaceChild(d2, document.querySelector("#2 > div:first-child"));
</script>
```

#### 删除节点
```javascript
<div id="t3">
  <div>下一个div将被删除</div>
  <div>被删除</div>
</div>

<script type="text/javascript">
  document.getElementById("t3").removeChild(document.querySelector("#t3 > div:nth-child(2)"));
</script>
```

#### 绑定事件
```javascript
<div id="t4" style="color:red;">点我</div>

<script type="text/javascript">
  document.getElementById("t4").addEventListener("click", e => {
    console.log("点击事件");
  });
</script>
```

#### 访问子节点
```javascript
<div id="t5" style="color:grey;">
  <div>1</div>
  <div>2</div>
</div>

<script type="text/javascript">
  console.log(document.getElementById("t5").childNodes);	// 获取所有子节点
  console.log(document.getElementById("t5").childElementCount);		// 获取子节点数量
  console.log(document.getElementById("t5").firstChild);		// 获取第一个子节点, 会匹配#text
  console.log(document.getElementById("t5").firstElementChild);		// 获取第一个子节点
  console.log(document.getElementById("t5").lastChild);		// 获取最后一个子节点, 会匹配#text
  console.log(document.getElementById("t5").lastElementChild);		// 获取最后一个子节点
</script>
```

#### 访问父节点
```javascript
<div style="color:yellow;">
  <div id="t6">1</div>
</div>

<script type="text/javascript">
  console.log(document.getElementById("t6").parentNode);
</script>
```
#### 访问兄弟节点
```javascript
<div style="color: brown;"><div>1</div><div id="t7">2</div><div>3</div></div>

<script type="text/javascript">
  console.log(document.getElementById("t7").previousSibling);	// 访问上一个兄弟节点, 会匹配#text
  console.log(document.getElementById("t7").priviousElementSibling);	// 不匹配文本节点, 注释节点
  console.log(document.getElementById("t7").nextSibling);		// 访问下一个兄弟节点, 会匹配#text
  console.log(document.getElementById("t7").nextElementSibling);	// 不匹配文本节点, 注释节点
</script>