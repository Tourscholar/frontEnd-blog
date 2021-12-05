### Cookie和Session
会话跟踪是Web程序中常用的技术,  而`HTTP`协议是无状态的, 确定用户身份就需要跟踪用户的整个会话. 最常用的会话跟踪是使用`Cookie`与`Session`, 简单来说`Cookie`通过在客户端记录信息确定用户身份, `Session`通过在服务端记录信息确定用户身份

#### Cookie
HTTP Cookie(也叫Web Cookie或浏览器Cookie), 是服务器发送到用户浏览器并保存在本地的一小块数据, 它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上. 通常, 它用于告知服务端两个请求是否来自同一浏览器, 如保持用户的登录状态. Cookie使基于无状态的HTTP协议记录稳定的状态信息成为了可能
Cookie主要用于以下三个方面:
  - 会话状态管理(如用户登录状态, 购物车, 游戏分数或其他需要记录的信息)
  - 个性化设置(如用户自定义设置, 主题等)
  - 浏览器行为跟踪(如跟踪分析用户行为等)

#### Session
Session代表着服务器和客户端一次会话的过程. Session对象存储特定用户会话所需的属性及配置信息. 这样, 当用户在应用程序的Web页之间跳转时, 存储在Session对象中的变量将不会丢失, 而是在整个用户会话中一直存在下去, 当客户端关闭会话或者Session超时失效时会话结束

#### Cookie和Session的不同
  1. 作用范围不同, Cookie保存在客户端(浏览器), Session保存在服务器端
  2. 存取方式的不同, Cookie只能保存ASCII, Session可以保存任意数据类型, 一般情况下我们可以在Session中保持一些常用变量信息, 比如说UserID等
  3. 有效期不同, Cookie可设置为长时间保持, 比如我们经常使用的默认登录功能, Session一般失效时间较短, 客户端关闭或者Session超时都会失效
  4. 隐私策略不同, Cookie存储在客户端中, 比较容易遭到不法获取, 早期有人将用户的登陆名和密码存储在Cookie中导致信息被窃取; Session存储在服务端, 安全性相对Cookie要好一些
  5. 存储大小不同, 单个Cookie保存的数据不能超过4K, Session可存储数据远高于Cookie

#### Cookie和Session的配合使用
仅使用Cookie
  - 仅使用Cookie而不使用Session进行用户身份跟踪, 服务端将所有的用户数据信息告知浏览器, 浏览器进行存储, 每次请求将数据发送到服务端. 此种方式理论上可行, 但是相对不安全, 尤其是在用户数据未加密的情况下, 若是被中间人攻击则用户的数据信息将全部被泄露, 或者用户自身将浏览器数据进行修改进行请求伪造, 伪造他人身份访问服务器等, 此外不同浏览器对于同一域Cookie的大小(一般为`4KB`左右)与数量都有限制, 将用户数据都存储在Cookie可能会有空间或数量不够的情况

仅使用Session 
  - 仅使用Session而不使用Cookie进行用户身份跟踪, 由于使用Session在客户端仅需要一个`SessionID`传输到服务端就能进行会话跟踪, 所以实现比较简单, 可以通过对所有的URL后拼接一个`SessionID`或者对于每个表单设置一个隐藏的input用以存储`SessionID`进行提交, 服务器就可以进行会话跟踪, 由于数据存储在服务端, 相对较安全, 且存储量大小完全取决于服务端, 可以较好控制

现在常采用Cookie和Session结合使用的方式, 用户在第一次请求服务器的时候, 服务器根据用户提交的相关信息, 创建对应的Session, 请求返回时将此Session的唯一标识信息`SessionID`返回给浏览器, 浏览器接收到服务器返回的`Session`信息后, 会将此信息存入到Cookie中, 同时Cookie记录此`SessionID`属于哪个域名
当用户第二次访问服务器的时候, 请求会自动判断此域名下是否存在Cookie信息, 如果存在则自动将Cookie信息也发送给服务端, 服务端会从Cookie中获取`SessionID`, 再根据`SessionID`查找相应的Session信息, 如果没有找到说明用户没有登录或者登录失效, 如果找到Session证明用户已经登录可执行后面操作
这样既不需要在Cookie中存储大量信息, 也不需要对每次请求都需要操作附带`SessionID`, `SessionID`是连接Cookie和Session的一道桥梁

#### 浏览器的同源策略和跨域请求
同源策略/SOP(Same origin policy)是一种约定, 由Netscape公司1995年引入浏览器, 它是浏览器最核心也是最基本的安全功能, 如果缺少了同源策略, 浏览器很容易受到XSS, CSFD等攻击, 同源指的是三个相同:
  1. 协议相同
  2. 域名相同
  3. 端口相同

由于同源政策规定, Ajax请求只能发给同源的网址, 否则就报错, 除了架设服务器代理(浏览器请求同源服务器, 再由后者请求外部服务), 有三种方法规避这个限制
  - JSONP
  - WebSocket
  - CORS

CORS跨域资源共享, 阮一峰老师的 [<跨资源共享CORS详解>](https://www.ruanyifeng.com/blog/2016/04/cors.html)讲解得很详细

JSONP是通过动态`<script>`元素来使用的, 使用时可以为src属性指定一个跨域URL. 这里的`<script>`元素与`<img>`元素类似, 都有能力不受限制地从其他域加载资源. 因为JSONP是有效的JavaScript代码, 所以在请求完成后, 即在JSONP响应加载到页面中以后, 就会立即执行
```javascript
let addScriptTag = (src) => {
  let script = document.createElement('script');
  script.setAttribute("type", "text/javascript");
  script.src = src;
  document.body.appendChild(script);
};

window.onload = () => {
  addScriptTag("http://example.com/ip?callback=getData");
};

let getData = (data) => {
  console.log("Your public IP address is: " + data.ip);
};
```
服务器在收到这个请求以后, 会将数据放在回调函数的参数位置返回, 由于`<script>`元素请求的脚本直接作为代码运行. 这时只要浏览器定义了`getData`函数, 该函数就会立即调用. 作为参数的JSON数据被视为JavaScript对象, 而不是字符串, 因此避免了使用`JSON.parse`的步骤