---
layout: css3
title: clip-pash剪个三角形
date: 2018-10-10 04:48:09
tags:
---
>今天这篇博客主要是探究一下CSS3中clip-pash用法，也是我学习前端快一个月的小小足迹吧！

*本来是想把元素居中这个问题拿来好好说说，不过正好遇到就写写*
#怎么遇到这个用法的
![需求.png](https://upload-images.jianshu.io/upload_images/8661291-b535688e7eba7d06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
就是将图片中的三角形底边和矩形重叠的这部分去掉。
![需求end.png](https://upload-images.jianshu.io/upload_images/8661291-f21c0f9512dee725.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我当时能想到的办法就是将我设置的伪元素背景变成白色，但是这样会挡住我在文本框里的文字。![完成办法1.png](https://upload-images.jianshu.io/upload_images/8661291-da97f7c20cd82729.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这段的CSS如下：
```
 .container3::before{
    content:"";
    display: block;
    width: 25px;
    height: 25px;
    background:white;
    border-top: 2px solid #ccc; 
    border-left: 2px solid #ccc;
    border-bottom: 2px solid transparent;
    border-right: 2px solid transparent;
    position: absolute;
    top: -14px;
    left: 15px;
    transform: rotatez(45deg);
  }
```
这时候是完成了样式，不过如果你想写文字就会被挡住，想必也不是我们想要的。我想了个笨办法，那就是再添加一个标签，这里面写入文字，使标签叠层在文本框之上。所以再写一个<span>在文本框之下，作为其子元素，因为文本框是绝对定位的，所以这个新标签也绝对定位，就解决了文本问题。

# 有没有更炫酷的办法呢？
我咨询了下若愚大大，给的建议是**CSS3 clip-pash**。
## 什么是clip-pash？
>clip-path属性可以创建一个只有元素的部分区域可以显示的剪切区域。区域内的部分显示，区域外的隐藏。剪切区域是被引用内嵌的URL定义的路径或者外部svg的路径，或者作为一个形状例如[circle()](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/circle).。clip-path属性代替了现在已经弃用的剪切 [clip](https://developer.mozilla.org/en-US/docs/Web/CSS/clip)属性。

clip的英语是剪切，pash为路径。根据定义，大致可以知道两件事：
1. clip-pash作用的区域是分为可见和不可见两个部分的；
2. 这个功能和某种路径有关系。

## 实现功能
根据[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clip-path)的介绍，还有其他谷歌博客的介绍，大致找到4种写法：
```
.clip-me {
clip-path: inset();
clip-path: circle();
clip-path: ellipse();
clip-path: polygon();
}
```
其中inset是矩形的剪切，circle是圆形的剪切，ellipse是椭圆的剪切，polygon是多边形的剪切。对于我们想要把伪元素这个矩形剪切成三角形，应该使用polygon这个语法。

在polygon的（）里是我们在平面中定位的点，系统会自动地把这几个点按照顺序连接起来，并剪切。例如我们要剪切的这个例子：
![坐标图.png](https://upload-images.jianshu.io/upload_images/8661291-346672dde878e14e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

红色区就是我们要剪切的这部分，坐标分别是（0,0），（25,0），（0，25）。**因为我们没有设置初始点，所以默认从左上角为（0,0），另外这个和数学坐标系是不同的**
所以代码如下：
```
.container3::before{
    content:"";
    display: block;
    width: 25px;
    height: 25px;
    background:white;
    border-top: 2px solid #ccc; 
    border-left: 2px solid #ccc;
    border-bottom: 2px solid transparent;
    border-right: 2px solid transparent;
    position: absolute;
    top: -12px;
    left: 15px;
    transform: rotatez(45deg);
    -webkit-clip-path: polygon(0px 0px, 0px 25px, 25px 0px);
    clip-path: polygon(0px 0px, 0px 25px, 25px 0px); 
  }
  ```
*-webkit-是因为有些兼容问题，所以有时候必须带上。*
最后out出来也能实现了效果。

##其他
- 在caniuse.com上查找，发现兼容性实在很成问题，ie上全部不支持这个语法，所以在使用场景中需要慎重。
- inset是矩形剪切，括号里有四个值，剪切后的上边框离原图上边框的距离，剪切后的右边框离原图右边框的距离，剪切后的下边框离原图下边框的距离，剪切后的左边框离原图左边框的距离。
![inset.png](https://upload-images.jianshu.io/upload_images/8661291-4f599493edc299b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- circle中默认是从图形中心为初始点，后面（）里面可以写像素，也可以写百分比。像素应该是半径或者直径的长度，百分比不太确定。
- ellipse是椭圆形的剪切方式，暂时还有需要研究。
- 同时感谢若愚大大给我介绍这个用法，不然我都不知道。

参考的链接：
[CSS3 clip-path polygon图形构建与动画变换二三事](http://www.zhangxinxu.com/wordpress/?p=4732)
[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clip-path)



