## XML 命名空间
XML 命名空间提供避免元素命名冲突的方法。

### 命名冲突
在 XML 中，元素名称是由开发者定义的，当两个不同的文档使用相同的元素名时，就会发生命名冲突。

这个 XML 携带 HTML 表格的信息：
```xml
<table>
  <tr>
  <td>Apples</td>
  <td>Bananas</td>
  </tr>
</table>
```

这个 XML 文档携带有关桌子的信息（一件家具）：
```xml
<table>
  <name>African Coffee Table</name>
  <width>80</width>
  <length>120</length>
</table>
```
假如这两个 XML 文档被一起使用，由于两个文档都包含带有不同内容和定义的 `<table>` 元素，就会发生命名冲突。

XML 解析器无法确定如何处理这类冲突。

### 使用前缀来避免命名冲突
在 XML 中的命名冲突可以通过使用名称前缀从而容易地避免。

该 XML 携带某个 HTML 表格和某件家具的信息：
```xml
<h:table>
  <h:tr>
    <h:td>Apples</h:td>
    <h:td>Bananas</h:td>
  </h:tr>
</h:table>
<f:table>
  <f:name>African Coffee Table</f:name>
  <f:width>80</f:width>
  <f:length>120</f:length>
</f:table>
```
在上面的实例中，不会有冲突，因为两个 `<table>` 元素有不同的名称。

### XML 命名空间 - xmlns 属性
当在 XML 中使用前缀时，一个所谓的用于前缀的命名空间必须被定义。
命名空间是在元素的开始标签的 xmlns 属性中定义的。
命名空间声明的语法如下。xmlns:前缀="URI"。
```xml
<root>
  <h:table xmlns:h="http://www.w3.org/TR/html4/">
    <h:tr>
      <h:td>Apples</h:td>
      <h:td>Bananas</h:td>
    </h:tr>
  </h:table>
  <f:table xmlns:f="http://www.w3cschool.cc/furniture">
    <f:name>African Coffee Table</f:name>
    <f:width>80</f:width>
    <f:length>120</f:length>
  </f:table>
</root>
```

在上面的实例中，`<table>` 标签的 xmlns 属性定义了 h: 和 f: 前缀的合格命名空间。
当命名空间被定义在元素的开始标签中时，所有带有相同前缀的子元素都会与同一个命名空间相关联。
命名空间，可以在他们被使用的元素中或者在 XML 根元素中声明：
```xml
<root xmlns:h="http://www.w3.org/TR/html4/" xmlns:f="http://www.w3cschool.cc/furniture">
  <h:table>
    <h:tr>
      <h:td>Apples</h:td>
      <h:td>Bananas</h:td>
    </h:tr>
  </h:table>
  <f:table>
    <f:name>African Coffee Table</f:name>
    <f:width>80</f:width>
    <f:length>120</f:length>
  </f:table>
</root>
```

> 命名空间 URI 不会被解析器用于查找信息。
> 其目的是赋予命名空间一个惟一的名称。不过，很多公司常常会作为指针来使用命名空间指向实际存在的网页，这个网页包含关于命名空间的信息。

### 统一资源标识符（URI，全称 Uniform Resource Identifier）
统一资源标识符（URI）是一串可以标识因特网资源的字符。
最常用的 URI 是用来标识因特网域名地址的统一资源定位器（URL）。另一个不那么常用的 URI 是统一资源命名（URN）。
在我们的实例中，我们仅使用 URL。

### 默认的命名空间
为元素定义默认的命名空间可以让我们省去在所有的子元素中使用前缀的工作。它的语法如下：
```
xmlns="namespaceURI"
```

这个 XML 携带 HTML 表格的信息：
```xml
<table xmlns="http://www.w3.org/TR/html4/">
  <tr>
    <td>Apples</td>
    <td>Bananas</td>
  </tr>
</table>
```

这个XML携带有关一件家具的信息：
```xml
<table xmlns="http://www.w3schools.com/furniture">
  <name>African Coffee Table</name>
  <width>80</width>
  <length>120</length>
</table>
```

