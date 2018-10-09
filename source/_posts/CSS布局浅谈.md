---
title: CSS布局浅谈
date: 2018-10-10 04:14:58
tags:
---
>本文主要介绍一些简单css初级布局方式，包括左右布局、左中右布局、水平居中、垂直居中。

#横向布局

###两栏布局

这里介绍两栏布局的其中一栏自适应的例子，方法是**float+margin**
![两栏布局1.png](https://upload-images.jianshu.io/upload_images/8661291-db7635bcc3058344.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![两栏布局2.png](https://upload-images.jianshu.io/upload_images/8661291-7b8b610760d90dcc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
代码如下：
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>JS Bin</title>
</head>
<body .clearfix>
  <aside></aside>
  <main></main>
  <span>11111</span>
</body>
</html>
```
```
*{
  margin:0;
  padding:0;
}
.clearfix{
  content: "";
  display: block;
  clear: both;
}
aside{
  float: right;
  background: red;
  height: 200px;
  width: 100px;
}
main{
  background: yellow;
  height: 200px;
  margin-right: 100px;
}
```
**值得注意的是HTML的书写顺序不能调换，因为渲染顺序，如果main和aside换了就不会出现在同一行上了。**同理三栏布局也可以使用这种方式。
<br>

### 两栏布局，都自适应
例子中的两栏，随着屏幕的缩放而等比（50%）收缩，

![两栏等比适应.png](https://upload-images.jianshu.io/upload_images/8661291-339738fb6c9d6fb0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
<body>
  <div class="main">
    <div>1列</div>
    <div>2列</div>
    <div>3列</div>
    <div>4列</div>
    <div>5列</div>
    <div>6列</div>
    <div>7列</div>
    <div>8列</div>
  </div>
  <div class="aside">
    <div>1列</div>
    <div>2列</div>
    <div>3列</div>
    <div>4列</div>
    <div>5列</div>
    <div>6列</div>
    <div>7列</div>
    <div>8列</div>
  </div>
</body>
```
```
*{
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body{
  font-size: 0px;
}
.main{
  width:50%;
  background-color:red;
  font-size: 20px;
  display: inline-block;
  vertical-align: top;
}
.aside{ 
  background-color:yellow;
  width:50%;
  font-size: 20px;
  display: inline-block;
  vertical-align: top;
}
```
要点：使用ie盒模型，让计算简单。其中最重要的就是**font-size的设定**，假如没有设定这个属性就会出现下面的情况
![错误的情况.png](https://upload-images.jianshu.io/upload_images/8661291-a2af1b4e08d5180c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
最开始我是很惊奇的，明明“什么都没有”，宽度也正好够，怎么不并排呢？问过才知道，**这里面有个“回车”**，解决办法有几种，最为符合书写规范的就是用**font-size:0**。


##### 横向布局的看法：
横向布局其实最主要的是解决宽度问题，只要两个元素之间的宽度不冲突，就能布局完成。第一个例子用float+margin的方式，让浮动使aside脱离文档流，这样main就不会和aside冲突，同时设定margin的值使外观上也不冲突。第二个例子用IE盒模型和width计算出相应的宽度，最后把回车干掉，问题解决。

<hr>

#纵向布局
解决高度问题即可，用margin调节，不多赘述。

<hr>

#块级元素内部布局问题

###行内元素左右对齐
一般的行内元素对齐都很简单，text-align解决大部分问题。但这里举一个经典例子，看图：
![文字两端对齐布局.png](https://upload-images.jianshu.io/upload_images/8661291-19b463f1e5205268.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
当前除了css3新功能之外，能简单做到这点的真不多。这里给出代码：
```
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>JS Bin</title>
</head>
<body>
  <div>姓名</div>
  <div>联系电话</div>
</body>
</html>
```
```
div{
  width:80px;
  /* border:1px solid; */
  text-align: justify;
  vertical-align: top;
  line-height: 8px;
}

div:after {
    content: " ";
    display: inline-block;
    width: 100%;
}
```
这里的要点是**：text-align: justify;**出于一些书写上的原因，**默认最后一行不两端对齐**，所以当你只有一行时，不产生效果。我们有必要在最后添加一个空行，但是添加标签违反了书写的规则，所以**添加伪元素**是比较合适的选择。另外在两个div之间是有空隙的（还不小），这个时候用行高解决问题**调节line-height**，调试的时候加上**边框**。
<hr>

###行内元素水平居中
<br>

![行内元素水平居中.png](https://upload-images.jianshu.io/upload_images/8661291-2ba5c32f483fcc60.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
<body>
  <span>行内元素水平居中</span>
</body>
```
```
body{
  text-align: center;
}
```
这个不需要我多说吧。只需要记得在**父元素**上加text-align：center；
<hr>

###块级元素水平居中

####宽度确定的块级元素居中
![宽度确定的块级元素居中.png](https://upload-images.jianshu.io/upload_images/8661291-bd9513957a0cddd7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
<body>
  <div>块级元素水平居中</div>
</body>
```
```
div{
  border: 1px solid;
  margin: 0 auto;
  width:200px; 
}
```
要点在：**margin: 0 auto;**另外子元素宽度必须确定，不然撑满整个父容器哪里有居中这说法？文字要居中的话在div上添加text-align:center;即可。
<br>

####宽度不确定的块级元素居中
现实中经常遇到块级元素不确定宽度的情况，那么还是上面的例子：
![宽度不确定的块级元素居中.png](https://upload-images.jianshu.io/upload_images/8661291-3a4c3e7b4f4884de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
div{
  border: 1px solid;
  display: inline-block;
}

body{
  text-align: center;
} 
```
html是没有变化的，就没有写出来。要点在**我们将块级元素变成inline-block**，这个属性直接让块级元素由扩张变成了收缩，**相当于一个内联元素**。那这个问题就变成了内联元素怎么水平居中，是不是刚刚才解决的问题，答案是在父元素身上加text-align: center;，完美解决！

<hr>
##垂直居中

###设置line-height 和padding居中方式

![单行文本垂直居中.png](https://upload-images.jianshu.io/upload_images/8661291-78749ee9564027ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
<body>
  <span>blog</span>
</body>
```
```
*{
  margin: 0;
  padding: 0;
}
span{
  background-color:red;
  padding: 20px;
  line-height: 20px;
  display: inline-block;
}
```
要点：不要先去限定span的宽高，这样容易出bug，用内部元素撑开父容器，代码里面**line-height或者padding**都可以达到居中效果，而且可以再添加文字。

###table-cell的垂直居中方式

![table-cell.png](https://upload-images.jianshu.io/upload_images/8661291-80beea38903bb8bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
<body>
  <div>
     <span>多行文本多行文本多行文本多行文本多行文本多行文本多行文本多行文本多行文本</span>
  </div>
</body>
```
```
*{
  margin: 0;
  padding: 0;
}
div{
  width:200px;
  display: table-cell;
  border: 1px solid;
  height:200px;
  vertical-align: middle;
}
span{
  background-color:red; 
}
```
要点：在父容器中加上两句代码**display: table-cell; vertical-align: middle;**不管是单行文本还是多行都可以居中，而且块级元素同样生效。
![table-cell_块级元素.png](https://upload-images.jianshu.io/upload_images/8661291-03e7c96c0d159432.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
<body>
  <div class="parent">
    <div class="child">我是块级元素我是块级元素我是块级元素我是块级元素我是块级元素我是块级元素</div>
  </div>
</body>
```
```
*{
  margin: 0;
  padding: 0;
}
.parent{
  width: 300px;
  height: 100px;
  display: table-cell;
  vertical-align: middle;
  background-color: yellow;
}
.child{
  background-color: red;
}
```
**缺点：父容器不是固定宽高就会改变父容器的宽度，我原本设置的width：50%；不能起作用。**
##### 总结：只要是固定宽高的父容器，用table-cell可以完美解决，不论其实行内元素或者块级元素。
<br>

## 绝对定位垂直水平居中
这个也是相当有效，并且是常用的方式，特别是你需要**覆盖其他元素**的时候。
只看块级元素的适用性：
![绝对定位.png](https://upload-images.jianshu.io/upload_images/8661291-0f21cce83d63d8fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
<body>
  <div class="parent">
    <div class="child">我是块级元素我是块级元素我是块级元素我是块级元素我是块级元素我是块级元素</div>
  </div>
  </body>
```
```
*{
  margin: 0;
  padding: 0;
}
.parent{
  width: 50%;
  height: 200px;
  background-color: yellow;
  position: relative;
}
.child{
  background-color: red;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%)
}
```
要点：父元素有**position: relative;**相对定位才能定位准确，子元素left和top确定子元素相对于父元素的位置，然后子元素 transform: translate(-50%,-50%)；微调。当你要选择是子元素还是父元素在上方时，需要再加上[z-index](https://developer.mozilla.org/zh-CN/docs/Web/CSS/z-index)。行内元素垂直水平居中方法相同，不赘述。

<br>
## flex 弹性布局垂直水平居中
![flex垂直居中.png](https://upload-images.jianshu.io/upload_images/8661291-19ec184ff219af8a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
<body>
  <div class="parent">
    <div class="child">我是块级元素我是块级元素我是块级元素我是块级元素我是块级元素我是块级元素</div>
  </div>
  </body>
```
```
*{
  margin: 0;
  padding: 0;
}
.parent{
  width: 60%;
  height: 200px;
  background-color: yellow;
  display: flex;
  align-items:center;
  justify-content: center;
}
.child{
  background-color: red;
}
```
要点：记住3句代码 **display: flex;align-items:center;justify-content: center;给父容器**无副作用，行内元素和块级元素都适合。
<hr>

#总结
布局主要是横向布局和纵向布局，布局方式主要是把复杂的页面通过容器包裹的方式全部转化成纵向和横向，再用float、padding、margin和决定定位等方式布局内部的元素位置。行内元素一般用padding、line-height就可以完成布局，块级元素如果在固定宽高的父容器内，可以使用display:table-cell的方式布局，如果要覆盖某些元素使用绝对定位，flex布局是最万金油的选择，也最简单。