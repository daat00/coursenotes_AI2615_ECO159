CSS 预处理器 - SASS/SCSS, LESS
===

240922 - css 工程化
https://www.bilibili.com/video/BV1Ci4y1d74K/

[TOC]

## CSS 预处理器的由来

### 为什么会出现 CSS 预处理器？

CSS 不是一种编程语言，仅仅只能用来编写网站样式。

在 Web 初期时，网站的搭建还比较基础，所需要的样式往往也很简单。但是随着用户需求的增加以及网站技术的升级，CSS 渐渐不再满足于项目。例如，同一站点下不同 html 所需要的 css 并不一定会合在一起；组件化编程时也可能引入多个 css。如果 css 多文件，那么维护起来会比较困难。

没有类似 JavaScript 这样的编程语言所有的变量、常量以及其他的编程语法，CSS 的代码难免会显得臃肿以及难以维护。但是又没有 CSS 的替代品，于是 CSS 预处理器就作为 CSS 的扩展，出现在了前端技术中。

## CSS 预处理器（Sass/SCSS、Less、Stylus）是什么？

CSS 预处理器用一种专门的编程语言进行 Web 页面样式设计，然后再编译成正常的 CSS 文件以供项目使用。CSS 预处理器为 CSS 增加一些编程的特性，无需考虑浏览器的兼容性问题，可以自动添加兼容性前缀，如 -webkit。

常见的预处理器有 Sass、Less 和 Stylus。特别地，Sass-version3 又被叫做 SCSS，他们是一个东西。


#### SASS
SASS 由 Ruby 语言的程序员设计，语法与 Ruby 接近。在 scss(sass3) 之前，靠缩进区分代码块；scss 改进为了花括号和分号。

在 sass 文件中编写 sass 代码，编译器会将其转换为 css 代码。依赖 ruby

#### Less
Less 是另一种动态样式语言，语法受 sass 影响。Less 可以在浏览器环境下引入 less.js 动态编译，也可以通过 node 运行于服务端

#### Stylus
产生于 node 社区，依赖 node

#### SASS, Less 的变量赋值风格
```
Sass
$color: #00c;

Less
@color: #00c;

Stylus
mainColor = #00c;
```

## 环境
vscode - live  sass compiler
用它跑 scss

#### 生成格式
- nested
- expanded
- compact
- compressed

其中 expanded 和 compressed 用的最多。
压缩不会替换 class 名称。只是调整缩进、空格和空行

## sass 语法
### recap: 后代选择器
在 css 中，`.parent .child` 选择器指定了 class="child" 且祖先包含 'parent' 的元素的样式。

在 sass 中，可以用 `.parent{.child}` 表示同样的功能

### & 操作符
`&` 操作符表示父选择器。例如



```scss=
.container{
    &:hover{
    /* 表示 .container:hover */
    }
    &-md{
    /* .container-md */
    }
}
```

### 属性嵌套

例如 font-size, font-family, font-weight 可以简化为
```scss
// font-size: 14px;
font: {
    size:
    family:
    weight:
}
```

#### @at-root
`@at-root {}` 可以提升大括号内的内容到 root
```scss=
.foo {
    & { // ==> @at-root &
        font-size: 12px
    }
    & .bar { // @at-root .bar
    }
    .bar & { // .bar .foo
    }
}
```
默认情况下，`@at-root` 不能跳出 @media 和 @support


### % 占位符选择器
带有 % 的类仅在被 @extend 使用的时候会被编译

```scss=
.button%base{
    // some styles
}
.btn-success{
    @extend %base;
}
```


## 变量 
### css 变量
```css=
/* 全局可以访问 */
:root{
    --color: #F00;
}
/* body 内访问 */
body{
    --border-color:  #f2f2f2;
}
/* header 内访问 */
.header{
    --background-color: #f8f8f8;
}

.usage{
    color: var(--color)
}
```

### scss 变量
变量的定义以 $ 开头

```scss
$border-color: #f00
$border_color: #f00 /* 相同变量 */
```

#### scss 变量作用域

类似 c/c++，局部变量不超过大括号
类似 python，可以通过 !global 声明全局变量


#### scss 变量类型
```scss=
// 数字类型
$layer-index: 10; // 10px
// 字符类型
$font-base-family: unquote('Open Sans'); // 也可以无引号 Sans-Serif
// 颜色类型
$top-bg-color: rgba(255,147,29,0.6); // #fa0000
// bool
$blank-mode: true;
// null
$var: null;
// 数组：用空格或逗号分隔
$padding: 6px 10px 6px 10px
// maps：相当于 JS Object
$color-map: (color1: #a00, color2: #b00)
```

#### scss 默认值
在大型项目，有时候你不知道别的地方有没有定义过一个变量。此时可以通过 !default 声明默认值，若变量未定义，则使用该值