### 实际使用中的命名空间
XSLT 是一种用于把 XML 文档转换为其他格式的 XML 语言，比如 HTML。
在下面的 XSLT 文档中，您可以看到，大多数的标签是 HTML 标签。
非 HTML 的标签都有前缀 xsl，并由此命名空间标识：xmlns:xsl="http://www.w3.org/1999/XSL/Transform"：

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>

<xsl:stylesheet version="1.0"
  xmlns:xsl="http://www.w3.org/1999/XSL/Transform">

  <xsl:template match="/">
    <html>
      <body>
        <h2>My CD Collection</h2>
        <table border="1">
          <tr>
            <th align="left">Title</th>
            <th align="left">Artist</th>
          </tr>
          <xsl:for-each select="catalog/cd">
            <tr>
              <td>
                <xsl:value-of select="title" />
              </td>
              <td>
                <xsl:value-of select="artist" />
              </td>
            </tr>
          </xsl:for-each>
        </table>
      </body>
    </html>
  </xsl:template>

</xsl:stylesheet>
```


## XML CDATA
XML 文档中的所有文本均会被解析器解析。
只有 CDATA 区段中的文本会被解析器忽略。

### PCDATA - 被解析的字符数据
XML 解析器通常会解析 XML 文档中所有的文本。
当某个 XML 元素被解析时，其标签之间的文本也会被解析：
```xml
<message>This text is also parsed</message>
```

解析器之所以这么做是因为 XML 元素可包含其他元素，就像这个实例中，其中的 `<name>` 元素包含着另外的两个元素（first 和 last）：
```xml
<name><first>Bill</first><last>Gates</last></name>
```

而解析器会把它分解为像这样的子元素：
```xml
<name>
  <first>Bill</first>
  <last>Gates</last>
</name>
```

解析字符数据（PCDATA）是 XML 解析器解析的文本数据使用的一个术语。

### CDATA - （未解析）字符数据
术语 CDATA 是不应该由 XML 解析器解析的文本数据。
像 `<` 和 `&` 字符在 XML 元素中都是非法的。
`<` 会产生错误，因为解析器会把该字符解释为新元素的开始。
`&` 会产生错误，因为解析器会把该字符解释为字符实体的开始。
某些文本，比如 JavaScript 代码，包含大量 `<` 或 `&` 字符。为了避免错误，可以将脚本代码定义为 CDATA。
CDATA 部分中的所有内容都会被解析器忽略。
CDATA 部分由 `<![CDATA[" 开始，由 "]]>` 结束：
```html
<script>
<![CDATA[
function matchwo(a, b) {
  if (a < b && a < 0) {
    return 1;
  } else {
    return 0;
  }
}
]]>
</script>
```

解析器会忽略 CDATA 部分中的所有内容。
关于 CDATA 部分的注释：
- CDATA 部分不能包含字符串 `]]>`。也不允许嵌套的 CDATA 部分。
- 标记 CDATA 部分结尾的 `]]>` 不能包含空格或换行。


## XML 编码
XML 文档可以包含非 ASCII 字符，比如挪威语 æ ø å，或者法语 ê è é。
为了避免错误，需要规定 XML 编码，或者将 XML 文件存为 Unicode。

### XML 编码错误
如果您载入一个 XML 文档，您可以得到两个不同的错误，表示编码问题：

**在文本内容中发现无效字符。**
- 如果您的 XML 中包含非 ASCII 字符，且文件保存为没有指定编码的单字节 ANSI（或 ASCII），您会得到一个错误。

**将当前编码切换为不被支持的指定编码**
- 如果您的 XML 文件保存为带有指定的单字节编码（WINDOWS-1252、ISO-8859-1、UTF-8）的双字节 Unicode（或 UTF-16），您会得到一个错误。
- 如果您的 XML 文件保存为带有指定的双字节编码（UTF-16）的单字节 ANSI（或 ASCII），您也会得到一个错误。

### Windows 记事本
Windows 记事本默认会将文件保存为单字节的 ANSI（ASCII）。
如果您选择 "另存为..."，就可以指定 ANSI、UTF-8、Unicode（UTF-16）或 Unicode Big。
将下面的 XML 保存为 ANSI、UTF-8 和 Unicode（注意文档不包含任何编码属性）。
```xml
<?xml version="1.0"?>
<note>
  <from>Jani</from>
  <to>Tove</to>
  <message>Norwegian: æøå. French: êèé</message>
