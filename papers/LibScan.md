LibScan: Towards More Precise Third-Party Library Identification for Android Applications
===
```
In: TPL * 1 , .apk * 1 (.class)
Out: (TPL in .apk) ?
```

**Step 0**: 粗筛，遍历每个 apk 的类和 TPL 的类，计算每个 pair 的相似度 (class - class similarity)

1. fix 一个待测 APK
2. for each fixed 待测 TPL，处理 TPL 之间的依赖关系，合并相关 node_dict，然后进行 detect

---
**准备工作**：提取 feature，fingerprint / method opcode

---
后续细筛先后筛三次

**Step 1**: `Signature based Class Correspondence Detection`
- 为每个 .class 建立一个 signature(class - class)
- `public, abstract, L, [` 等

Step 2: `Method-opcode similarity detection`(method - method)

Step 3: `method call chain similarity comparison`(method call chain)


## 详细说明

### Step 1 - Signature based Class Correspondence Detection
见 Table 2,3,4. 要求 app.class 的 signature *identical* to TPL.class 的 signature

对每个 TPL.class, fix 该 class，然后求 apk.class 中满足所有约束（signature）的 class（集合交集），返回一个集合



### Step 2 - Method-opcode similarity detection
(coarse-match)

可能会出现多对多（多对一，一对多）的情况，需要更细致地用 method 来 match class

> Dalvik inst = opcode + operand

定义 $S_{method}$ 为函数中不同 `opcode` 的集合，给定一个 apk.class  和一个 TPL.class，尝试 match 他们的 method。若 $S_{TPL.method} \subseteq S_{apk.method}$，则认为 match。

这么做可能会出现多个 apk.method 对应一个 TPL.method 的情况，需要进一步细筛。此时取 $best(apk.m, tpl.m) = \arg\min_{x\in apk.method} |S_x - S_{TPL.method}|$ 作为最终 match TPL.method 的 apk 函数。

注意我们要比较的是 class 的异同，找到 match 的函数后，通过 `match 函数占 TPL.class 的比例` 评估 class 的相似度。$MOSS > \theta_1$ 出现在这里
$$
MOSS(apk.class,tpl.class) = \frac{\text{\#opcode of matched TPL.class.methods }}{\text{ \#opcode of all TPL.class.methods}}
$$

```python
:filter_result: Dict[lib.cls_name, Set[apk.cls_name]], apk 中，与固定 lib.cls_name 匹配的所有 apk.cls_name
    :opcode_dict: opcode -> int

    for lib_class:  for apk_class:
        if abstract, 只要 lib_cls 和 apk_cls 的 method_num 相等，就匹配
        else:
            for lib_method: for apk_method:
                if lib_method 与 apk_method 的 descriptor 相等，进行进一步比较。
                在 match 的前提下（lib_method 的 opcode 均出现于 apk），取 lib_method 与 apk_method 的 argmin( |lib_method_opcode_num - apk_method_opcode_num| ) 作为最佳匹配，记录 lib_method -> apk_method 于 methods_match_dict 中。

            把每个 method 匹配后，计算 sum(apk_class_method.opcode_num) / apk_class.opcode_num。**若按该 APK 公式计算的比率**超过阈值 class_similar，认为 lib_class 匹配了（多个）apk_class，记录于 match_classes, 并记录 lib_class -> {apk_class -> methods_match_dict} 于 lib_class_match_dict 中。

    返回 lib_match_classes, abstract_lib_match_classes, lib_class_match_dict
```


### Step 3 - method call chain similarity comparison
对于上一步选出来的 `best matched` 的 apk 函数和 TPL 函数 $m, n$, 分别通过限制 depth 的 dfs 得到 call chain $CC^m, CC^n$（函数的集合）。

要求 `call-chain-opcode inclusion`，$CC^n \subseteq CC^m$。否则直接 pass（即混淆只增添了 op

### Confidence
$\theta_2$ 出现在这里

令 $X$ 为 apk 的所有 classes，$Y$ 为 TPL 的所有 classes
$$
confidence(X,Y) = \frac{\text{\#opcode of matched methods}}{\text{\#opcode of all methods of all class}}
$$

计算 $confidence > \theta_2$

# Dataset

# 不足
反射，inline，wrapper   

# 代码思路
所有 .class 均被转换为 .dex 交由 androgurad 处理

分析的入口在 module/analyzer `search_libs_in_app`，顶层函数建立数个 cache，然后 for apk: for TPL，查询其之前分析的结果。若没有则进行分析（记忆化暴力搜索

在顶层循环中，循环检查是否有未分析的 TPL.jar，如有，则对于每个包不同版本的 TPL(.jar) 中，遍历每个 TPL.class 找到 match 的 apk_class，进行 signature 比较

然后进行 coarse_match 和 fine_match



### ThirdLib 反编译类
该类基于 Androguard 的 .dex parser 进行 feature extraction

在构造对象时加载 .dex 文件，并根据 decl-seq 中，限定符、参数返回值类型，dex 反汇编的 smali code，invoke 调用的函数名等信息，计算
- 在特征空间中的一个向量 ($v\in \{0,1\}^n$，n 为参数组合总数)
- 函数调用关系的元数据（列表）
- 给每个函数记录一个 opcode-seq，计算 hash，然后将该 seq 去重


----

### Misc
#### Bloom Filter
- 创建一个 $n$-bit 的 bitmap
- 用 $k$ 个 hash 函数。查询/插入元素 $x$ 时，计算 $h_1(x), h_2(x), \cdots, h_k(x)$，将对应的 bit $B[h_i(x)]$ 置为 1
- 查询时，若对应的 bit 全为 1，则返回 true，否则返回 false。
- **由于 hash 冲突，返回 true 不一定是 true。但 false 一定 false**

问题：无法扩容，无法删除元素

#### 布谷鸟过滤器
> Cuckoo Hashing（布谷鸟 hash）：鸠占鹊巢。用于寻找 hash 碰撞后下一个位置
> - 新元素直接占用位置，旧元素去占用别的位置
> - 两个 hash 函数
>
> 当插入元素 $x$ 时，计算 $h_1(x), h_2(x)$，随机优先向没有被占用的位置插入 x。若两个位置都被占用，则将 $x$ 替换掉旧元素（二者 hash 冲突），并递归地重新做


