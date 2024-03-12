# jar 包结构
jar 就是 zip 包，class 保存 byte code 和符号
```
META-INF
    MANIFEST.MF
com
    util
        Util.class
    main
        Main.class
```
MANIFEST.MF
```text
Main-Class: com.main.Main
```



> 为什么 bytecode 容易被反编译？
>
> 1. 类型通过半文字的方式显性定义
> 2. 优化通常由 JIT 完成，而不是编译器
> 3. 代码中包含了很多元数据，比如方法名，字段名，类名等

# Invocation
1. `invokespecial`: call constructor, private method, super method
2. `invokevirtual`: instance method(virtual dispatch, 派生)
3. `invokeinterface`: cannot optimization, 检查是否全部实现
4. `invokestatic`: static method
5. `invokedynamic`: make dynamic language easier to implement on JVM

method signature: class name: return type method name (parameter type list)

\<clinit\>: class loader initialization