</note>
```
尝试将文件拖到您的浏览器，并查看结果。不同的浏览器会显示不同的结果。

### 结论
- 始终使用编码属性
- 使用支持编码的编辑器
- 确保您知道编辑器使用什么编码
- 在您的编码属性中使用相同的编码


## XML 服务器
XML 文件是类似 HTML 文件的纯文本文件。
XML 能够通过标准的 Web 服务器轻松地存储和生成。

### 在服务器上存储 XML 文件
XML 文件在 Internet 服务器上进行存储的方式与 HTML 文件完全相同。
启动 Windows 记事本，并写入以下行：
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<note>
  <from>Jani</from>
  <to>Tove</to>
  <message>Remember me this weekend</message>
</note>
```

然后用适当的文件名，比如 "note.xml"，在 Web 服务器上保存这个文件。

### 通过 ASP 生成 XML
XML 可在不安装任何 XML 软件的情况下在服务器端生成。
如需从服务器生成 XML 响应 - 只需简单地编写以下代码并在 Web 服务器上把它保存为一个 ASP 文件：
```jsp
<%
response.ContentType="text/xml"
response.Write("<?xml version='1.0' encoding='ISO-8859-1'?>")
response.Write("<note>")
response.Write("<from>Jani</from>")
response.Write("<to>Tove</to>")
response.Write("<message>Remember me this weekend</message>")
response.Write("</note>")
%>
```

> 此响应的内容类型必须设置为 "text/xml"。

### 通过 PHP 生成 XML
如需使用 PHP 从服务器上生成 XML 响应，请使用下面的代码：
```php
<?php
header("Content-type: text/xml");
echo "<?xml version='1.0' encoding='ISO-8859-1'?>";
echo "<note>";
echo "<from>Jani</from>";
echo "<to>Tove</to>";
echo "<message>Remember me this weekend</message>";
echo "</note>";
?>
```

> 响应头部的内容类型必须设置为 "text/xml"。

### 从数据库生成 XML
XML 可在不安装任何 XML 软件的情况下从数据库生成。
如需从服务器生成 XML 数据库响应，只需简单地编写以下代码，并把它在 Web 服务器上保存为 ASP 文件：
```jsp
<%
response.ContentType = "text/xml"
set conn=Server.CreateObject("ADODB.Connection")
conn.provider="Microsoft.Jet.OLEDB.4.0;"
conn.open server.mappath("/db/database.mdb")

sql="select fname,lname from tblGuestBook"
set rs=Conn.Execute(sql)

response.write("<?xml version='1.0' encoding='ISO-8859-1'?>")
response.write("<guestbook>")
while (not rs.EOF)
response.write("<guest>")
response.write("<fname>" & rs("fname") & "</fname>")
response.write("<lname>" & rs("lname") & "</lname>")
response.write("</guest>")
rs.MoveNext()
wend

rs.close()
conn.close()
response.write("</guestbook>")
%>
```

### 在服务器上通过 XSLT 转换 XML
下面的 ASP 代码在服务器上把 XML 文件转换为 XHTML：
```jsp
<%
'Load XML
set xml = Server.CreateObject("Microsoft.XMLDOM")
xml.async = false
xml.load(Server.MapPath("simple.xml"))

'Load XSL
set xsl = Server.CreateObject("Microsoft.XMLDOM")
xsl.async = false
xsl.load(Server.MapPath("simple.xsl"))

'Transform file
Response.Write(xml.transformNode(xsl))
%>
```

