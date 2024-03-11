java 字节码 be like
```
Compiled from "Main.java"
public class Main {
  public Main();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
       3: ldc           #13                 // String Hello World!
       5: invokevirtual #15                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
       8: return
}
```

jar 包的 META-INF/MANIFEST.MF 文件中
- Main-Class 属性指定了程序的入口类；
- Class-Path 属性指定了程序的依赖库（其他 jar 包）。用于解决 jar 包与 jar 包之间的依赖关系，但不能解决 jar 包引用 内部 jar 包的问题。
- 