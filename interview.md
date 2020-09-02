###面试题
2020.09.01  
9月第一天借到阿里电话面试的通知 紧张自己什么都没准备好
临时抱佛脚复习复习了之前的rudux基础，react基础
然后刷刷面试题吧


###水平居中的几种方法
  1.行内元素，给其父元素设置text-align:center
  2.块级元素，该元素设置margin:0 auto即可
    ‘margin-left’（x） + ‘border-left-width’（0） + ‘padding-left’（0） + ‘width’（假设 300px） + ‘padding-right’（0） + ‘border-right-width’ （0）+ ‘margin-right’（x） = width of containing block（假设 600 px）
    x+0+0+300+0+0+x = 600
    求x
    2x+300 = 600
    2x = 300
    x = 150 （px）
  3.flex布局
  ```
    .son{
      display:flex;
      justify-content:center;
    }
  ```


###垂直居中
 1.若元素是但行文本，则可以设置line-height等于父元素高度
 2.块级元素的父元素上，设置display:flex 和align-items:center(start end)
 3.使用display:inline-block,vertical-algin:middle 和一个伪元素让内容快处于容器中央
    ```
      .parent::after,.son{
        display:inline-block;
        vertical-align:middle;
      }
      .parent::after{
        content:'';
        height:100%;
      }

    ```


### let var const 区别

const 声明必须初始化且只读不可更改
let 不支持变量提升，拥有块级作用域
var 支持变量提升，没有块级作用域
