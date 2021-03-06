# ajax 和 xml

##  数据传输

### 请求数据

有五种常用技术用于向服务器请求数据：
- xmlHttpRequest (XHR)
- 动态脚本标签插入
- iframes
- Comet
- Multipart XHR

现在常用的是 XHR、动态脚本标签插入、Multipart XHR，其余的往往是极限情况使用。

目前最常用的方法是 XHR 。

#### XHR
当使用XHR 请求数据时，你可以选择POST 或GET。如果请求不改变服务器状态只是取回数据（又称作幂等动作）则使用GET。GET 请求被缓冲起来，如果你多次提取相同的数据可提高性能。只有当URL 和参数的长度超过了2'048 个字符时才使用POST 提取数据。因为Internet Explorer 限制URL的长度，过长将导致请求（参数）被截断。

#### 动态脚本标签插入
该技术克服了XHR 的最大限制：它可以从不同域的服务器上获取数据。这是一种黑客技术，而不是实例化一个专用对象，你用JavaScript 创建了一个新脚本标签，并将它的源属性设置为一个指向不同域的URL。

例子：
```
var scriptElement = document.createElement('script');
scriptElement.src = 'http://any-domain.com/javascript/lib.js';
document.getElementsByTagName_r('head')[0].appendChild(scriptElement);
```

但是动态脚本标签插入与XHR 相比只提供更少的控制。你不能通过请求发送信息头。参数只能通过GET方法传递，不能用POST。你不能设置请求的超时或重试，实际上，你不需要知道它是否失败了。你必须等待所有数据返回之后才可以访问它们。你不能访问响应信息头或者像访问字符串那样访问整个响应报文。

最后一点非常重要。因为响应报文被用作脚本标签的源码，它必须是可执行的JavaScript。你不能使用裸XML，或者裸JSON，任何数据，无论什么格式，必须在一个回调函数之中被组装起来。

#### Multipart XHR

多部分XHR（MXHR）允许你只用一个HTTP 请求就可以从服务器端获取多个资源。它通过将资源（可以是CSS 文件，HTML 片段，JavaScript 代码，或base64 编码的图片）打包成一个由特定分隔符界定的大字符串，从服务器端发送到客户端。JavaScript 代码处理此长字符串，根据它的媒体类型和其他“信息头”解析出每个资源。

使用此技术有一些缺点，其中最大的缺点是以此方法获得的资源不能被浏览器缓存。如果你使用MXHR获取一个特定的CSS 文件然后在下一个页面中正常加载它，它不在缓存中。因为整批资源是作为一个长字符串传输的，然后由JavaScript 代码分割。由于没有办法用程序将文件放入浏览器缓存中，所以用这种方法获取的资源也无法存放在那里。

## 数据格式

- XML 标准 速度慢
- JSON 
- html

## 总结

- 高性能Ajax 包括：知道你项目的具体需求，选择正确的数据格式和与之相配的传输技术
- 作为数据格式，纯文本和HTML 是高度限制的，但它们可节省客户端的CPU 周期。XML 被广泛应用普遍支持，但它非常冗长且解析缓慢。JSON 是轻量级的，解析迅速（作为本地代码而不是字符串），交互性与XML 相当。字符分隔的自定义格式非常轻量，在大量数据集解析时速度最快，但需要编写额外的
程序在服务器端构造格式，并在客户端解析
- 减少请求数量，可通过JavaScript 和CSS 文件打包，或者使用MXHR
- 缩短页面的加载时间，在页面其它内容加载之后，使用Ajax 获取少量重要文件
- 确保代码错误不要直接显示给用户，并在服务器端处理错误