> - 第一个代码块创建微软 XML 解析器的实例（XMLDOM），并把 XML 文件载入内存。
> - 第二个代码块创建解析器的另一个实例，并把 XSL 文件载入内存。
> - 最后一个代码使用 XSL 文档来转换 XML 文档，并把结果以 XHTML 发送到您的浏览器。

### 通过 ASP 把 XML 保存为文件
这个 ASP 实例会创建一个简单的 XML 文档，并把该文档保存到服务器上：
```jsp
<%
text="<note>"
text=text & "<to>Tove</to>"
text=text & "<from>Jani</from>"
text=text & "<heading>Reminder</heading>"
text=text & "<body>Don't forget me this weekend!</body>"
text=text & "</note>"

set xmlDoc=Server.CreateObject("Microsoft.XMLDOM")
xmlDoc.async=false
xmlDoc.loadXML(text)

xmlDoc.Save("test.xml")
%>
```


## XML DOM 高级
在本章中我们将结合一些其他重要的 XML DOM 方法。

### 获取元素的值
下面的实例检索第一个 `<title>` 元素的文本值：
```js
txt=xmlDoc.getElementsByTagName("title")[0].childNodes[0].nodeValue;
```

### 获取属性的值
下面的实例检索第一个 `<title>` 元素的 "lang" 属性的文本值：
```js
txt=xmlDoc.getElementsByTagName("title")[0].getAttribute("lang");
```

### 改变元素的值
下面的实例改变第一个 `<title>` 元素的文本值：
```js
x=xmlDoc.getElementsByTagName("title")[0].childNodes[0];
x.nodeValue="Easy Cooking";
```

### 创建新的属性
XML DOM 的 setAttribute() 方法可用于改变现有的属性值，或创建一个新的属性。
下面的实例创建了一个新的属性（edition="first"），然后把它添加到每一个 `<book>` 元素中：
```js
x=xmlDoc.getElementsByTagName("book");
for(i=0;i<x.length;i++) {
  x[i].setAttribute("edition","first");
}
```

### 创建元素
XML DOM 的 createElement() 方法创建一个新的元素节点。
XML DOM 的 createTextNode() 方法创建一个新的文本节点。
XML DOM 的 appendChild() 方法向节点添加子节点（在最后一个子节点之后）。
如需创建带有文本内容的新元素，需要同时创建元一个新的元素节点和一个新的文本节点，然后把他追加到现有的节点。

下面的实例创建了一个新的元素（`<edition>`），带有如下文本：First，然后把它添加到第一个 `<book>` 元素：
```js
newel=xmlDoc.createElement("edition");
newtext=xmlDoc.createTextNode("First");
newel.appendChild(newtext);
x=xmlDoc.getElementsByTagName("book");
x[0].appendChild(newel);
```

> - 创建一个 `<edition>` 元素
> - 创建值为 "First" 的文本节点
> - 把这个文本节点追加到新的 `<edition>` 元素
> - 把 `<edition>` 元素追加到第一个 `<book>` 元素

### 删除元素
下面的实例删除第一个 `<book>` 元素的第一个节点：
```js
x=xmlDoc.getElementsByTagName("book")[0];
x.removeChild(x.childNodes[0]);
```


## XML 注意事项
这里列出了您在使用 XML 时应该尽量避免使用的技术。

### Internet Explorer - XML 数据岛
它是什么？
- XML 数据岛是嵌入到 HTML 页面中的 XML 数据。

为什么要避免使用它？
- XML 数据岛只在 Internet Explorer 浏览器中有效。

用什么代替它？
- 您应当在 HTML 中使用 JavaScript 和 XML DOM 来解析并显示 XML。

### XML 数据岛实例
把 XML 文档绑定到 HTML 文档中的一个 `<xml>` 标签。id 属性定义数据岛的标识符，而 src 属性指向 XML 文件：
```html
<xml id="cdcat" src="cd_catalog.xml"></xml>
<table border="1" datasrc="#cdcat">
  <tr>
    <td><span datafld="ARTIST"></span></td>
    <td><span datafld="TITLE"></span></td>
  </tr>
</table>
```

