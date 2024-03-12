# JNI
https://zhuanlan.zhihu.com/p/650000825

JNI 支持在 Java 中调用 C/C++ 代码（同进程），也支持在 C/C++ 中调用 Java 代码，是一套双向的接口。通过 JNI 可以
- 用本地代码实现 Java 中定义的 native method 接口，使 Java 调用本地代码；
- 把 JVM 嵌到一个应用程序中，此时 Java 平台作为应用程序的增强，使其可以调用 Java 类库。

JNI 是 Java API 的一部分，任何实现都支持该功能

> Java Platform 包含 Java API 和 JVM，所有实现都支持 Java API

### JNI 使用流程
1. 创建一个类(HelloWorld.java)，并声明 native 方法
   ```java
    public class HelloWorld {
         public native void print();
         static {
             System.loadLibrary("HelloWorld"); 
         }
    }
    ```
2. 使用 `javac` 编译该类
3. 利用 `javah -jni` 产生头文件
4. 在 C/C++ include 头文件，实现方法，编译成 .dll/.so 
   ```c
    #include <jni.h>
    #include "HelloWorld.h"
    JNIEXPORT void JNICALL Java_HelloWorld_print(JNIEnv *env, jobject obj) {
        printf("Hello World!\n");
        return;
    }
    ```
5. RUN

### 向 C/C++ 传递参数
#### JNIEnv, jobject/jclass
javah 向函数添加两个参数 
- `JNIEnv*, jobject`（实例方法 instance method）
- `JNIEnv*, jclass`（静态方法 static method）

JNIEnv * 是一个指向 JNI 接口函数表（Array of pointers to JNI functions）的指针，通过它可以调用 JNI 接口函数。JNIEnv 是线程相关的

native method 的类型在 JNI 中有两种
- primitive types: int -> jint, boolean -> jboolean, char -> jchar, ...
- reference types：eg. class, instance, array
- 对于 Java 层的 reference types，JNI 会传递一个  指向 JVM 内部数据结构的 opaque references(不透明指针)
- 对 opaque reference 的操作，都要通过 JNI 方法进行。用户不需要了解 JavaVM 内部数据结构。

#### accessing String, Array
"java.lang.String" 在 JNI 层对应的类型为 jstring。可以使用 JNIEnv -> GetStringUTFChars() 转换为 char*，直接访问

Array 类似

#### accessing field
静态成员
```c++
JNIEXPORT void JNICALL 
Java_InstanceFieldAccess_accessField(JNIEnv *env, jobject obj)
{
	/* Get a reference to obj’s class */
	jclass cls = (*env)->GetObjectClass(env, obj);

	/* Look for the instance field `String s` in cls */
	jfieldID fid = (*env)->GetFieldID(env, cls, "s", "Ljava/lang/String;");
	
	/* Read the instance field s */
	jstring jstr = (*env)->GetObjectField(env, obj, fid);

	/* Create a new string and overwrite the instance field */
	jstr = (*env)->NewStringUTF(env, "123");
	(*env)->SetObjectField(env, obj, fid, jstr);
}
```

获取静态成员是从 cls 获取，与上述流程类似但函数不同

#### accessing static/instance method, constructor
```c++
jmethodID mid = (*env)->GetMethodID(env, cls, "callback", "()V");
(*env)->CallVoidMethod(env, obj, mid);
```

静态方法调用流程类似，函数不同 

可以调用被子类覆盖的父类方法，CallNonvirtual\<Type>Method()，传入父类的 class 对象

构造函数的名字是 `<init>` 构造新对象

#### type signature
    
可用于解决重载。

| Type Signature           | Java Type             |
| ------------------------ | --------------------- |
| Z                        | boolean               |
| B                        | byte                  |
| C                        | char                  |
| S                        | short                 |
| I                        | int                   |
| J                        | long                  |
| F                        | float                 |
| D                        | double                |
| L fully-qualified-class; | fully-qualified-class |
| [ type                   | type[]                |
| ( arg-types ) ret-type   | method type           |

Notes:
- 类描述符开头的'L'与结尾的';'必须要有
- 数组描述符，开头的'['必须有
- 方法描述符，参数描述符间没有任何分隔符号
  
