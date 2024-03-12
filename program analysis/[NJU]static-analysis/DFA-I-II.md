# Lesson 3: Data Flow Analysis

DFA is about
- How data flows on CFG
- How application-specific data flows through the Nodes (BBs/statements), Edges (control flows) of the CFG

### Safe-Approx
We want a *safe-approximation*. Depending on backend scenarios, safe could be
- safe = over, `may analysis`. Output may be true
- safe = under, `must analysis`. Output must be true

(among branches)

---

Different DFA applications have 
- different data abstractions, 
- different flow safe-approximation
- different strategies, and transfer functions, contrl flow handlings (eg. merging strategies)

## Preliminary
In DFA application, we associate with every program point a `data-flow value` that represents an `abstraction` of the set of all possible `program states` that can be observed for that point.
- 类似用一个结构体表示 abstraction

DFA is to find a solution to a set of safe approximation directed constraints on the $IN[s]$s and $OUT[s]$s for all statements $s$.<br>
where the constraints based on
- semantics of statements (transfer functions)
- flows of control

#### Forward Analysis
沿控制流分析
$$OUT[s] = f_s(IN[s])$$

#### Backward Analysis
$$IN[s] = f_s(OUT[s])$$

部分特殊的 BA 会反转 Control flow（CFG 的边）


#### Input and Output States
- Each IR transforms the input state to a new output state. $IN[s] \rightarrow OUT[s]$
- The input/output state is associated with the `program point` before/after the statement
#### Domain
The set of possible DF values 

#### Control Flow Constraints
- CF within a BB, $IN[s_{i+1}] = OUT[s_i]$
- CF among BBs, forward $\begin{array}{l}
  IN[B] = IN[s_1]\\ 
  OUT[B] = OUT[s_n]\\
  OUT[B] = f_B(IN[B])\\
  f_B = f_{s_n} \circ \cdots \circ f_{s_2} \circ f_{s_1}\\
  IN[B] = \land_{P: \text{a predecessor of B}} OUT[P]\end{array}$

#### Meet Operator
`^` or $\land$


## Application
### Reaching Definitions Analysis
目前只处理简化的场景：
- Intra-procedural CFG
- No Aliases/Pointer

#### Terminology
A `definition d` at program point p `reaches` a point q if there is a path from p to q such that d is not “killed” along that path

lec3 - slides - P41

- Assignments are definition
- 求解：任一 program point p, 哪些 definition 能 reach p.

Application: Entry 设置 dummy value 检测 undefined

#### Transfer Function
$$
\operatorname{OUT}[B] = gen_B \cup (\operatorname{IN}[B] - kill_B)
$$
where $gen,kill$ are sets.

#### Control Flow
$$
\operatorname{IN}[B] = \bigcup_{P: \text{a predecessor of B}} \operatorname{OUT}[P]
$$

safe
#### Algorithm

```
while (changes to any OUT occur){
    for each (basic block B \ entry){
        IN[B] = Union(OUT[P]);
        OUT[B] = f(In[B]);
    }
}
```
使用 bitmap[n] 表示 $d_0, \ldots d_n$, decltype(IN[B]) =  bitmap[n]。单调有限保证算法会停

# Lesson 4: Data Flow Analysis Contd.