`<table>` 标签的 datasrc 属性把 HTML 表格绑定到 XML 数据岛。
`<span>` 标签允许 datafld 属性引用要显示的 XML 元素。在这个实例中，要引用的是 "ARTIST" 和 "TITLE"。当读取 XML 时，会为每个 `<CD>` 元素创建相应的表格行。

### Internet Explorer - 行为
它是什么？
- Internet Explorer 5 引入了行为。行为是通过使用 CSS 样式向 XML （或 HTML ）元素添加行为的一种方法。

为什么要避免使用它？
- 只有 Internet Explorer 支持 behavior 属性。

使用什么代替它？
- 使用 JavaScript 和 XML DOM（或 HTML DOM）来代替它。

略


## XML 技术
- XHTML (可扩展 HTML)：更严格更纯净的基于 XML 的 HTML 版本。
- XML DOM (XML 文档对象模型)：访问和操作 XML 的标准文档模型。
- XSL (可扩展样式表语言) XSL 包含三个部分：：
  - XSLT (XSL 转换) - 把 XML 转换为其他格式，比如 HTML：
  - XSL-FO (XSL 格式化对象) - 用于格式化 XML 文档的语言：
  - XPath - 用于导航 XML 文档的语言
- XQuery (XML 查询语言)：基于 XML 的用于查询 XML 数据的语言。
- DTD (文档类型定义)：用于定义 XML 文档中的合法元素的标准。
- XSD (XML 架构)：基于 XML 的 DTD 替代物。
- XLink (XML 链接语言)：在 XML 文档中创建超级链接的语言。
- XPointer (XML 指针语言)：允许 XLink 超级链接指向 XML 文档中更多具体的部分。
- SOAP (简单对象访问协议)：允许应用程序在 HTTP 之上交换信息的基于 XML 的协议。
- WSDL (Web 服务描述语言)：用于描述网络服务的基于 XML 的语言。
- RDF (资源描述框架)：用于描述网络资源的基于 XML 的语言。
- RSS (真正简易聚合)：聚合新闻以及类新闻站点内容的格式。
- SVG (可伸缩矢量图形)：定义 XML 格式的图形。


## XML 现实案例
如何使用 XML 来交换信息的一些实例。

### 实例：XML 新闻
XMLNews 是用于交换新闻和其他信息的规范。
对新闻的供求双方来说，通过使用这种标准，可以使各种类型的新闻信息通过不同软硬件以及编程语言进行的制作、接收和存档更加容易：
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<nitf>
  <head>
    <title>Colombia Earthquake</title>
  </head>
  <body>
    <headline>
      <hl1>143 Dead in Colombia Earthquake</hl1>
    </headline>
    <byline>
      <bytag>By Jared Kotler, Associated Press Writer</bytag>
    </byline>
    <dateline>
      <location>Bogota, Colombia</location>
      <date>Monday January 25 1999 7:28 ET</date>
    </dateline>
  </body>
</nitf>
```

### 实例：XML 气象服务
XML 国家气象服务案例，来自 NOAA（National Oceanic and Atmospheric Administration）：
```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<current_observation>

  <credit>NOAA's National Weather Service</credit>
  <credit_URL>http://weather.gov/</credit_URL>

  <image>
    <url>http://weather.gov/images/xml_logo.gif</url>
    <title>NOAA's National Weather Service</title>
    <link>http://weather.gov</link>
  </image>

  <location>New York/John F. Kennedy Intl Airport, NY</location>
  <station_id>KJFK</station_id>
  <latitude>40.66</latitude>
  <longitude>-73.78</longitude>
  <observation_time_rfc822>Mon, 11 Feb 2008 06:51:00 -0500 EST
