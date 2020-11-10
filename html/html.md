## 目录

- [`Doctype`(文档声明)](#Doctype(文档声明))
- [描述`cookie`，`sessionStorage`以及`localStorage`之间的差异](#描述cookie，sessionStorage以及localStorage之间的差异)
- [描述`script`，`script` `async`和`script` `defer`之间的差别](#描述script，script-async和script-defer之间的差别)
- [为什么CSS的`<link>`和JS的`<script>`要放置在`<head></head>`之间和`</body>`之前](#为什么CSS的<link>和JS的<script>要放置在<head></head>之间和</body>之前)
- [描述`<img>`标签中的`srcset`属性](#描述<img>标签中的srcset属性)
- [html5新特性](#html5新特性)

### `Doctype`(文档声明)
1. 文档声明用来告诉浏览器当前网页的版本,它会告诉浏览器应该用什么规则集来解释文档中的标记
2. 规则是w3c所发布的一个文档类型定义（dtd）中包含的规则
3. 每个dtd都包括标记、attributes、properties等内容，它们用于标记web文档的内容；此外还包括一些规则，它们规定了哪些标记能出现在其他哪些标记中。每个web建议标准（比如html4 frameset和xhtml 1.0 transitional）都有自己的dtd。
4. XHTML1.0中有3种DTD（文档类型定义）声明可以选择：过渡的（Transitional）、严格的（Strict）和框架的（Frameset）
5. HTML 4.01 规定了三种文档类型：Strict、Transitional 以及 Frameset
6. 在HTML4.01中<!doctype>声明指向一个DTD，由于HTML4.01基于SGML，所以DTD指定了标记规则以保证浏览器正确渲染内容
7. HTML5不基于SGML，所以不用指定DTD
```
HTML5的文档声明(HTML5中不区分大小写)
<!Doctype html>
<!doctype html>
<!DOCTYPE html>

HTML4
<!-- 该 DTD 包含所有 HTML 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets） -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<!-- 该 DTD 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets） -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<!-- 该 DTD 等同于 HTML 4.01 Transitional，且允许框架集内容 -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">

XHTML
<!-- XHTML1.0 Strict:不使用允许表现性、废弃元素以及frameset。文档必须是结构良好的XML文档 -->
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<!-- XHTML1.0 Transitional:允许使用表现性、废弃元素，不允许frameset，文档必须是结构良好的XMl文档 -->
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<!-- XHTML 1.0 Frameset:允许使用表现性、废弃元素以及frameset，文档必须是结构良好的XML文档 -->
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
```

### 描述`cookie`，`sessionStorage`以及`localStorage`之间的差异

#### cookie
* cookie客户端保存用户信息的一种机制，用来记录用户的一些信息
##### cookie构成
* name:一个唯一确定的cookie名称。通常来讲cookie的名称是不区分大小写的
* value:存储在cookie中的字符串值。最好为cookie的name和value进行url编码
* domain:cookie对于哪个域是有效的。所有向该域发送的请求中都会包含这个cookie信息。这个值可以包含子域(如：http://e.baidu.com)，也可以不包含它(如：.http://baidu.com，则对于http://baidu.com的所有子域都有效)
* path: 表示这个cookie影响到的路径，浏览器跟会根据这项配置，像指定域中匹配的路径发送cookie
* expires:失效时间，表示cookie何时应该被删除的时间戳(也就是，何时应该停止向服务器发送这个cookie)。如果不设置这个时间戳，浏览器会在页面关闭时即将删除所有cookie；不过也可以自己设置删除时间。这个值是GMT时间格式。如果客户端和服务器端时间不一致，使用expires就会存在偏差。并且如果给cookie设置一个过去的时间，浏览器会立即删除该cookie
* max-age: 与expires作用相同，用来告诉浏览器此cookie多久过期（单位是秒），而不是一个固定的时间点。正常情况下，max-age的优先级高于expires
* HttpOnly: 告知浏览器不允许通过脚本document.cookie去更改这个值，同样这个值在document.cookie中也不可见。但在http请求张仍然会携带这个cookie。注意这个值虽然在脚本中不可获取，但仍然在浏览器安装目录中以文件形式存在。这项设置通常在服务器端设置
* secure: 安全标志，指定后，只有在使用SSL链接时候才能发送到服务器，如果是http链接则不会传递该信息
##### cookie优点
* 通过良好的编程，控制保存在cookie中的session对象的大小。
* 通过加密和安全传输技术，减少cookie被 破解的可能性。
* 只有在cookie中存放不敏感的数据，即使被盗取也不会有很大的损失。
* 控制cookie的生命期，使之不会永远有效。这样的话偷盗者很可能拿到的就是一个过期的cookie.
##### cookie缺点
* cookie的长度和数量的限制。每个domain最多只能有20条cookie,每个cookie长度不能超过4KB。否则会被截掉、
* 安全性问题。如果cookie被人拦掉了，那个人就可以获取到所有session信息。加密的话也不起什么作用
* 有些状态不可能保存在客户端。例如，为了防止重复提交表单，我们需要在服务端保存一个计数器。若把计数器保存到服务端，则起不到什么作用
#### sessionStorage,localStorage
* localStorage生命周期是永久,这意味着除非用户显示在浏览器提供的UL.上清除localStorage信息，否则这些信息将永远存在。存放数据大小为-般为5MB,而且它仅在客户端(即浏览器)中保存,不参与和服务器的通信
* sessionStorage仅在当前会话下有效,关闭页面或浏览器后被清除。存放数据大小为一般为5MB,而且它仅在客户端(即浏览器)中保存，不参与和服务器的通信。

### 描述`script`，`script async`和`script defer`之间的差别
* `<script>` 标签用于定义客户端脚本，比如JavaScript,在浏览器继续解析页面之前，立即读取并执行脚本
* `<script>` 元素既可包含脚本语句，也可以通过"src" 属性指向外部脚本文件
* `<script async>`脚本相对于页面的其余部分异步地执行
* `<script defer>`脚本将在页面完成解析时执行

### 为什么CSS的`<link>`和JS的`<script>`要放置在`<head></head>`之间和`</body>`之前
* 由于js的执行流程是由上到下依次执行的，将css的link标签放置在head标签之间和js的script标签放在</body>之前可以让浏览器在加载页面的时候正常显示

### 描述`<img>`标签中的`srcset`属性
* 以逗号分隔的一个或多个字符串列表表明一系列用户代理使用的可能的图像
```
<img srcset="elva-fairy-320w.jpg 320w,
             elva-fairy-480w.jpg 480w,
             elva-fairy-800w.jpg 800w"
     sizes="(max-width: 320px) 280px,
            (max-width: 480px) 440px,
            800px"
     src="elva-fairy-800w.jpg" alt="Elva dressed as a fairy">
```

### html5新特性
1. 语义标签
2. WebStorage
3. WebSocket
4. Web Worker
Web Worker可以通过加载一个脚本文件，进而创建一个独立工作的线程，在主线程之外运行
5. 三维、图形及特效特性
基于SVG、Canvas、WebGL及CSS3的3D功能，用户会惊叹于在浏览器中，所呈现的惊人视觉效果
6. 增强型表单
html5修改一些新的input输入特性，改善更好的输入控制和验证
7. 多媒体
html5提供了音频和视频文件的标准，既使用`<audio>`元素