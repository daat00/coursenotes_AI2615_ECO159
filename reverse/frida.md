> 一直想学 frida 一直鸽，终于有 ctf 用到了，我测东西这么多

> 官方文档感觉写的依托，这 python rpc 直接拿来入门真的太有生活了

frida 是一个**动态插桩** (dynamic instrumentation) 工具，提供了交互式 cli 界面来追踪函数行为。用人话说就是允许用户从任意位置跳转执行自定义代码，用 js 代码片段 hook 原始程序的任意位置。具体而言，类似断点的实现方法，frida 会将 hook 入口处的汇编指令替换为跳转到 js 引擎的 jmp 指令，在 js 运行完毕时复原汇编代码。

典型的应用场景：从任意行号开始执行、替换任意函数的实现


@[toc]

# 安装

直接 pip install 即可。安装后会创建 `frida`, `frida-ps` 等命令。miniconda 会在切换时将 Scripts 添加到 PATH，在 windows 也可以直接用。
## 安装类型补全

利用 vscode 自动导入 ts 类型的机制，在 pwd 下 `npm install @types/frida-gum` 可以安装类型补全。这样之后写 js 的回调就有 type hint 了

## frida-gum 介绍

上面依赖中出现的 `gum` 是 frida 的动态插桩核心库 `Gum` 的意思。`Gum` 用 C 开发，支持访问整个内存空间。考虑到 hook 的代码变化会很频繁，frida 又提供了一套脚本语言的 api `GumJS` ，为用户省去构建流程，缩短逆向开发时的反馈周期。

在运行时，`Gum` 和 `GumJs` 被打包成共享库注入到目标进程内。自定义脚本在用户进程中，通过 ipc 跨进程代理变量访问。这一打包、注入、双向通信的工作由 `frida-core` 完成。`frida-core` 包含了 frida 在目标机的主要功能，除了支持 gum 注入外，还包含 ps，枚举安装程序等功能。


# frida 使用：插桩 apk

Frida 提供了简洁的语法，支持用户利用 js 的 api 通过字符串名称筛选符号，找到目标模块、目标类、目标对象或目标函数后，最后在 js 中检查传入参数和返回值、并支持用回调函数覆盖目标的实现。

Frida 依赖操作系统的调试 syscall 实现 hook (JVM, webasm 运行在 custom 解释器)，因此天生依赖 OS，所以也会在用户体验上对 os 做很多优化。Frida 支持 Windows/Linux, Android, iOS, macos 等多种系统。被主要用于 Android/iOS, webasm 等环境。PC 系统上，Frida 只支持 Ring3 的部分特性因此不如其他平台好用，在这些平台常常需要其他工具来配合。

## 注入 frida-core 并启动 cli

注入 js 脚本的基础语法形如：

```bash
frida -U -f com.example.someapp -l hook.js
```
这个命令会在当前 shell 创建一个 REPL cli 交互环境，类似浏览器的 console，用户在里面敲命令回显结果。

- `-l` 参数加载 js 脚本文件。和浏览器一样，加载 js 文件后，可以在 cli 敲符号名运行自定义的函数，也可以直接在 js 写 `setImmediate(main) ` 加载后立即运行 `main`。frida 支持 js 脚本的热更新，在 js 文件 onSave 的时候会触发重新加载
- `-f` 参数会 spawn 一个新的目标程序
- `-U` 表示通过 USB 在目标设备上运行程序（而非当前客户机），调试移动端通常都会有 -U。理论上需要在手机上安装并启动 `frida-server`，但实测下来雷电模拟器似乎并不需要，不知道为啥

> frida-server 在目标机运行 frida-core，并对外暴露一个 tcp 服务通信
## 插桩安卓 Java 程序

编写 `hook.js` 如下：

```js
function main() {
    // Java perform 指进入 jvm 执行回调
    Java.perform(function() {
        var _class = Java.use('java.lang.String') // 从全限定名拿到 class
        // 根据名称直接访问成员，用类型 spec 重载函数。（变量和函数同名会将变量添加 _ 前缀）
        // .impl hook 函数实现
        _class.someMethod.overload("int", "int").implementation = function(arg0, arg1) {             
        	var ret = this.someMethod(arg0, arg1) // this 访问原始函数
            console.log(arg0, arg1, ret) // 通过 log 打印
            return ret
        }
        let string = _class.$new("helloworld").getBytes()
        
        this.m.value = 0; // 直接设置变量值

		Java.choose // 从 cls 枚举所有 obj
    })
}

setImmediate(main) // 异步执行 main 避免阻塞
```

> 常见开发模式：在 aosp java 的源码里搜索内部调用链，找一个合适的 hook 掉

### frida hook Java 中动态加载的 dex

有些时候，Java 会在运行时动态加载 dex，frida 可能找不到。这是因为 frida 默认的 classloader 不一致，需要在 js 里通过 `Java.enumerateClassLoaders` 切换 classLoader.

