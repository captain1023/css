# -
终于搞好atom了
尝试使用Mark down来记录我这将来一个月的前端学习之路 😂

# css

 div元素----为了作为额外的样子接入点或者改变布局（类似居中）
 span元素 ---无需表示语义、仅需要添加样式的情况中
 <i>元素用于表示与周围内容不一样的内容（斜体）
 <b>元素用于表示粗体

###css selector and Pseudo class element
```
p {
  color:black,    
}
```
选择一类

```
blockquote p {
  padding-lef:2em;
}
```
descendant selector(后代选择器):只会应用到block quote中到p段落 缩进2em

ID选择器(#)和class(.)选择器
ID选择器可以与descendant selector结合
```
#intro h1{
  font-size:1.8em     
}
```
只改变intro中的h1

子选择器(child selector) 只能选择作为某元素的子元素
h1 > strong {color:red;}
<h1>This is <strong>very</strong> <strong>very</strong> important.</h1>
<h1>This is <em>really <strong>very</strong></em> important.</h1>
目前理解的子元素：h1标签后紧接跟着的标签


Adjacent sibling combinator(相邻兄弟选择器)
p1 + li {font-weight:bold;}
用一个结合符只能选择两个相邻兄弟中的第二个元素(只能选中li)

综合例子
html > body table + ul {margin-top:20px;}
body中的table的ul margin top 20px且body是html的子元素

通用选择器
可以匹配任何元素 (`*`)
属性选择器
a[herf] {color:red;} 对有herf链接的a元素应用
可以根据属性值来选择如
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

伪类
```
a:link {color: #FF0000}
a:visited {color: #00FF00}
a:hover {color: #FF00FF}
a:active {color: #0000FF}

```

结构化伪类
css3新增nth-child 可以用来交替的为表格行应用样式
tr:nth-child(odd)奇数行应用style
tr:nth-child(3n+4)表达式(n=0,1,2,3)

input伪类
input[type="email"]:valid{

}
input[type="email"]:invalid{

}