</observation_time_rfc822>

  <weather>A Few Clouds</weather>
  <temp_f>11</temp_f>
  <temp_c>-12</temp_c>
  <relative_humidity>36</relative_humidity>
  <wind_dir>West</wind_dir>
  <wind_degrees>280</wind_degrees>
  <wind_mph>18.4</wind_mph>
  <wind_gust_mph>29</wind_gust_mph>
  <pressure_mb>1023.6</pressure_mb>
  <pressure_in>30.23</pressure_in>
  <dewpoint_f>-11</dewpoint_f>
  <dewpoint_c>-24</dewpoint_c>
  <windchill_f>-7</windchill_f>
  <windchill_c>-22</windchill_c>
  <visibility_mi>10.00</visibility_mi>

  <icon_url_base>http://weather.gov/weather/images/fcicons/</icon_url_base>
  <icon_url_name>nfew.jpg</icon_url_name>
  <disclaimer_url>http://weather.gov/disclaimer.html</disclaimer_url>
  <copyright_url>http://weather.gov/disclaimer.html</copyright_url>

</current_observation>
```


## XML 编辑器
如果您希望极认真地学习和使用 XML，那么您一定会从一款专业的 XML 编辑器的使用上受益。

### XML 是基于文本的
XML 是基于文本的标记语言。
关于 XML 的一件很重要的事情是，XML 可被类似记事本这样的简单的文本编辑器来创建和编辑。
不过，在您开始使用 XML 进行工作时，您很快会发现，使用一款专业的 XML 编辑器来编辑 XML 文档会更好。

### 为什么不使用记事本?
许多 Web 开发人员使用记事本来编辑 HTML 和 XML 文档，这是因为最常用的操作系统都带有记事本，而且它很容易使用。从个人来讲，我经常使用记事本来快速地编辑某些简单的 HTML、CSS 以及 XML 文件。
但是，如果您将记事本用于 XML 编辑，可能很快会发现不少问题。
记事本不能确定您编辑的文档类型，所以也就无法辅助您的工作。

### 为什么使用 XML 编辑器?
当今，XML 是非常重要的技术，并且开发项目正在使用这些基于 XML 的技术：
- 用 XML Schema 定义 XML 的结构和数据类型
- 用 XSLT 来转换 XML 数据
- 用 SOAP 来交换应用程序之间的 XML 数据
- 用 WSDL 来描述网络服务
- 用 RDF 来描述网络资源
- 用 XPath 和 XQuery 来访问 XML 数据
- 用 SMIL 来定义图形

为了能够编写出无错的 XML 文档，您需要一款智能的 XML 编辑器！

### XML 编辑器
专业的 XML 编辑器会帮助您编写无错的 XML 文档，根据某种 DTD 或者 schema 来验证 XML，以及强制您创建合法的 XML 结构。
XML 编辑器应该能够：
- 为开始标签自动添加结束标签
- 强制您编写合法的 XML
- 根据某种 DTD 来验证 XML
- 根据某种 Schema 来验证 XML
- 对您的 XML 语法进行代码的颜色化

推荐：**XMLSpy**
- 在 32 位和 64 位版本中可用
- 使用方便
- 上下文敏感的人们帮手
- 语法着色和漂亮的印刷
- 智能修复验证与自动校正错误
- 文本视图和网格视图之间轻松切换
- 图形化的 XML Schema 编辑器
- 所有主流数据库的数据库导入导出
- SharePoint® 服务器支持
- 内置许多 XML 文档类型的模板
- 显示 XML 数据的图表创建
- XPath 1.0/2.0 的智能自动完成
- XSLT 1.0/2.0 编辑器、分析器和调试器
- XQuery 编辑器、分析器和调试器
- SOAP 客户端和调试器
- 图像化的 WSDL 1.1/2.0 编辑器
- XBRL 验证 & 分类编辑
- 支持 Office 2007 / OOXML
- Java、C++ 和 C# 的代码生成
- HTML5 和 CSS3 支持


## XML E4X
E4X 向 JavaScript 添加了对 XML 的直接支持。

### 作为一个 JavaScript 对象的 XML
E4X 是正式的 JavaScript 标准，增加了对 XML 的直接支持。
使用 E4X，您可以用声明 Date 或 Array 对象变量的方式声明 XML 对象变量：
```js
var x = new XML()
var y = new Date()
var z = new Array()
```

### E4X 是一个 ECMAScript（JavaScript）标准
ECMAScript 是 JavaScript 的正式名称。ECMA-262（JavaScript 1.3）是在 1999 年 12 月标准化的。
E4X 是 JavaScript 的扩展，增加了对 XML 的直接支持。ECMA-357（E4X）是在 2004 年 6 月标准化的。
ECMA 组织（成立于 1961 年），是专门用于信息和通信技术（ICT）和消费电子（CE）的标准化。 ECMA 制定的标准为：
- JavaScript
- C# 语言
- 国际字符集
- 光盘
- 磁带
- 数据压缩
- 数据通信
- ...

### 没有使用 E4X
加载一个现有的 XML 文档（"note.xml"）到 XML 解析器，并显示消息说明：
```js
var xmlDoc;
//code for Internet Explorer
if (window.ActiveXObject) {
  xmlDoc = new ActiveXObject("Microsoft.XMLDOM");
  xmlDoc.async = false;
  xmlDoc.load("note.xml");
  displaymessage();
}
// code for Mozilla, Firefox, etc.
else (document.implementation && document.implementation.createDocument)
{
  xmlDoc = document.implementation.createDocument("", "", null);
  xmlDoc.load("note.xml");
  xmlDoc.onload = displaymessage;
}