### frida 调用额外 .dex 文件的函数

逆向工程师可以自己写一个 Java 程序，用 d8 编译成 dex 文件后，通过 frida 的 `openClassFile` 加载到 frida 的 VM 里。之后，可以在 frida 中同样地用 `Java.use` 调用自己写的 Java 函数。

### 在 activity 启动前附加 frida

在启动 frida 时，指定 `--no-pause -f` 参数，并在 js 里 hook `ActivityThread.getApplicationContext`


### 在 frida 中定义类，实现一个接口
参看 api 文档的 `Java.registerClass` 节。可以实现一个新的类，用 js 编写类的成员函数，并实现接口条件。

### 打印 stacktrace
两种实现，既可以 hook Java 的 `java.lang.Exception` 类，查看文档选择 element。似乎也可以用 backtrace 的 api
## 插桩安卓 so 原生代码

动态库的导出其实和 js 有点像，会指明  import 和 export。在 IDA 可以看到有一个 exports 面板展示了所有导出的函数。对于 export 的函数，frida 可以直接通过函数名定位并 hook。对于其他函数，不管函数有没有导出，frida 都支持直接索引地址 hook，也可以 hook 到函数内部的任意汇编位置。（似乎非 export 函数也能按名字hook

> 由于安卓程序的入口是 Java，在 System.loadLibrary 之前 so 还没有加载。所以通常需要将 Interceptor 放到 loadLibrary 的 hook 回调里。也看到过放到 linker 的 Android dlopen 函数后面的，或者也可以手动在 cli 执行函数。
### 按名字查找 export 函数

借助 `Interceptor` 和 `Module.getExportByName` 可以实现通过字符串符号名 hook 动态库 module 的函数。这里 Module 是专门用来处理动态库的组件，甚至可以写一个新 c 函数。回到 hook，文档里给的例子是 hook socket, inet 库的 send 函数，修改入参的 ip 地址以实现向别处发包。

```js
const st = Memory.alloc(16);
// Now we need to fill it - this is a bit blunt, but works...
st.writeByteArray([0x02, 0x00, 0x13, 0x89, 0x7F, 0x00, 0x00, 0x01, 0x30, 0x30, 0x30, 0x30, 0x30, 0x30, 0x30, 0x30]);
// Module.getExportByName() can find functions without knowing the source
// module, but it's slower, especially over large binaries! YMMV...
Interceptor.attach(Module.getExportByName(null, 'connect'), {
    onEnter(args) {
        send('Injecting malicious byte array:');
        args[1] = st;
    }
    //, onLeave(retval) {
    //   retval.replace(0); // Use this to manipulate the return value
    //}
});

// Use Interceptor.replace to replace impl.
```

### 按名字查找 module
`Process.findModuleByName`， 见下节

### 枚举 module 的符号
如果是通过 JNIOnload 或其他方法动态注册的 Native 函数。被注册的 Native 函数本身并不是 export 的。此时可以 hook libart 的 RegisterNative 函数，劫持注册过程。

```js
let libart_module = Process.findModuleByName("libart.so")
let symbols = libart_module.enumerateSymbols()

for (var i = 0; i < symbols.length; i++) {
	var name = symbols[i].name
	if (name.indexOf("RegisterNative")) {}
}
```

### hook RegisterNative



### 识别 jclass
> jclass 在底层只是一个序号，可以通过 frida-java-bridge 的 Env.getClassName 转换得到类名等信息

frida-java-bridge 提供了 jclass 的解析库，可以在直接使用。

```js
Java.vm.tryGetEnv().getClassName(jclass)
```
### 按地址 hook 函数 
通过地址 hook 有多种实现方法：
```js
// 用 `Interceptor.attach` hook 特定地址。ptr 是 frida 的指针类
let base = Module.findBaseAddress("my.so")
Interceptor.attach(ptr("0x1234"), { // Or base.add
    onEnter(args) {
        console.log(args[0].toInt32());
        args[0] = ...
    }
});

// 用 NativeFunction 封装二进制函数在 js invoke
// 用 Memory 在目标进程 malloc 内存，并在 invoke 传参新指针
const st = Memory.allocUtf8String("TESTMEPLZ!"); // See also: Memory.alloc(), Memory.protect()
const f = new NativeFunction(ptr("0x1234"), 'int', ['pointer']);
    // In NativeFunction param 2 is the return value type,
    // and param 3 is an array of input types
f(st);
```

按地址 hook 具有很强的灵活性。可以 hook 到函数内部的某一条汇编指令然后修改寄存器。这种被称作内联 hook



> Imyang 说，内联 hook（hook到函数内部的 asm）不如 hook 函数入口稳定。能用倒是


# 使用 frida stalker 进行 assembly tracing
由于 ida 反编译现在已经相对比较成熟，所以市面上也会有对 c 程序进行混淆的工具链。逆向混淆后的程序很折磨，特别是引入虚假控制流程和控制流平坦化后，难以识别到底哪些代码被执行了。在这种时候，可以追踪每条实际执行的汇编指令，得到实际执行的汇编指令串。IDA 和 frida 都提供了 tracer 模块。

