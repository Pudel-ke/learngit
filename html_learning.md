# HTML学习笔记



## day1 

###  1.网页

* 构成网站的基本元素，由文字、声音、视频等元素组成
* 通常是.htm或.html文件
* HTML 超文本语言是一种**标记语言**

### 2.WEB标准

* W3C等组织制定的标准集合
* 结构

> HTML结构标签

* 表现

> CSS实现表现

* 行为

> Javascripts实现行为交互效果，“动起来”

### 3.html标签(语义化标签学习)

结构：

    <!DOCTYPE>
    <html>
    <head>
    <title>..</title>
    <meta ..>
    </head>  
    <body>.
    .
    .
    </body>
    <html>

 



### 4.具体标签

#### 双标签

< h1>..< /h1>:标题标签，head，h1-h6由大到小

< p>...< /p> : paragraph，段落标签

< a href="路径"> 锚点链接(文字图片均可)</ a>:  anchor, 其中href 必加，可实现跳转（配合属性id="",href="#"）or网页切换,属性中有target="_self",本页跳转，=“ _blank” 新页面跳转

< div>..< /div> :单独成一行

< span>..< /span> 一行多个"box"

#### 单标签

< br> : break，换行符

< img src="" > 属性可加height,width, border等

< !---...---> 注释标签  ctrl+/ 

特殊字符：&nbsp ; 空格  

​                   &lt ;  小于号 <   less than

​                   &gt ; 大于号>   greater than

## day2

### 表格标签

```html
<table>
    <tr>
        <th>表头1</th>
        <th>表头2</th>
        <th>表头3</th>
        <!---一般用th是对表头进行加粗，td也行，代表数据单元格--->
    </tr>
    <tr>
        <td>meta1</td>
        <td>meta2</td>
        <td>meta3</td>
    </tr>
</table>
```

tr: table row  

td: table data  可以是其他内容，不仅限于文字

表格相关属性:

* align    表格相对周围元素对其 left center right
* border  表格边框 单位为px
* cellpadding  元素距离边框内容
* cellspacing   单元格之间的距离
* width           表宽度
* height         表高度

**后续学习CSS，在CSS中设置**

表格结构标签：

* < thead> </ thead>  表格头部一行
* < tbody> </ tbody> 表格主体区域

>  表格更好的语义

合并单元格:

* 行合并  rowspan   从最上一行开始 < td rowspan="2"> </ td>
* 列合并  colspan  从最左边的一列开始  < td colspan="3"> </ td>

**删除多余单元格**



### 列表标签

#### 无序列表

​	unordered list

```html
<ul>
    
</ul>
```









