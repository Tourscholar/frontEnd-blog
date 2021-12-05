###  HTML中的DOCTYPE声明
HTML文档类型声明, 也称为```DOCTYPE```, 是每个HTML或XHTML文档所需的第一行代码. 该```DOCTYPE```声明是对We浏览器的一个指令, 说明该页面是用哪个版本的HTML编写的. 这确保了不同的Web浏览器以相同的方式解析该网页

在HTML4.01中, ```DOCTYPE```声明指的是文档类型定义(DTD). DTD定义了XML文档的结构和合法元素. 因为HTML4.01基于标准通用标记语言(SGML), 所以```DOCTYPE```必须在声明中引入DTD

另外, 对于HTML文档类型4.01所需的任一声明```strict```, ```transitional```和```frameset```DTD, 每一个具有不同的用例概述如下:
  - ```Strict DTD```: 要求严格的DTD, 不能使用任何表现层面的标识和属性, 用于排除W3C希望随着CSS支持的增长而逐步淘汰的属性和元素的网页
  - ```Transitional DTD```: 要求非常宽松的DTD, 它允许继续使用HTML4.01的标识(但是要符合XHTML的写法), 用于包含W3C希望随着CSS支持的增长而逐步淘汰的属性和元素的网页
  - ```Frameset DTD```: 专门针对框架页面设计的DTD, 用于带有框架的网页

相比之下, HTML5的声明```DOCTYPE```要简单得多: HTML5不再需要对DTD的引用, 因为它不再基于SGML

#### 例子
HTML5及更高版本的```DOCTYPE```语法
    ```html
    <!DOCTYPE html>
    ```
严格HTML4.01的```DOCTYPE```语法

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN""http://www.w3.org/TR/html4/strict.dtd">
```

过渡HTML4.01的```DOCTYPE```语法
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 
4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

#### 历史
在HTML的形成时期, Web标准尚未达成一致, 浏览器供应商会以他们想要的任何方式构建新功能, 但Web开发人员必须选择一种浏览器来开发他们的网站, 这意味着网站在不受支持的浏览器中无法很好地呈现. 于是WC3(万维网联盟)编写了一套网络标准来处理这种情况. 所有浏览器供应商和Web开发人员都应遵守这些标准以确保网站能够在浏览器中呈现良好的效果

但是标准所要求的变化与一些现有的做法大不相同, 遵守它们会破坏现有的不符合标准的网站. 为了解决这个问题, 供应商开始在他们的浏览器中编写渲染模式. Web开发人员需要在HTML文档的顶部添加一个DOCTYPE声明. DOCTYPE声明会告诉浏览器该文档使用哪种渲染模式

三种独立的渲染模式通常可以跨浏览器使用
  - ```Full standards mode```完全标准模式: 根据W3C Web标准呈现页面
  - ```Quirks mode```怪异模式: 指为了维护对较旧的网页设计的向后兼容性, 以不符合标准的方式呈现页面
  - ```Almost standards mode```几乎标准模式: 接近于完全标准模式, 但在功能上支持少量的怪异模式

在现代HTML5时代, 所有主要浏览器都完全实现了Web标准. 网站通常以符合标准的方式开发, 因此HTML5 DOCTYPE声明仅用于告诉浏览器以完全标准模式呈现文档