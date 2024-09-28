黑马 Java
====
https://www.bilibili.com/video/BV1m84y1w7Tb


[TOC]

## Web 基础
### HTML
其他的基本都会，参考 bootstrap 画前端页面。注意版心啥的

新知识：表单项标签 `input`, `select`, `textarea` - 
（仅在包裹于父级 form 标签时），表单会自动收集标签内的所有 `input`, `select`, `textarea` 子标签的数据，按照 name 分组并按 method, action 指定的操作发送 http 请求。

```htmlmixed=
<!-- 触发 form 提交 -->
<input type='submit'/>
<button type='submit'/>
```




### JS
#### 对象 object
成员函数可以省略 function 关键字，如
```javascript=
{
    data: {},
    mounted(){
      // some function  
    }
}
```



#### BOM
BOM 的组成：
- window
- navigator
- screen
- history
- location

#### DOM


### Vue

MVVM 模型：Model - View - ViewModel。
由 vue 实现 ViewModel 层，实现双向数据绑定

触发：
```htmlmixed=
<div id='app'>
    <a :href="url"></a>
</div>

<script>
    new Vue({
        el: "#app",
        data:{
            url: "//baidu.com"
        },
        // router 路由表配置
        // router 是一个插件
    })
</script>
```



#### v-bind, v-model
- 单向绑定：v-bind
- 双向绑定：v-model


#### v-on
`@click`, `@blur`, `@focus`...

#### v-if, v-show

#### v-for
`v-for="(addr, index) in addrs"`


### AJAX
#### xhr
参考 https://www.w3school.com.cn/js/js_ajax_intro.asp

#### axios
基于 promise 封装了原生 xhr

```javascript=
axios({
    method: "get",
    url: ""
}).then(()=>{})
axios.get
```




## 前后端分离工程化
以接口文档为核心，先根据需求文档和项目原型指定一个接口文档，然后分别按这个接口文档做开发。

