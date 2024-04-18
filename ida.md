```x86asm
jmp    ds:0x804a008 # load data addr then jmp
jmp    ds:SOME_OFFSET # both these two jmp [SOME_OFFSET]
```