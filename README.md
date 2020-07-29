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
.class1 .class2{
  只会应用到应用class1的内容中应用class2的元素
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


块级盒子会沿着垂直方向堆叠，盒子在垂直方向上的间距有他们的上下外边距决定

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

ps。今天突然知道留学生要提前一年参加秋招，突然感觉时间紧 任务重 不过好在已经开始自己学习课外的东西了 争取一个月看完这三本关于前段开发的书 并且同时要刷算法题和计算机网络 还有数据库 还有实战项目 准备参加今年秋招 希望不会太惨

文本颜色 --- color
字体族(font family)属性的值是一个被选字体的列表，按优先级从左到右排列

```
body {
  font-family : 'Georigia Pro',Georgial,Times
}
h1,h2,h3,h4,h5,h6 {
  font-family : Avenir Next,SegoeUI,arial
}
```

body元素使用的字体依次是 'Georigia Pro', Georgial, Times 如果没有则会选择下一个


字体与字型
所有浏览器font-size的默认大小是16像素，我们不修改默认的font-size，而是选择使用em单位调整特定元素的大小
em单位用于font-size属性时，实际上是一个相应元素 **继承** 的font-size缩放因子。比如我们的h3元素，字型大小就是1.314*16=21px
rem单位则是相对于根元素，基与html的font-size缩放
```
h1,h2,h3,h4,h5,h6{
  margin-top:1.5rem /*24px*/
}
```
当em用于计算盒模型的大小时，它不是基于继承的font-size，而是基于元素自身计算的font-size。因此，不同级别的标题对应的font-size不一样
为了得到一致的值，要么使用rem 要么对每个标题级别分别以em计算margin-top的值

line-height:行高 行间距 一般设置为1.2-1.5之间，一般不会去设置单位，意思是当前的font-size的1.5倍。body的font-size为16px那么line-height=24px
如果给line-height设置像素值，百分比值或者em值，子元素继承的是一个固定的数值而不是一个系数。
不设置单位则继承的是一个和自己font-size成正比的系数

#### 文本粗细 font-weight
一般直接给出关键词 normal、bold、bolder、lighter
也可以直接给出数值 100 200 都是100的整倍数

#### 大小写转换 text-transform：uppercase lowercase capitalize

#### 文本缩进 text-indent

#### text-align   left right center justify
给text-align属性应用justify值，可以在但此间平均分布间距，结果就是左右两端对齐，消除毛边

####  `@font-face规则`

嵌入Web的字体的关键是@font-face规则。通过它可以制定浏览器下载Web字体的服务器地址、以及如果在样式表中引用改字体
```
  @font-face{
    font-family:Vollkorn;
    font-weight:bold;
    src:url("fonts/vollkorn/Vollkorn-Bold.woff") format('woff')  
  }
  h1,h2,h3,h4,h5,h6{
    font-family:Vollkorn,Georgia,serif;
    font-weight:bold;
  }
```

前面@font-face块生命了在font-family值为Vollkorn且为粗体时应用该规则。之后提供了URL
生命了新字体后就可以通过css中的font-family属性正常使用它

#### 字距调整
```
.kern{
  font-kerning:normal;(auto)
  font-feature-settings:"kern";
}

```
### 文本特效
文本阴影 text-shadow
```
h1 {
  text-shadow:-.2em .4em .2em     #ccc
  //         x轴    y轴  模糊距离  颜色   模糊距离=0（不模糊）
}
```

### 背景颜色透明度
rgba-> red grenn blue alpha->代表控制透明度的通道 [0-1]
opacity同样可以控制透明度
区别在于opacity使得元素透明后 元素内容也同样变透明了 无法让其子元素变得不透明

####图片的位置

background-position：一般是给两个值 一个表示相对于左侧的偏移量，一个是表示相对于右侧的偏移量

background-clip属性可以剪裁图片大小 boarder-box、padding-box、context-box 剪裁掉这三个box之外的图片

background-attachment 背景会附着在制定元素的后面

background-size 明确指定一个值，可以重新设置图片的大小，也可以让它随元素大小缩放而缩放

background-size关键词 contain 让浏览器尽可能保持图片最大化，同时不改变图片的宽高比 浏览器自动绝对哪边用100%哪边用auto

                      cover 图片会缩放以保证覆盖元素的每一个像素，同时不会变形


```
.profile-box{
  background-size:400px, 240px;
}
```

要让图片随着元素缩放而缩放，则必须使用百分比值。百分比值是相对于容器大小而不是相对于图片的大小
因此一般不讲图片的宽度和高度都设置成百分比值，可能会因容器的高度变化而导致图片变形
更好的做法是值给一个纬度设置百分比值，另一个纬度设置关键词auto
```
.profile-box{
  background-size:100% auto

}
```


 background属性简写
```
.profile-box{
  background:url(img/cat.jpg) 50% 50% / cover no-repeat padding-box content-box #bada55;
}
```
因为background的属性有两个长度 background-postion 和background-size 要先写position后写size且数值后面以/分开

### 边框半径：圆角
border-radius:0.5em 2em 0,5em 2em 表示从左上角开始按顺时针方向依次列出各个值

正圆:border-raidus:50% 胶囊两头的半圆border-radius:9999rem（不是百分比的任意大的数值）

### 边框图片 border-image
会自动讲图片分成9块 中间那块被content-box取代
### 盒阴影 box-shadow  和text-shadow一样
相比text-shadow新增扩展半径 可以在模糊半径后面再加一个值、用于表示扩展阴影的大小。这个值默认为0，即阴影于所属元素一样大。增大这个值，阴影相应增大

新增内阴影inset

把元素当成投影表面，可以创造一种背景被镂空的效果。

### 略过了渐变 阴影 理解范围之外了

### 可伸缩的图片模式
```
img{
  max-width:100%；
}
```
max-width属性意味着图片会随着包含它的容器缩小而缩小，但在容器变大时，他不会大到超过自身固有尺寸。

控制对象大小的新方法 一个矩形的图片想设置成方形可以使用object-fit属性，不会破坏原图像的宽高比
![avatar](/Users/shuophone0432346234/Desktop/前端学习资料/精通css/object-fit.png)


### 可保持宽高比的容器
对于具有固定宽高比的图可以把高度设置为auto只改变宽度
对于没有固定宽高比的元素：iframe  object
通过padding-bottom来实现一个<div>包装标签
padding-bottom值为%的时候是根据定位块的width去计算
```
.object-wrapper{
  width:100%;
  height:0;
  padding-bottom:75%
}
4/3的宽高比的容器

尽管包装元素的高度是0，仍然可以通过绝对定位把嵌入对象放到一个可保持宽高比的内边距盒子理
.object-wrapper{
  width:100%;
  height:0;
  position:relative;
  padding-bottom:75%;
}

.object-wrapper iframe{
  postion:absolute;
  width:100%;
  height:0;
  padding-bottom:75%
}

```



### 内容布局


css中负外边距是有效的
左边或者上边的负外边距会把像素向左或者向上拉
右边或者下边的负外边距会把相邻的元素向左或者向上拉


#### 水平布局


### 使用表格显示属性实现布局

```
.navbar ul {      //ul是css的列表样式：纵向
  width:100%;
  display:table;
  table-layout :fixed;
}

.navbar li{       //li是css的的列表内容
  width:25%;
  display:table-cell;//table-cell 横向排列
}
```
>display:table-cell；绝对是一个现代的布局神器。

用float来做布局触发的问题比较多，例如要清除浮动，元素浮动后还会导致该元素脱离文档流，即使你清除float，该元素依旧是脱离文档流。

左右布局能用display:inline-block;布局我就用它来布局，但是还是无法完全不使用它，很多布局例如需要靠左和靠右的布局场景下就没办法不去使用float来布局。
元素两端对齐
第一个案例是让两个元素分别向左和向右对齐，如果是过去，我一定会用float来实现，但其实用table可以这么做：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
    <style type="text/css">
        *{
            box-sizing:border-box;
         }

        .content{
            display: table;
            border:1px solid #06c;
            padding:15px 15px;
            max-width: 1000px;
            margin:10px auto;
            min-width: 320px;
            width:100%;
         }

        .box{
            width:100px;
            height:100px;
            border:1px solid #ccc;
            text-align: center;
            display: inline-block;
            font-size: 40px;
            line-height: 100px;
        }
       .right{
            text-align: right;
            display: table-cell;
        }
       .left{
            text-align: left;
            display: table-cell;
        }     
    </style>
</head>
<body>
   <div class="content">
        <div class="left">
             <div class="box">B</div>
        </div>
        <div class="right">
             <div class="box">A</div>
        </div>
   </div>
</body>
</html>
```
>>自动平均划分每个小模块，使其一行显示

遇到上面这种布局，一般会用float来做，或者把每个li设置成display:inline-block;来做，并且都要设置给他们设置一个宽度，而且最痛苦的是5个li如果你设置width:20%;他们一定会掉下来，如果li都设置成display:table-cell；就不会出现这种情况，即使不设置宽度他们也会在一行显示，你在加多一行他也不会掉下来，依旧会在一样显示。
```
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title></title>
   <style type="text/css">
         *{
           box-sizing:border-box;
          }

         .content{
             display: table;
             border:1px solid #06c;
             padding:15px 15px;
             max-width: 1000px;
             margin:10px auto;
             min-width:320px;
             width:100%;
         }
        .content ul{
             display: table;
             width:100%;
             padding:0;
             border-right:1px solid #ccc;
         }

        .content ul li{
            display: table-cell;
            border:1px solid #ccc;
            text-align: center;
            height:100px;
            border-right: none;
            line-height: 100px;
         }
   </style>
</head>
<body>
   <div class="content">
       <ul>
         <li>1</li>
         <li>2</li>
         <li>3</li>
         <li>4</li>
         <li>5</li>
      </ul>
   </div>
</body>
</html>
```
>图片垂直居中于元素

```
<!DOCTYPE html>
<html lang="en">
<head>
       <meta charset="UTF-8">
       <title></title>
       <style type="text/css">
        *{
            box-sizing:border-box;
         }

        .content{
            display: table;
            border:1px solid #06c;
            padding:15px 15px;
            max-width: 1000px;
            margin:10px auto;
            min-width:320px;
            width:100%;
         }
        .img-box{
            height:150px;
            width:100px;
            border:1px solid red;
            display: table-cell;
            vertical-align: middle;
            text-align: center;
            background-color: #4679bd;
        }
       </style>
 </head>
 <body>
      <div class="content">
            <div class="img-box">
               ![logo](http://upload-images.jianshu.io/upload_images/1432546-53d1c7f44dc6e873.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
            </div>
      </div>  
 </body>
 </html>
 ```

2020.7.28
END 女朋友夸我这个学习的笔记了 开心😄
### flex box
加油 晚上还要和朋友学怎么用ps做简历、

flexbox可以针对页面中某一区域、控制其中元素的顺序、大小、分布及对齐。这个区域内的盒子可以沿着两个方向排列：默认水平排列(成一行),也可以垂直排列(成一列)。这个排列方向成为主轴。

与主轴垂直的方向成为辅轴，区域内的盒子可以沿辅轴发生位移或者伸缩
justify-content用于指定排布方式，其默认值是flex-start，表示按照当前文本方向排序(flex-end,center,space-between,space-around)
flexbox不允许通过以上这些关键字指定个别项的排布方式。然而,对flexbox的子项指定值为auto的外边距可以让该外边距扩展占据可用空间。

因此可以制造出一项在左侧 剩余的在右侧

```
.navbar li:first-child {
  margin-right:auto
}
```
辅轴对齐
align-items控制辅轴对齐属性
默认值stretch，子项默认拉伸填满可用空间
flex-start center flex-end 可选
容器的基线由较大的活动项决定 其他的和基线对齐

flexbox垂直对齐
在容器里面只有一个元素的时候，只要将容器设置为flex，再将需要居中的元素的外边距设置为auto就行了（外边距会自动扩展填充）

也可使用justify-content algin-items 垂直居中




flexbox的相关属性
  1.flex-basis:控制项目在主轴方向上、经过修正之前的"首选"大小(width或height)。可以是长度值(18em)、百分比(相对于容器的主轴而已),也可以是关键字auto(默认值)
  2.flex-grow
  设置了一个flex项主尺寸的flex增长系数。它指定了flex容器中剩余空间的多少应该分配给项目（flex增长系数）剩余的空间是flex容器的大小减去所有flex项的大小加起来的大小。如果所有的兄弟项目都有相同的flex-grow系数，那么所有的项目将获得相同的剩余空间，否则将根据不同的flex-grow系数定义的比例进行分配。

3.flex-shrink
  于flex-grow作用相反，空间不够后怎么收缩。

flexbox计算过程
 1.检查flex-basis,确定假象的主尺寸
 2.确定实际的主尺寸。如果按照flex-basis把各项拍排布好之后，容器内还有剩余空间，那么他们可以伸展。伸展多少由flex-grow系数决定。相应的，如果容器装不下那么多项，则根据flex-shrink系数决定各项如何收缩。
一次性设置flex-grow、flex-shrink、flex-basis
```
.navbar li {
  flex: 1 0 0%
}
flex-basis必须带单位(%,px)
```


```
.tags {
  display:flex; 将元素列表转换成flex box
  flex-wrap:wrap; 告诉子元素可以折行
  margin:0;
  padding:0;
  list-style:none;
}
```
flex-direction:row-reverse 可以反转行中标签的排布方向(默认从左到右 row-reverse后 从右到左)
flex-warp：wrap-reverse也可以反转垂直排布的方向，让第一行从底部开始向上折行



#2020.7.29 今天不想看书 不想看书 不想看书
今天不想学习不想学习不想学习、不学不行啊 不学没有好工作 没有好工作就没有女朋友

##页面布局
固定布局：页面具有特定的宽度
弹性布局：布局元素的尺寸使用em单位。这样即使用户缩放文本大小，布局的比例也不会变
流流布局：页面元素按比例缩放，但元素与元素之间的比率保持不变（web的默认模式）
网格布局：display:grid

网格布局
```
.grid-a {
  display:grid;
  grid-template-rows:300px 300px;         //定义row的宽度和高度 支持auto关键词
  grid-template-columns:1fr 1fr 1fr 1fr;  //4列等宽 可以换成repeat(4,1fr)
                                          //每个网络格项没有特定的元素表示 无法通过
                                          //max-width or min-width 指定大小
                                          //可以使用minmax函数指定

  grid-template-rows:auto minmax(4em,1fr) // 最后一行至少4em高，相距相等的可用空间   
}

```
grid-tempale语法糖
```
.grid-a{
  display:grid;
  gird-template:auto minmax(4em,1fr) minmax(4em,1fr) / repeat(5,1fr)
  //前面是行 后面是列 用/隔开

}

```

添加网络项需要同时指定两个维度上起止的网格编号
```
.subsection-header{
  grid-row-start:1;
  grid-column-start:1;
  gird-row-end:4;
  gird-column-end:2;
}
```
语法糖

```
.subsection-header{
  grid-row:1/4；     //grid支持(-) -1表示最后一个一个网格轨道的终止网格线的编号
  gird-column:1/2    //默认的跨度是一个网格单元
  //上面两行改写为
  grid-row:1/-1;
  grid-column:1
}

```
语法糖
```
.subsection-header{
  grid-area:1/1/-1    /* 1/1/1-1/1 */ 省略最后一个默认跨度1
                      /* row-start,column-start,row-end,column-end*/
}

```
垂直对齐:align-items align-self 水平对齐:justify-items justify-self
align对-items目标整个网格
align-self 网络项


###自动网格定位
默认机制 ，不会改面网格项的源代码次序。所有网格项自动从第一行第一个可用的网格单元开始，逐列填充。一行填满后，网格会自动开启下一行并继续填充

```
.grid-a{                                //定义网格    基于浮动实现 个人感觉不会常用
  display:grid;
  grid-template-rows:auto auto auto;
  grid-template-columns:repeat(5,1fr)
}

.subcategory-header{                    //定义标题区域
  grid-row:1/-1
}

.story-featured{                        //重点文章看跨两列
  grid-column: span 2;
}
```


###网络模版区
实现了可视化网格布局

```
.grid-b {
  display:grid;
  grid-template-columns:20% 1fr 1fr 1fr;
  grid-template-areas: "hd st1 . st2"
                       "hd st1 . st2"

/*
grid-template-areas 属性的值是以空格分割的字符串列表，每个字符串本身是空格分割的自定义标示符，标示网格中的一行，其中每个标示符标示一列。跨行or跨列相邻的同名网络单元构成命名网格区。命名网格区必须是矩形。用点号标示的区域是匿名单元，没有名字

*/

}

```

将网络项放到放格中仍然使用gird-area属性
```
.grid-b .subcategory-header{
  grid-area : hd;
}
.grid-b .story:nth-child(2){
  grid-area : st1;          //使用了伪类nth-child，找到当前元素的兄弟元素并标记
}
.grid-b .story:nth-child(3){
  grid-area : st2;
}


```

2020.7.30
今儿还看了看react的<route> 不知道应该记些什么 以后可能还会去看react的document 以后可以补上
done
#响应式web设计于css
