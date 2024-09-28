前端设计稿
===

#### 最早期设计图
一张 jpg
![](https://notes.sjtu.edu.cn/uploads/upload_449d17c675ea83fe9e2a5f36aa459e60.png)

#### 现代工具
前端页面设计工具

找到的解决方案
- Midjourney + MasterGo
- ps 的插件，导出静态 html


## Components
以 jd 为例

开发时一般先用纯色填充。
#### .container 版心
宽度一般 960-1200

整个页面的 container 一般是同一的（对应最大宽度）

无高度

#### top bar 顶部导航栏
导航栏，top nav bar。一部分左对齐一部分右对齐

横着的列表，如

#### 搜索框
是一个 form 表单，左边一个搜索框 + 右边一个 button。button 内部 1. 放一个放大境的 bg-img 2. 嵌套一个 img 标签

基线对齐问题：由于字体设计问题，存在文字时文字竖直方向不一定居中。对齐

清除间距：设置父元素的 font-size = 0，子元素会自动继承父元素。注意，对于搜索框，子元素有默认的 font-size，所以不会也清零。

#### 二级菜单（鼠标放到侧边导航栏之后显示）
1. 定位：相对定位 + 绝对定位
2. 显示：一开始是 display: none，然后在 `parant:hover .second-menu` 选择器里打开即可（相当于每个 class 有 hover 和非 hover 两个 node，先把两个 node 都 none 掉，最后再 display 有 hover 的）

可以理解为，是把二级菜单和触发按键看成一个整体，一起 hover