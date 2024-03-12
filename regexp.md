[https://github.com/ziishaned/learn-regex/tree/master](https://github.com/ziishaned/learn-regex/tree/master)

## 关于 ?
为了兼容性，最好的解决方案是把一个之前非法的语法添加新的语意。quantifier 后面本来不能接 ?，于是有了这些
```
Following a normal regex token (a character, a shorthand, a character class, a group...), it means "Match the previous item 0-1 times".

Following a quantifier like ?, *, +, {n,m}, it takes on a different meaning: "Make the previous quantifier lazy instead of greedy (if that's the default; that can be changed, though - for example in PHP, the /U modifier makes all quantifiers lazy by default, so the additional ? makes them greedy).

Right after an opening parenthesis, it marks the start of a special construct like for example

a) (?s): mode modifiers ("turn on dotall mode")
b) (?:...): make the group non-capturing
c) (?=...) or (?!...): lookahead assertion
d) (?<=...) or (?<!...): lookbehind assertion
e) (?>...): atomic group
f) (?<foo>...): named capturing group
g) (?#comment): inline comments, ignored by the regex engine
h) (?(?=if)then|else): conditionals

and others. Not all constructs are available in all regex flavors.

Within a character class ([?]), it simply matches a verbatim ?```

# Capture group vs. Non-capturing group
