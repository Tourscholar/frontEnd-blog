### JavaScript选择器
JS选择器常用的有```getElementById()```, ```getElementsByClassName()```, ```getElementsByName()```, ```getElementsByTagName()```, ```querySelector()```, ```querySelectorAll()```

#### getElementById
通过```id```来定位, 返回指定```id```的第一个对象的引用, 返回类型为```HTMLDivELement```
```html
<div id="t1">T1</div>

<script type="text/javascript">
  let t1 = document.getElementById("t1");
  console.log(t1);								// <div id="t1">D1</div>
  console.log(Object.prototype.toString.call(t1));	// [object HTMLDivElement]
</script>
```

#### getElementsByClassName
通过```class```属性来定位, 返回文档中指定```class```属性值的元素的引用, 返回类型为```HTMLCollection```
```html
<div class="t2">D2</div>
<div class="t2">D3</div>

<script type="text/javascript">
  let t2List = document.getElementsByClassName("t2");
  console.log(t2List);						// HTMLCollection(2) [div.t2, div.t2]
  // 使用for循环遍历
  for (let i = 0, n = t2List.length; i < n;) console.log(t2List[i]);
  // HTMLCollection的prototype中没有forEach方法, 遍历需要使用Array的prototype中forEach通过call绑定对象实例并传参
  Array.prototype.forEach.call(t2List, v => console.log(v));
  // HTMLCollection的prototype中没有map方法, 也需要使用Array的prototype中forEach通过call绑定对象实例并传参
  Array.prototype.map.call(t2List, v => console.log(v));
</script>
```

#### 通过```name```属性来定位, 返回文档中指定```name```属性值的元素的引用, 返回类型为```NodeList```
```html
<div name="t3">D4</div>
<div name="t3">D5</div>

<script type="text/javascript">
  let t3List = document.getElementsByName("t3");
  console.log(t3List);						// NodeList(2) [div, div]
  t3List.forEach(v => console.log(v));
  // NodeList的prototype中没有map方法, 使用map的场景也需要借助Array的prototype中map通过call绑定对象实例并传参
  Array.prototype.map.call(t3List, v => console.log(v));
</script>
```

#### getElementsByTagName
通过标签的名字来定位, 返回文档中指定标签的元素的引用, 返回类型为```HTMLCollection```
```html
<p class="t4">P6</p>
<p class="t4">P7</p>

<script type="text/javascript">
  let t4List = document.getElementsByTagName("p");
  console.log(t4List);					// HTMLCollection(2) [p, p]
  Array.prototype.forEach.call(t4List, function(v) {console.log(v)});
  Arrat.prototype.map.call(t4List, function(v) {console.log(v)});
</script>
```

#### querySelector
通过```CSS```选择器来定位, 返回文档中匹配指定```CSS```选择器的所有元素的引用, 返回类型为```NodeList```
```html
<div>
  <div class="t5">D8</div>
</div>

<script type="text/javascript">
  let t5 = document.querySelector("div .t5");
  console.log(t5);							// <div class="t5">D8</div>
  console.log(Object.prototype.toString.call(t5));
  // [object HTMLDivElement]
</script>
```

#### querySelectorAll
通过```CSS```选择器来定位, 返回文档中匹配指定```CSS```选择器的所有元素的引用, 返回类型为```NodeList```
```html
<div>
  <div id="t6">D9</div>
  <div>D10</div>
  <div>D11</div>
</div>

<script type="text/javascript">
  let t6List = document.querySelectorAll("#t6 ~ div");
  console.log(t6List);					// NodeList(2) [div, div]
  t6List.forEach(function(v) {console.log(v)});
  Array.prototype.map.call(t6List, function(v) {console.log(v)});
</script>
```