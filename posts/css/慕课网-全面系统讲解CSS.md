# 前言

系统讲解CSS之前，需要先了解下HTML跟CSS的基本知识。


<br/>

# HTML

## H5的理解

H4之前，开发者非常随意，标签名、属性名使用大写，标签没有闭合等，W3C认为这样过于随意不行，所以推出了XML。

XML 的要求非常严格，标签名、属性名不允许大写，标签必须闭合等。

后面开发者又发现，太严格也不好，所以就有了HTML5，HTML5的要求又降下来。

<br/>
<img src='https://github.com/jiangxia/FE-Knowledge/raw/master/images/180.png' width='800'>
<br/>

H5新增内容，主要包括语义化标签、表单增强、供JS调用的新特性，比如定位、存储等。

<br/>

## HTML元素分类

按默认样式分：
- 块级 block
- 行内 inline
- inline-block

按内容分：

<br/>
<img src='https://github.com/jiangxia/FE-Knowledge/raw/master/images/181.png' width='600'>
<br/>

<br/>

## HTML元素的嵌套关系
- 块级元素可以包含行内元素
- 块级元素不一定能包含块级元素，比如p不能包含div
- 行内元素一般不能包含块级元素

关于第三点，我们可以联想到a包含div的场景。

a包含div是否合法？不一定，取决于外面包裹的元素，a是一个transparent content model，也就是计算时，会把a当成透明的，所以取决于外层包裹的元素。

以下就是不合法的。

```html
<p><a><div>测试</div></a></p>
```

因为a被当成透明，所以结果是

```html
<p><div>测试</div></p>
```

<br/>

## HTML面试题

### doctype 的意义是什么？

- 让浏览器以标准模式渲染
- 让浏览器知道元素的合法性

### HTML、XHTML、HTML5的关系

- HTML属于 SGML
- XHTML 属于 XML，是HTML进行XML严格化的结果
- HTML5不属于SGML或XML，比XHTML宽松

### HTML5有什么变化？

- 新的语义化元素
- 表单增强
- 新的API，如离线、音视频、图形、实时通信、本地存储、设备能力
- 分类与嵌套变更

### em与i有什么区别？

- em是语义化标签，表强调
- i是纯样式的标签，表斜体
- HTML5中i不推荐使用，一般用作图标

### 语义化的意义？

- 开发者容易理解
- 机器容易理解结构（搜索、读屏软件）
- 有助于SEO

### HTML跟DOM的关系

- HTML是“死”的
- DOM由HTML解析出来，是活的
- JS可以维护DOM

### property和attribute的区别

- attribute是“死”的，也就是写在HTML中的属性
- property是“活”的，是解析出来的特性

例子：
```html
<input type='number' value='1'/>
```

value是HTML上的一个attribute，是死的。

浏览器拿到这段代码，会解析成DOM元素，property是DOM上的特性，是活的。

我们可以在浏览器控制台执行以下代码

```js
$0.value // $0指向input元素，value是input元素上的property
"1"
$0.value=2 // 改变input元素上的property，页面上输入框的值会改变，但浏览器Element面板上，input代码上的value还是1
2
$0.getAttribute('value') // 要想拿到attribute值，可以使用getAttribute。注意，此时页面输入框的值已经是3，但attribute值仍为1
"1"
$0.setAttribute('value',5) // 要想改变attribute值，可以使用setAttribute，此时将attribute值设置为5，但页面输入框的值仍为3
```

可见，attribute是HTML的属性，而property是DOM上的特性，HTML是“死”的，所以attribute也是“死”的，渲染出DOM元素后，就跟DOM元素没有关联。

而DOM元素是活的，property也是活的，当我们改变property的值时，页面输入框的值也会跟着改变。

<br/>

# CSS

## CSS 选择器权重问题

class的权重是10，id的权重是100。

如果有11个class选择器，权重是110，是否意味着比一个id选择器的权重大？

答案是否定的！css选择器的权重计算是不进位的。可以理解为“官大一级压死人”。id选择器的权重就是比class大，class权重的计算值是不进位的。

也就是说，十位虽然是11，但不会进位，所以百位仍是0.

此外，还有一些特殊场景，也就是内联样式、!important、id选择器的优先级。他们的优先级是：!important > 内联样式 > id选择器。

<br/>

## 非布局样式

- 文字相关：字体、字重、颜色、大小、行高、换行
- 盒子相关：背景、边框
- 页面相关：滚动
- 装饰相关：粗体、下划线、斜体

### 文字

#### 字体族

一共有5种字体族，分别是： serif (衬线字体) 、 sans-serif （非衬线字体）、 monospace （等宽字体）、 cursive （手写体） 、 fantasy （花体）

设置字体时，可以同时设置多种字体，如果一个字体找不到，就会往后找下一个。这就是**多字体的fallback机制**。

#### 行高

也就是line-height，它不会影响inline box的高度，但可以决定空余空间在inline box上下的均分。所以可以用line-height实现垂直居中。

请看[例子](https://github.com/jiangxia/FE-Knowledge/blob/master/code/line-height.html)

#### 文字折行

- overflow-wrap（word-wrap）是否保留单词
- word-break 是否把单词看成一个单位。（keep-all表示把单词、中文句子看成整体，break-all表示不把单词看成一个整理）
- white-space 空白处是否断行。设置nowrap表示不换行

### 盒子

#### 背景

如果想给一个盒子同时设置背景图片跟渐变色背景，可以这样做：

```css
.class{
  background: url(./test.png) 174px center no-repeat,linear-gradient(to bottom,red,green);
}
```

CSS3 为 background 增加了一些属性，其中需要重点关注的是：background-origin、background-clip、background-size: contain;

- background-origin设置图片的定位，即图片左上角的位置(从哪开始显示)，跟background-position配合。
- background-clip设置图片显示哪部分区域，可以实现背景图片的裁剪(显示哪)，例如可以设置图片从边框左上角进行定位，但是针对内容区域进行裁剪
- background-size: contain;背景图像可以完整显示，但可能不能完全覆盖背景区域
- background-size: cover;背景图像可以完整覆盖背景区域，但是图像可能显示不完整

更多资料，看[这里](https://blog.csdn.net/m0_37617778/article/details/84988372)

<br/>

## CSS 布局