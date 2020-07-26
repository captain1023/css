# -
2020.7.26

终于搞好atom了
尝试使用Mark down来记录我这将来一个月的前端学习之路 😂

# css
 div元素----为了作为额外的样子接入点或者改变布局（类似居中）

 span元素 ---无需表示语义、仅需要添加样式的情况中

 \<i>元素用于表示与周围内容不一样的内容（斜体）

 \<b>元素用于表示粗体

### Css selector and Pseudo class element
```
p {
  color:black,    
}
```
**选择一类**

```
blockquote p {
  padding-lef:2em;
}
```
**descendant selector(后代选择器)**:只会应用到block quote中到p段落 缩进2em

**ID选择器(#)和class(.)选择器**
ID选择器可以与descendant selector结合
```
#intro h1{
  font-size:1.8em     
}
```
只改变intro中的h1

**子选择器(child selector)** 只能选择作为某元素的子元素

```
h1 > strong {color:red;}
<h1>This is <strong>very</strong> <strong>very</strong> important.</h1>
<h1>This is <em>really <strong>very</strong></em> important.</h1>
```
第一个行但strong会变成红色

第二行不会 strong的父元素是em

目前理解的子元素：h1标签后紧接跟着的标签


**Adjacent sibling combinator(相邻兄弟选择器)**

p1 + li {font-weight:bold;}

用一个结合符只能选择两个相邻兄弟中的第二个元素(只能选中li)

综合例子
html > body table + ul {margin-top:20px;}
body中的table的ul margin top 20px且body是html的子元素

**通用选择器**

可以匹配任何元素 (`*`)

**属性选择器**

a[herf] {color:red;} 对有herf链接的a元素应用

可以根据属性值来选择

```
input[type="submit"] {
  cursor:pointer;
}

匹配某些字符的开头属性
a[herf ^= "https:"]
匹配以某些字符结尾
img[src$=".jpg"]
匹配包含某些字符的属性值
a[herf *= "/about/"]包含about
匹配以空格分割的字符串中的属性值(rel属性的值)
a[rea ~=next]
```

**伪类**
```
a:link {color: #FF0000}
a:visited {color: #00FF00}
a:hover {color: #FF00FF}
a:active {color: #0000FF}

```

**结构化伪类**

css3新增nth-child 可以用来交替的为表格行应用样式

tr:nth-child(odd)奇数行应用style

tr:nth-child(3n+4)表达式(n=0,1,2,3)

input伪类
input[type="email"]:valid{

}
input[type="email"]:invalid{

}
### 层叠(cascade)
层叠机制重要性级别从高到底


note:
样式写在了元素的style属性里，那么这些样式的特殊性就最高 。
通过id属性应用的规则高于未通过id属性应用的规则
通过类选择器应用的规则，其特殊性高于只通过类型选择器应用的规则(????)

###css加载性能
如果在HTML文档的\<head>元素中加入\<script>元素，浏览器必须先把它链接的脚本下载下来，然后再向用户显示网页内容。换句话说，这种情况下的html和css解析完全被下载以及执行脚本阻断了，也就是所谓的"渲染阻塞"

渲染阻塞会明显拖慢网站加载速度。为此，主流的做法是在HTML页面底部的结束标签\</body>之前加载JavaScript
```
<scrpit src="/scripts/core.js"></script>
</body>
```
比较现在的做法是在head中使用scpirt标签但添加 **async 和defer** 属性。给\<script>标签加上asyn属性，会异步加载脚本，不阻塞html解析，但会在脚本加载完毕执行时阻断HTML解析。
给script加上defer属性，同样会异步加载脚本，不同但是会在HTML解析完毕后再执行加载但脚本


## 可见格式化模型
一般布局中很少使用height去限制box模型(通过top bottom调整位置)

元素的height高度通常应该取决于包含的内容

块级元素:p、h1、article这些元素都是块级元素

行内元素:strong、span time 以行内盒子的形式显示在行内

可以通过把display属性设置为block、让span变得块级元素意义

如果display=none 还可以让浏览器不为相应的元素生成盒子(不占用文档空间，内容也不会显示)

## css定位模型：浮动、绝对定位和相对定位
除非特别制定，否则所有的元素盒子都会在常规文档流中生成，即position的属性默认为static(位置由元素在html中的位置绝对)


块级盒子会沿着锤子方向堆叠，盒子在垂直方向上的艰巨有他们的上下外边距决定

行内盒子是沿着文本流水品排列的，也会随着文本行换行而换行。他们之间的水平间距可以通过水平方向的内边距、边框和外边距调节。但行内盒子但高度不受其为垂直方向上的内边距、边框、外边距影响。此外给行内盒子明确设置高度和宽度也不会起作用


由一行文本形成但水平盒子叫行盒子(line box),而行盒子的高度由所包含但行内盒子决定。修改行盒子大小的唯一途径就是修改行高(line-height)，或者给它内部的行盒子设置水平方向的边框、内边距或者外边距。


###外边距折叠
常规块盒子有一种机制叫做外边距折叠。当垂直方向上当两个外边距相遇时，会折叠成一个外边距。折叠后外边距的高度等于两者中较大的那一个高度。
外边距折叠只发生在常规文本流中的块级盒子的垂直方向上。
行内盒子、浮动盒子、或者绝对定位盒子的外边距不会折叠


### 包含块
默认情况下，width、height、margin 和padding 的值为百分比时，就以该父元素的尺寸为计算依据
#### 相对定位
包含块定位到最近的父元素，该元素display的属性值必须能够提供类似块级的上下文，如block、inline-block
display属性设置为relative 元素仍会在原来的位置
top left right bottom会相对于原来的位置进行偏移
#### 绝对定位
绝对定位会把元素拿出文档流，因为也就不在再占用原来的空间。与此同时，文档流中的其他元素会各自重新定位，仿佛绝对定位的元素没有存在过一样。
包含块是距离它最近的定位祖先，也就是display属性设置为static之外任意值的祖先元素。如果没有这么一个定位祖先，那么它就相对于文档的跟元素即html元素定位。
可以使用z-index属性控制盒子的堆叠顺序。z-index属性越大，盒子在层叠是次序就越靠近用户的眼睛。
#### 固定定位
固定定位元素的包含块是视口(viewport)。因此，固定定位可用来创建始终停留在窗口相同位置的浮动元素。
应用：固定导航栏（滑动条下拉导航栏依旧可见）
#### 浮动
浮动盒子可以向左或者向右移动，知道其外边沿接触包含块的外边沿或者接触另一个浮动盒子的外边沿。浮动盒子也会脱离常规文档流，因此常规流中的其他块盒子的表现，几乎当浮动盒子根本不存在一样。
如果包含元素太窄，无法容纳所有浮动元素水平排列，则后面的浮动元素会向下移动。如果高度不够则会卡在前面的浮动元素右下方

行盒子与清除
跟在浮动元素后面的行盒子会缩短，从而为浮动元素留空，造成文本环绕浮动盒子的效果。要阻止行盒子环绕在浮动盒子外面，需要给包含行盒子的元素应用clear属性。clear属性的值有left、right、both、none，用于指定盒子但哪一侧不应该紧挨着浮动盒子。
浮动但元素会被拿出文档流，因为不会再常规文档流中有高度。
![avatar](/Users/shuophone0432346234/Desktop/前端学习资料/精通css/float)

2020.7.26

课外学习第一天 以后也要加油

atom真的蛮好用的哈哈哈哈哈哈哈

End

## 网页排版