function displaymessage() {
  document.write(xmlDoc.getElementsByTagName("body")[0].firstChild.nodeValue);
}
```

### 使用 E4X
```js
var xmlDoc=new XML();
xmlDoc.load("note.xml");
document.write(xmlDoc.body);
```

### 浏览器支持
Firefox 是目前唯一对 E4X 的支持比较好的浏览器。
目前还没有支持 E4X 的有 Opera、Chrome 或 Safari。
到目前为止，没有迹象显示在 Internet Explorer 中对 E4X 的支持。

### E4X 的未来
E4X 没有得到广泛的支持。也许它提供的实用功能太少，尚未被其他的解决方案涉及：
- 对于完整的 XML 处理，您还需要学习 XML DOM 和 XPath
- 对于访问 XMLHttpRequests，JSON 是首选的格式。
- 对于简单的文档处理，JQuery 选择更容易。


## XML 总结
- XML 可用于交换、共享和存储数据。
- XML 文档形成 树状结构，在"根"和"叶子"的分支机构开始的。
- XML 有非常简单的 语法规则。带有正确语法的 XML 是"形式良好"的。有效的 XML 是针对 DTD 进行验证的。
- XSLT 用于把 XML 转换为其他格式，比如 HTML。
- 所有现代的浏览器有一个内建的 XML 解析器，可读取和操作 XML。
- DOM（Document Object Model）定义了一个访问 XML 的标准方式。
- XMLHttpRequest 对象提供了一个网页加载后与服务器进行通信的方式。
- XML 命名空间提供了一种避免元素命名冲突的方法。
- CDATA 区域内的文本会被解析器忽略。
- 我们的 XML 实例也代表了这个 XML 教程总结。

### 下一步
XML DOM
- XML DOM 定义了一种访问和处理 XML 文档的标准方式。
- XML DOM 是平台和语言独立的，可用于任何编程语言，如 Java、JavaScript 和 VBScript。

XSLT
- XSLT 是 XML 文件的样式表语言。
- 通过使用 XSLT，可以把 XML 文档转换为其他格式，比如 XHTML。

XML DTD
- DTD 的目的是定义 XML 文档中合法的元素、属性和实体。
- 通过使用 DTD，每个 XML 文件可以随身携带它自己的格式的描述。
- DTD 可以被用来确认您收到的数据和您自己的数据是否有效。

Schema
- XML Schema 是一种基于 XML 的 DTD 替代。
- 不像 DTD，XML Schema 支持数据类型，且使用 XML 语法。


## XML 实例
略
