# Chapter 15. Object-Oriented Programming
## 继承，派生类
``` 
c++ 标准没有规定继承的内存布局

在 GCC 中，通常采用以下原则来安排类的内存布局：

多继承: 在多重继承的情况下，GCC 会根据基类声明的顺序，依次将各个基类的成员依次加入派生类的内存布局中。同时，GCC 会使用指针偏移来解决不同基类中可能存在的命名冲突。

虚函数表: 对于包含虚函数的类，GCC 会在对象的内存布局中添加一个指向虚函数表（vtable）的指针，以支持动态绑定和多态性。
```

`基类指针和引用`可以指向`派生类`对象，但这样就不知道派生类的信息，所以只能调用基类的成员函数，而不能调用派生类的成员函数。

在引入`虚函数`后，会出现`动态绑定`的问题。基类调用的虚函数有可能是派生类覆盖的。

> `引用`与`指针`的静态类型与动态类型可以不同；而`对象`的类型并不会改变

### 默认实参
虚函数的默认实参在`编译`时确定，因而如果基类和派生类的默认实参不同，在运行时，会把基类默认参数传给派生类虚函数


### 覆盖虚函数
可以使用作用域运算符`::`来指定调用哪个版本的虚函数。
``` c++
baseP->Base::print();
```

### 静态成员
所有派生类共享同一个静态成员

### 抽象类
包含纯虚函数的类是抽象类，不能实例化

## 虚继承
在多重继承中，
```c++
        ios
        /  \
istream     ostream
        \  /
     iostream
```
iosteam 从 istream 和 ostream 继承了两份 ios，但我们希望共用同一个 ios，这时就需要虚继承。

通过使基类成为虚基类，istream 和 ostream 指定，如果有其他类继承它们，无论 ios 在派生层次中出现多少次，都只继承一份 ios。

``` c++
class istream: virtual public ios { ... };
class ostream: public virtual ios { ... };
```

在构造时，由最终派生类负责初始化虚基类的成员。无论虚基类出现在继承层次的任何地方，总是在构造非虚基类之前构造虚基类。


## Misc
C++ 有静态成员函数的概念，声明为 static 的成员函数不与任何对象绑定，只可以访问静态成员。

# Chapter 17 Miscellaneous Topics
## 异常
异常类型是静态类型，编译时确定

``` c++
class exception {
    ...
    virtual const char* what() const noexcept;
};
```

### 函数 try 块
``` c++
constructor() try: member-init-list {
    // function body
} catch (exception e) {
    // exception handler
}
```

### 栈展开 stack unwinding
析构函数应当不抛出异常，否则 terminate。标准库的析构函数都不抛出异常。

### 重新抛出异常
``` c++
throw;
```

## Auto Pointer
``` c++
auto_ptr<T> ap(void*);
```

exception 发生后，无法执行 delete，但把指针交给对象，调用对象析构时会 delete

auto_ptr 不能保存到容器中，因为容器要求 copy constructor 和 operator=

### Shared Pointer
### Weak Pointer

## 命名空间与作用域
派生类会屏蔽基类的同名成员，*哪怕是不同的声明*。但是可以通过作用域运算符`::`来访问基类的成员。

可以通过`using`声明来引入基类的同名成员，这样会导致基类的所有同名成员都可见，可以再重定义。

### 未命名的命名空间 unnamed namespace
代替 static，未命名的命名空间可以是不连续的，其中的名字只在当前文件中有效。但生命周期从程序开始到结束。
``` c++
namespace {
    int i;
}
```

### using 声明
``` c++
using std::cout;
```



# Chapter 18 优化内存管理
`new` 可以分解为两个步骤：`operator new` 分配内存，`constructor` 构造对象。

## Allocator
``` c++
allocator<T> a;
T* p = a.allocate(n); // 分配 n 个 T 类型的内存，但不构造对象
a.deallocate(p, n); // 释放内存

a.construct(p, args); // 在 p 处构造对象
a.destroy(p); // 析构对象
```

## Operator new
不构造对象，只分配内存
``` c++
void* operator new(size_t);
void* operator new[](size_t);
void operator delete(void*);
void operator delete[](void*);
```

## Placement new
``` c++
new (place_address) type (initializers);
```
``` c++
void* operator new(size_t, void*); // 不分配内存，只调用 constructor
void* operator new[](size_t, void*);
void operator delete(void*, void*); // 不释放内存，只调用 destructor
void operator delete[](void*, void*);
```

## 重载 new 和 delete
### 成员 new 和 delete
在成员函数中定义 operator new 和 operator delete

virtual ~ 时，编译器会动态地传 size

### 继承 std::allocator
240120 试着继承 std::allocator，但是发现 myAlloc.allocate 并没有被调用。检查了半天没找到原因。

    Alloc::rebind<T>::other if present, otherwise SomeAllocator<T, Args> if this Alloc is of the form SomeAllocator<U, Args>

继承需要重写 `rebind` 等函数，否则不会被调用（c++ 11）
https://cplusplus.com/forum/general/103471/
https://cplusplus.com/forum/general/86488/
https://itecnote.com/tecnote/c-what-does-template-rebind-do/

最后的 link 说明了 rebind 的用途

# RTTI
为了实现运行时类型检查，C++ 标准提供了一套名为 RTTI 的接口，这套接口的核心是 `std::type_info` 类，它用于在运行时表示某个类型的相关信息。但这套接口提供的功能过于简单，它只支持查询一个多态对象的动态类型以及类型的基本属性（例如类型的名称等），并不支持动态查询不同的类型之间的关系（例如查询两个 std::type_info 所表示的类型是否构成继承关系等）。因此，各个 ABI 都需要对 RTTI 定义的功能进一步扩充以满足 dynamic_cast 的需求。最关键的扩充部分在于以下三点：

由于 RTTI 机制的存在，编译器会在必要的条件下为每个类型生成一个 std::type_info 结构描述这个类型。Itanium ABI 进一步扩展了 std::type_info 结构中所包含的数据，使之能够描述类类型之间的继承关系。(略)

当父类不是子类的虚基类时，`__offset_flags` 成员是一个可以直接施加到父类子对象指针上的偏移，这个偏移会将父类子对象指针调整为指向子类对象；当父类是子类的虚基类时，`__offset_flags` 成员是一个施加到父类子对象的虚表指针上的偏移，这个偏移会使得父类子对象的虚表指针指向描述虚基类相对于子类对象的偏移的虚表项。但无论是哪种情况，通过 __offset_flags 成员，我们都可以将直接基类子对象的指针调整为子类对象的指针；在此基础上，只要沿着继承链一级一级地调整基类子对象指针，最终就可以将基类子对象指针转换为派生类对象指针。

## dynamic_cast
- 将基类的指针或引用安全地转换成派生类的指针或引用
- 运行时检查，如果转换失败，返回 0
``` c++
dynamic_cast<type*>(e);
```

## typeid


# 成员指针
只能用于非 static 成员，使用 .* 或 ->* 来访问成员

``` c++
class foo{
    int bar(int);
};

int (foo::*p)(int) = &foo::bar;


foo* pfoo = new foo;
pfoo->*p(1);
```

# Union 和 bit field
## union
union 表示互斥的成员，当一个成员被赋值后，其他成员的值是未定义的。

匿名 union 的成员暴露在外部作用域中

## bit field
位域的实现与机器相关，使用 : 来指定位域；

可以定义函数，但不能取地址。位域不能是静态成员

``` c++
struct {
    unsigned char a: 4;
    unsigned char b: 4;
} foo;
```