frida stalker 可以跟随线程的执行，跟踪每个函数、基本块和指令。具体使用不想写了。
# frida-python

> 官方文档乱的要死

rpc，姑且可以先看看官网的例子

# MISC
## 便捷命令 - frida-ps, frida-trace

> 官方文档从这里入门，但对 cli 参数基本没有解释。我每次都很难对上脑电波，一定是 doc 写的依托答辩 :\

frida 除了 `frida` 命令外，还提供 `frida-ps`, `frida-trace` 等 cli 命令，帮助用户快速跟踪函数。

`frida-ps` 可以直接敲，类似 ps 可以打印进程的名字。

`frida-trace` 需要附加额外参数，例如 `frida-trace -i "recv*" "Clash for Windows.exe"` 会按照进程名称字符串或 pid 把 frida 附加到进程，然后 trace 所有以 recv 开头的受支持函数。`frida-trace` 会给每个监听函数生成 js 模板，用户可以修改回调脚本，热更新刷新 trace 的行为。

`frida-trace` 自动生成的模板包含 `OnEnter` 和 `OnLeave` 两个关键函数，分别在目标函数进入和退出时同步调用。用户可以自定义回调函数的行为，支持覆盖参数和返回值。
```js
{
    /**
     * Called synchronously when about to call recvfrom.
     *
     * @this {object} - Object allowing you to store state for
     * use in onLeave.
     * @param {function} log - Call this function with a string
     * to be presented to the user.
     * @param {array} args - Function arguments represented as
     * an array of NativePointer objects.
     * For example use args[0].readUtf8String() if the first
     * argument is a pointer to a C string encoded as UTF-8.
     * It is also possible to modify arguments by assigning a
     * NativePointer object to an element of this array.
     * @param {object} state - Object allowing you to keep
     * state across function calls.
     * Only one JavaScript function will execute at a time, so
     * do not worry about race-conditions. However, do not use
     * this to store function arguments across onEnter/onLeave,
     * but instead use "this" which is an object for keeping
     * state local to an invocation.
     */
    onEnter(log, args, state) {
        log("recvfrom()");
        log("recvfrom(socket=" + args[0].toInt32()
    + ", buffer=" + args[1]
    + ", length=" + args[2].toInt32()
    + ", flags=" + args[3]
    + ", address=" + args[4]
    + ", address_len=" + args[5].readPointer().toInt32()
    + ")");
    },

    /**
     * Called synchronously when about to return from recvfrom.
     *
     * See onEnter for details.
     *
     * @this {object} - Object allowing you to access state
     * stored in onEnter.
     * @param {function} log - Call this function with a string
     * to be presented to the user.
     * @param {NativePointer} retval - Return value represented
     * as a NativePointer object.
     * @param {object} state - Object allowing you to keep
     * state across function calls.
     */
    onLeave(log, retval, state) {
    }
}
```

## 在 jailed iOS/Android 使用 frida

在 root 的 Android 下可以直接将 frida attach 到一个进程上，这被称为 *inject mode*。

但在 jailed OS 上该功能可能受限。在这些设备上，Frida 提供 `frida-gadget` 动态共享库，开发者需要在编译应用时把 `frida-gadget` 加入到 lib 中，从而在应用启动时自带 frida 功能。这被称为 `embed` 模式。（我感觉主要用于调试而非逆向）

除了编译 so，官网也给了 [其他途径](https://frida.re/docs/gadget/) 把 frida-gadget 嵌入到程序中，包括 `LD_PRELOAD`(` DYLD_INSERT_LIBRARIES`), patch binary

## frida 辅助分析 OLLVM
OLLVM(obfuscated LLVM) 是对 C 代码的混淆工具，额外增加了逆向的难度。混淆器可以把简单的混淆策略迭代多轮，提高复杂度。

### 常见特征
1. 程序所有常量加密，在 `.init_array` 存在解密函数。（官方默认解密函数名是 `datadiv_decode` 但可以换）
2. 控制流平坦化：while 1 ...
3. 指令替换：c = a+b => { r = rand(); c = a - (-b) = a - r - (-b) - (-r)。

### 技巧

hook 随机数发生器固定结果。然后多次调用函数根据输入输出猜函数功能

Google 搜索反编译结果里的特征查找算法的公开实现，包括 hex 常量，字节数组

识别混淆策略。如指令替换的 r，识别之后只看输入输出猜结果不管过程。

停止其他线程以防反调试。
## ref
http://littleshark.space/2024/08/25/Frida%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/Frida%E4%BD%BF%E7%94%A8%E5%A4%A7%E5%85%A8/

阿 b 直接搜 frida 看别人操作会直观很多：
https://www.bilibili.com/video/BV1wt4y1n7cq
https://www.bilibili.com/video/BV1G4sxemEuA