## 导入其他 css
### import 导入
```scss=
// css
@import url(other.css); // 用这种方式导入视作普通 css 导入，scss 的变量不会被解析。

// scss
@import 'public'; // .scss 可省略
```
如下的方式都将被视作普通 css。
- 文件拓展名是 .css
- 文件名 startwith 'http://'
- 文件名是 url()
- @import  包含媒体查询

> import 导入的变量都位于全局作用域，重名会覆盖；多次导入同一个文件会引入两次样式

#### 局部文件
通过 @import 导入一般的 scss 文件后，编译时会分别生成一个对应的 css 文件

但如果被 import 的 css 文件以 '_' 开头，则会被当做局部文件，不会生成 css，只会导出变量。

有了局部文件，scss 可以实现模块化。

### use 导入
类似 python 的 import，导入的变量被保存在以文件名命名的模块内部；不会重复导入同一个文件

```scss=
@use 'uses' as * with($var: 10px) // 全局作用域
```

#### 默认导入 -index.scss
为了便于管理，可以在文件夹下放置 _index.scss 导入文件夹下其他文件。导入时只需要导入文件夹即可。

### @forward
在默认情况下，@use 导入的变量只能在当前文件生效，不可再导出到子文件

可以配合使用 @forward 实现类似 es6 的 export 的功能，将内容导出。

```scss=
@forward 'common' as c-* ; // c-* 给 'common' 下的所有变量添加前缀
@forward 'common' as c-* hide c-bgColor with ($fz:70 !default); // 隐藏变量暴露范围，设置初始值
@use 'common'; // use 推荐放到 forward 下面，似乎和 with 初值的顺序有关
```

## sass 混入
类似函数。@mixin 可以定义一个混入，之后用 @include 调用即可
```scss=
@mixin linear-gradient($direction: 'to right', $gradients...) {
    background-color: nth($gradients, 1); // 获取数组的第一个元素
    background-image: linear-gradient($direction, $gradients);
}


.usage {
     width: 200px;
        height: 200px;
        @include linear-gradient(to right, red, blue); // 也可以用 kwargs
}

/* 扩展一条CSS语句优于创建一个mixin，
   这是由Sass组合所有共享相同基样式的类的方式决定的。
   如果使用mixin完成，width, height, 和border将会在
   调用了该mixin的每条语句中重复。虽然它不至于会影响你的工作流，
   但它会在由Sass编译器生成的的文件中添加不必要的代码。*/
```

## sass 函数
@function{@return}.语法和混入很像，支持解压运算符 ... 传参

## sass 继承 extend
在设计网页时经常会遇到的一种情况：第一个 class 指定了基础 style；第二个 class 指定细节样式。例如：btn
![](https://notes.sjtu.edu.cn/uploads/upload_35bc1c81d53bbb4c85d9d25375ebc14b.png)

此时，既可以将样式设计为 `class="btn btn-primary"` 的形式，也可以设计为 `class= "btn-primary"` 的形式。为了实现后者，所有 btn-x 需要继承 btn。可以通过 @extend 实现
```scss=
.btn {
    // some style
}
.btn-primary {
    @extend btn;
}
```
scss 编译器在实现 extend 时，会把基类代码块声明为并集选择器 .btn, .btn-primary，而非复制所有的内容。如果 btn 是不需使用的，可以用 % 占位符开头声明 %btn 类，这样该类不会出现在最终代码里


## sass 插值语法
类似于宏，可以规避掉一些语法的限制
```scss=
a.#{$class-name} {
    border-#{$attr}: #f00;
    /* 特殊地，多行注释会出现在目标 css 中；#{$author} */
}
```

## 预定义函数
略。语法一坨屎看文档去吧


## sass 分支
@if，@else    

通过内置函数 if 实现了三元运算符 `if(<cond>, <texp>, <fexp>)`

## sass 循环
使用 @for $varname 实现循环。下面的例子实现了过场动画

```scss=
@keyframes loading {
    0% {
        opacity: 0.3;
        transform: translateY(0px);
    }

    50% {
        opacity: 1;
        transform: translateY(-20px);
        background: green($color: #000000);
    }

    100% {
        opacity: 0.3;
        transform: translateY(0px);
    }
}

#loading {
    position: fixed;
    top: 200px;
    left: 46%;
}

#loading span {
    position: absolute;
    width: 20px;
    height: 20px;
    background: #3498db;
    opacity: 0.5;
    border-radius: 50%;
    animation: loading 1s infinite ease-in-out;
}


@for $i from 1 through 5 {
    #loading span:nth-child(#{$i}) {
        animation-delay: 0.2s * $i;
        left: 20px * $i;
    }
}
```

除此之外，还有 @each, @while

## scss 编译器轶闻
原本有三个编译器 dart sass, libsass, ruby sass. 

时至今日，只有 dart sass 还活着了。新版插件集成了 dart sass 编译器，旧版的没有