![](https://notes.sjtu.edu.cn/uploads/upload_bcf2afad8507da0f697987c2022ceb05.png)

### 接口文档
例如 

https://github.com/fython/BilibiliAPIDocs/blob/master/API.feedback.md

#### 维护接口文档
YAPI



### vue
#### 标准目录结构
![](https://notes.sjtu.edu.cn/uploads/upload_541d77ab4b0c3d14392039a5c7a1c62b.png)



脚手架通过 ws 实现热重启 hot-reload, 即 server 触发前端重启

#### node.js
js 运行时环境（类比 JRE）

npm 包管理器


#### 组件库 - Element
在实际开发中，从组件库中找到需要的组件，然后复制粘贴即可。


#### 打包部署
1. build 打包产生 dist
2. 将 dist 生成的静态文件部署到 nginx 的 http 目录下



# 后端 web 开发
## Maven
- 依赖管理（自动导入 jar 包）：依赖配置于 pom.xml，自动联网下载 externel library
- 统一的项目结构
- 跨平台自动化项目构建





<img src="https://notes.sjtu.edu.cn/uploads/upload_0b3e4dd5d910cc17f50c38a65b32d1b8.png" width=50%></img>




maven 通过各种插件来完成项目的编译、测试、打包等标准功能，产生 class, jar 文件

pom.xml 描述了项目的 POM(Project Object Model)

- POM 的`坐标` 包括 groupId, artifactId, version
- POM 中可以添加依赖模型
- groupId 对应文件夹树

<img src="https://notes.sjtu.edu.cn/uploads/upload_77f0977ad21f7695b3328176c80b03f0.png" width="50%" class="center"></img>

![](https://notes.sjtu.edu.cn/uploads/upload_97969ff000f09092325e14b698f91e69.png)


### Maven 仓库
- 本地仓库（类比本地 git）
- 中央仓库（类比 github）
- 远程仓库（proxy）


### maven 依赖 scope
![](https://notes.sjtu.edu.cn/uploads/upload_7005f954594dfacfbe879741de69514a.png)


### maven 生命周期（常用命令）
![](https://notes.sjtu.edu.cn/uploads/upload_e04dc2de62a632cf34d46c3a9a4abb0f.png)

在每套生命周期内部，执行后面的命令会自动执行前面的。







## Tomcat
Apache 旗下的 web server。支持 servlet, JSP 等少量 Java EE 规范

Tomcat 遵循标准目录结构


JSP 需要跑在 servlet 容器（web 容器）上，例如 Tomcat

#### Java EE 规范
Java 企业级技术规范。有 13 项技术规范：JDBC, JNDI, EJB, RMI, JSP, Servlet, XML, JMS, Java IDL, JTS, JTA, JavaMail, JAF



## Spring
spring 全家桶提供了几乎绝大多数应用场景的雏形框架
- spring framework 是所有 spring 子项目的基础底层框架
- spring framework 配置繁琐，不好上手。springboot 简化了配置，以便快速开发


### quickstart
https://start.spring.io/

1. 通过 idea 引导创建 springboot 工程，勾选 web dev kit
2. 定义 controller，引入 RestController 和 RequestMapping 注解，类似 Flask
3. 运行测试


### 起步依赖
- parent(springboot-starter-tomcat, (version info), ...)
- springboot-starter-web 
- springboot-starter-test


#### dispatcherController 接口
tomcat 只识别 servlet。springboot 底层提供了核心 servlet 程序 dispatcherServlet.

DispatcherServlet 实现了 servlet 接口，被称为核心控制器或前端控制器

![](https://notes.sjtu.edu.cn/uploads/upload_4f5660394d63c28aa445b40cc9e6981d.png)

tomcat 在处理 http 请求后，会将解析结果封装到 HttpServletRequest 对象中。

返回的结果应当放到 HttpServletResponse 对象中。

![](https://notes.sjtu.edu.cn/uploads/upload_9d0a6fc983701d37c410f6ddbe1fa225.png)


#### 获取参数
原始方法

![](https://notes.sjtu.edu.cn/uploads/upload_1068fc14857fead6e4029ab0d5cb78ce.png)


springboot 方法

在方法的形参列表里，声明与请求参数一致的形参变量名即可。框架会自动解析变量和类型。
（特别地，多选表单会给同一个 key 传递多个值，如 k=1&k=2&，可以用数组 String[], 集合 List\<String\> 接收参数）

![](https://notes.sjtu.edu.cn/uploads/upload_4a8a7d1dfbdbfea678a1729acf5635ce.png)

也可以添加注解，使得形参名和请求参数可以不一致。

![](https://notes.sjtu.edu.cn/uploads/upload_d84f7332e7d09d27f3b5d7b45de9e292.png)


还可以将请求参数的格式封装到一个 POJO 类中，形参只有一个即可。（支持类型的嵌套）

![](https://notes.sjtu.edu.cn/uploads/upload_cfc25389d353200219894ca9493efa91.png)

特殊类型：日期，json，路径参数。 有特殊注解


#### 返回参数 ResponseBody
ResponseBody 是类注解 + 方法注解
- @RestController = @Controller + @ResponseBody


添加了 @ResponseBody 注解的函数会直接把返回值作为响应，非字符串类型的会转换为 JSON

为了前端解析方便，开发中通常会定义一种统一的返回格式

![](https://notes.sjtu.edu.cn/uploads/upload_ccfe41f36a75ff67c6dbec3f915ac465.png)


### 分层、IOC/DI（控制反转/依赖注入）
#### 三层架构
所有业务逻辑写在一起很混乱，分层便于维护

![](https://notes.sjtu.edu.cn/uploads/upload_3f7571cce139c04e503efe1bc052c904.png)
 
- Controller 表示层、控制层：接受请求、响应数据
- Service 业务逻辑层
- DAO(Data Access Object) 持久层：数据访问操作，增删改查

![](https://notes.sjtu.edu.cn/uploads/upload_42dd3886bc4b0cfd7a910c80ef5aad01.png)

代码分层可以理解为代码的主要实现是分层的（即代码量）；
代码的调用关系依然是包含关系

![](https://notes.sjtu.edu.cn/uploads/upload_739a16eefe515db03e6c78177609f367.png)

#### IOC/DI 引入
- 高内聚：各个功能模块内部的功能联系 很高
- 低耦合：软件中各个层/模块之间的依赖、关联程度 很低


一般来说，caller 需要 new 一个 callee 的对象。如果对象的实现从 ImplA 切换到 ImplB，则 caller 创建对象的地方耦合了。


#### IOC 控制反转
![](https://notes.sjtu.edu.cn/uploads/upload_7048c99ce90c78deb941941209f0f746.png)

控制反转 (Inversion of Control)：对象的创建控制权由自身转移到外部（容器）


Bean 对象：IOC 的对象

#### DI 依赖注入

依赖注入 (Dependency Injection)：容器为应用程序提供运行时所依赖的资源，称为依赖注入（new 对象）


![](https://notes.sjtu.edu.cn/uploads/upload_49572a60576487e581f137f1de5f98dd.png)

  
#### IOC & DI 入门
使用 @Component 注解可以将实现类交给 IOC 容器管理

![](https://notes.sjtu.edu.cn/uploads/upload_4131630bc9a33fc31af50697e1cfa220.png)


在声明依赖对象处，使用 @Autowired 可以注入运行时对象

![](https://notes.sjtu.edu.cn/uploads/upload_180f3da6996a6cc080e76f4aef936e3f.png)


除了 @Component 以外，Spring 提供了 @Controller($\in$ @RestController), @Service, @Repository.三个注解。（仅仅是换了个名字

![](https://notes.sjtu.edu.cn/uploads/upload_1d0d546a07bc6ee009701acb5eb2b429.png)


##### 组件扫描 @ComponentScan
默认情况下，@SpringBootApplication 启动类会配置默认的扫描范围，该范围是启动类所在包及其子包

可以添加 @ComponentScan 控制扫描范围


## MySQL
### DQL (Data Query Language) 数据查询语言
![](https://notes.sjtu.edu.cn/uploads/upload_ea26a77d8d334231fa6eee8c747d4d08.png)


TO BE CONTINUED