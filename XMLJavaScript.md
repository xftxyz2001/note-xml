## XML HTTP Request

### XMLHttpRequest 对象
XMLHttpRequest 对象用于在后台与服务器交换数据。
XMLHttpRequest 对象是开发者的梦想，因为您能够：
- 在不重新加载页面的情况下更新网页
- 在页面已加载后从服务器请求数据
- 在页面已加载后从服务器接收数据
- 在后台向服务器发送数据

若要了解更多XML HTTP Request知识，[此处](https://www.runoob.com/dom/dom-tutorial.html)

### XMLHttpRequest 实例
![1669637489856](image/XMLJavaScript/1669637489856.png)

### 创建一个 XMLHttpRequest 对象
所有现代浏览器（IE7+、Firefox、Chrome、Safari 和 Opera）都有内建的 XMLHttpRequest 对象。
- 创建 XMLHttpRequest 对象的语法：
  ```js
  xmlhttp=new XMLHttpRequest();
  ```
- 旧版本的Internet Explorer（IE5和IE6）中使用 ActiveX 对象：
  ```js
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  ```


## XML 解析器
所有现代浏览器都有内建的 XML 解析器。
XML 解析器把 XML 文档转换为 XML DOM(文档对象模型) 对象 - 可通过 JavaScript 操作的对象。

### 解析 XML 文档
下面的代码片段把 XML 文档解析到 XML DOM 对象中：
```js
if (window.XMLHttpRequest) {// code for IE7+, Firefox, Chrome, Opera, Safari
  xmlhttp = new XMLHttpRequest();
}
else {// code for IE6, IE5
  xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
}
xmlhttp.open("GET", "books.xml", false);
xmlhttp.send();
xmlDoc = xmlhttp.responseXML;
```

### 解析 XML 字符串
下面的代码片段把 XML 字符串解析到 XML DOM 对象中：
```js
txt = "<bookstore><book>";
txt = txt + "<title>Everyday Italian</title>";
txt = txt + "<author>Giada De Laurentiis</author>";
txt = txt + "<year>2005</year>";
txt = txt + "</book></bookstore>";

if (window.DOMParser) {
  parser = new DOMParser();
  xmlDoc = parser.parseFromString(txt, "text/xml");
}
else // Internet Explorer
{
  xmlDoc = new ActiveXObject("Microsoft.XMLDOM");
  xmlDoc.async = false;
  xmlDoc.loadXML(txt);
}
```

> Internet Explorer 使用 loadXML() 方法来解析 XML 字符串，而其他浏览器使用 DOMParser 对象。

### 跨域访问
出于安全方面的原因，现代的浏览器不允许跨域的访问。
这意味着，网页以及它试图加载的 XML 文件，都必须位于相同的服务器上。


## XML DOM
DOM（Document Object Model 文档对象模型）定义了访问和操作文档的标准方法。

### XML DOM
XML DOM（XML Document Object Model）定义了访问和操作 XML 文档的标准方法。
XML DOM 把 XML 文档作为树结构来查看。
所有元素可以通过 DOM 树来访问。可以修改或删除它们的内容，并创建新的元素。元素，它们的文本，以及它们的属性，都被认为是节点。

### HTML DOM
HTML DOM 定义了访问和操作 HTML 文档的标准方法。
所有 HTML 元素可以通过 HTML DOM 来访问。

### 实例
```html
<h1>W3Schools Internal Note</h1>
<div>
  <b>To:</b> <span id="to"></span><br />
  <b>From:</b> <span id="from"></span><br />
  <b>Message:</b> <span id="message"></span>
</div>
```

用到的XML：
```xml
<note>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note>
```

从XML文件中读取
```js
if (window.XMLHttpRequest) {// code for IE7+, Firefox, Chrome, Opera, Safari
  xmlhttp = new XMLHttpRequest();
}
else {// code for IE6, IE5
  xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
}
xmlhttp.open("GET", "note.xml", false);
xmlhttp.send();
xmlDoc = xmlhttp.responseXML;
```

从字符串中读取
```js
if (window.DOMParser) {
  parser = new DOMParser();
  xmlDoc = parser.parseFromString(txt, "text/xml");
}
else // Internet Explorer
{
  xmlDoc = new ActiveXObject("Microsoft.XMLDOM");
  xmlDoc.async = false;
  xmlDoc.loadXML(txt);
}
```

展示
```js
document.getElementById("to").innerHTML =
    xmlDoc.getElementsByTagName("to")[0].childNodes[0].nodeValue;
document.getElementById("from").innerHTML =
    xmlDoc.getElementsByTagName("from")[0].childNodes[0].nodeValue;
document.getElementById("message").innerHTML =
    xmlDoc.getElementsByTagName("body")[0].childNodes[0].nodeValue;
```

> `getElementsByTagName("to")[0].childNodes[0].nodeValue`
> 请注意，即使 XML 文件只包含一个 `<to>` 元素，您仍然必须指定数组索引 `[0]`。
> 这是因为 getElementsByTagName() 方法返回一个数组。


## XML/HTML
在 HTML 页面中显示 XML 数据

```js
if (window.XMLHttpRequest) {// code for IE7+, Firefox, Chrome, Opera, Safari
  xmlhttp = new XMLHttpRequest();
}
else {// code for IE6, IE5
  xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
}
xmlhttp.open("GET", "cd_catalog.xml", false);
xmlhttp.send();
xmlDoc = xmlhttp.responseXML;

document.write("<table border='1'>");
var x = xmlDoc.getElementsByTagName("CD");
for (i = 0; i < x.length; i++) {
  document.write("<tr><td>");
  document.write(x[i].getElementsByTagName("ARTIST")[0].childNodes[0].nodeValue);
  document.write("</td><td>");
  document.write(x[i].getElementsByTagName("TITLE")[0].childNodes[0].nodeValue);
  document.write("</td></tr>");
}
document.write("</table>");
```


## XML 应用程序
```js
x=xmlDoc.getElementsByTagName("CD");
i=0;

function displayCD() //从第一个 CD 元素中获取 XML 数据，然后在 id="showCD" 的 HTML 元素中显示数据。displayCD() 函数在页面加载时调用
{
  artist=(x[i].getElementsByTagName("ARTIST")[0].childNodes[0].nodeValue);
  title=(x[i].getElementsByTagName("TITLE")[0].childNodes[0].nodeValue);
  year=(x[i].getElementsByTagName("YEAR")[0].childNodes[0].nodeValue);
  txt="Artist: " + artist + "<br />Title: " + title + "<br />Year: "+ year;
  document.getElementById("showCD").innerHTML=txt;
}
```

```js
// 添加导航（功能），创建 next() 和 previous() 两个函数
function next() { // 显示下一张CD，除非您在最后一张CD上
  if (i < x.length - 1) {
    i++;
    displayCD();
  }
}

function previous() { // 显示上一张CD，除非您在第一张CD上
  if (i > 0) {
    i--;
    displayCD();
  }
}
```
