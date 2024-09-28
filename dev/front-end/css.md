
# CSS
## 盒模型 - 块级元素
### 块级元素（Block-level Elements）
块级元素会在页面上单独占据一行，通常从新行开始，宽度会自动填满父元素的宽度。
常见的块级元素包括 `<div>、<p>、<h1>、<ul>、<ol>、<li>` 等。
默认高度由内容撑开

### 内联元素（Inline Elements）
内联元素不会在页面上单独占据一行，它们会在同一行内依次排列，宽度和高度均由内容决定。
常见的内联元素包括 `<span>、<a>、<strong>、<em>、<img>、<input>` 等。

行内元素的上下 padding 可能不占位，左右正常

### 内联块级元素（Inline-block Elements）：
内联块级元素表现类似于内联元素，但可以设置宽度、高度以及上下外边距和内边距。
常见的内联块级元素包括 `<div style="display: inline-block;">、<span style="display: inline-block;">、<img>` 等。

![](https://notes.sjtu.edu.cn/uploads/upload_b7f3a15976334dc389caffd3ceae24fb.png)

## 属性的继承
![](https://notes.sjtu.edu.cn/uploads/upload_1e39cb90a05ccea3a19b0995b780fa06.png)

#### 像素与系统缩放
Windows 的缩放会放大浏览器的 px. 100px -> 125px

#### px, em, rem
em: 相对于当前元素 font-size
rem: 相对 root 元素(`<html>`) 的 font-size
特别地，声明`foot-size: 1em` 会使用父元素 font-size


#### margin 塌陷
第一个和最后一个元素的外侧 margin 会被交给父元素。可以设置 padding/border/overflow 解决

#### margin 合并
两个元素之间的 margin 取二者最大，而非加起来
- 只存在于垂直方向，不存在于水平方向




#### chrome 调试工具如何查看 css 来源
![](https://notes.sjtu.edu.cn/uploads/upload_71104cba8ac2629a38c60fe018dbbbd1.png)


#### h 标签的默认样式
![](https://notes.sjtu.edu.cn/uploads/upload_0ecaed05991473f77220ac800327c102.png)


## 浮动
最早期用来实现文字环绕图片的布局
### 语法
`float:left`
![](https://notes.sjtu.edu.cn/uploads/upload_01b2149c06d5f16bb1f26efd6a88434a.png)

浮动元素会脱离文档流，宽高由内容撑开（可覆盖）。
可以和其他元素公用一行，文字自动错开
脱离文档流的元素之间，父子关系仍然保留


#### 清除浮动的影响
![](https://notes.sjtu.edu.cn/uploads/upload_7e8efcfee1a5262fd4452c993cb88959.png)


![](https://notes.sjtu.edu.cn/uploads/upload_8c03e03f1d6860ef3afe6edc95b01d7c.png)


实践中，常定义 leftfix, rightfix 和应用到 parent 上面的

### 用浮动布局
#### 布局的历史
1. 在最早期，用表格
2. 浮动在解决文字环绕后，被用来布局
3. 用 flex 弹性盒子


#### 代码风格
一个父元素的所有子元素要么都是 float 要么都不是
![](https://notes.sjtu.edu.cn/uploads/upload_29db594786500dcba1673e485c4c3565.png)


#### video 标签和 audio 标签

## 定位
定位的元素图层层级比普通元素高


### 相对定位
相对于元素原来的位置偏移

元素开启相对定位后不脱离文档流（在排版元素位置时，仍在原位，只是显示换了个位置。有点类似于 $du - dx$）

仍然可以浮动 & 添加 margin

### 绝对定位
开启绝对定位后，元素脱离文档流。（与 float 的脱离文档流不同，文本不会单独出现环绕效果）

定位参考点：包含块。
- 对于没脱离文档流的元素，父元素就是包含块
- 对于脱离文档流的元素，第一个开启定位（包括相对、绝对）的祖先是包含块

因此，常用子元素绝对定位，父元素开启0的相对定位

尽可以添加 margin 不可以浮动。



### 固定定位
定位参考点为视口的左上角。（注意：视口不等于 html 标签，下拉滚动条后，视口为当前显示范围，html 标签上去了）
- 脱离文档流
- 浮动失效

### 定位元素
与块元素、行内元素相似的概念。开启绝对定位和固定定位后，不论什么元素都会变为定位元素。定位元素可以设置宽高。

### 粘性定位
实现的效果：下拉滚动条，一个 div 悬停在顶端。

参考点是离他最近的一个有滚动条的元素
- 不脱离文档流

## 伸缩盒
解决 float 元素会脱离文档流，导致父亲、兄弟元素显示异常的问题。

flex 元素仍在文档流中。

f12 中会变成紫色

实际中仅在移动端用 flex 比较多，pc 端 float + 定位写顺了
### 弹性容器
![](https://notes.sjtu.edu.cn/uploads/upload_e3f37c34ceab46644b4c868cc88bf43e.png)

### 弹性项目


### 主轴侧轴
![](https://notes.sjtu.edu.cn/uploads/upload_c046351dacaf5784c4cc986a897d2336.png)

主侧轴有不同的换行对齐方式

#### 主轴对齐
设置弹性项目沿主轴是否居中，等
![](https://notes.sjtu.edu.cn/uploads/upload_b33bf199cb657edc6b6b55e62de62ff9.png)


（注：换行方式也是可以调的）

#### 侧轴对齐
![](https://notes.sjtu.edu.cn/uploads/upload_b0251c0906f7c4b7a077bcf6feae10e8.png)

#### 水平垂直居中
![](https://notes.sjtu.edu.cn/uploads/upload_40ad7ee82e21a8c4f37616a257b28b0c.png)

#### 伸缩容器拉伸
![](https://notes.sjtu.edu.cn/uploads/upload_0f4a84f1c7611bb37f6889f95a9d5a53.png)

#### 伸缩容器压缩
首先 flex-wrap 不能换行，设置成 no-wrap

#### 合并属性：flex
![](https://notes.sjtu.edu.cn/uploads/upload_72d8fa5f25c56cd435c9c160ae2d57af.png)

![](https://notes.sjtu.edu.cn/uploads/upload_730efe522223033d759c56258162752c.png)


## 媒体查询
当满足 @media 后列出的条件时，应用对应的 style。

![](https://notes.sjtu.edu.cn/uploads/upload_5b2ecad25a9ec5716a9e36197f237b53.png)

### 查询设备类型

### 查询视口宽度
（一般不需要查询设备的高度，由竖直下拉条解决）


### 查询设备分辨率
与视口不同


## 响应式布局
根据设备分辨率宽度修改页面布局

#### 常用阈值

![](https://notes.sjtu.edu.cn/uploads/upload_d745c42181f599b032e926211c121868.png)

可以理解为，定义多个不同的版心宽度



## Vite 构建流
### 拆包策略
通过指定 manualchunk 来避免所有 chunk 被打包到一起。
https://cloud.tencent.com/developer/article/2357991