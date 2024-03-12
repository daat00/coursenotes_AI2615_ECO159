# The missing semester
## 信号是一种软件中断
例如，可以通过
```python
except KeyboardInterrupt:
    pass
signal.signal(signal.SIGINT, handler)
```
屏蔽 keyboard interrupt
- SIGINT, SIGQUIT(强制退出), SIGKILL, SIGTERM
- SIGSTOP, SIGTSTP(暂停); fg, bg

## Dotfiles 配置文件
dotfiles，例如 `.`bashrc。在 shell 启动时，取决于 shell 和连接方式有不同的配置加载方式 [ref](https://blog.flowblok.id.au/2013-02/shell-startup-scripts.html)

# Tmux
Session
- windows
  - panes

# Git
### Terminology
- `tree`: folder
- `blob`: file

pseduo code
```
type blob = array<bytes>
type tree = map<string, tree | blob> # map<dirname, tree | blob>
type commit = struct{
    parents: array<commit>
    author: string
    message: string
    snapshot: tree
}
type object = blob | tree | commit

objects = map<sha1, *object> # where * is a pointer(hash)
```

```
references = map<string, sha1> # map<refname, sha1>
```

### Remote
```
git remote add origin ../remote
```

#### Misc
git bisect

git stash


# Profiling
```bash
time curl -s http://example.com // prints real, user, sys
strace // system call
perf // hardware performance counter
stress
```

## CPU Profilers
- Tracing profilers
- Sampling profilers

### Tracing profilers
For example, in Python, we have `cProfile` and `line_profiler`. 

Also, for memory profilers, `valgrind`

### Sampling profilers
Flame graphs: y-axis is stack depth, x-axis is time spent in that function

Call Graph

## Monitoring
- htop
- ncdu
- lsof(list open files)
- hyperfine - benchmark  

## Resource Isolation
`cgroups` and `namespaces`

# Meta Programming
## Build Systems
make helps to build whatever needed. C++, latex, etc.
```makefile
target: dependency0, dependency1, ...
    command
dependency%: _dependency% # % is a wildcard
    ./do.py -i _dependency$* -o out/$@ # $@ is the target
```

## Semantic Versioning
`MAJOR.MINOR.PATCH`
- `MAJOR`: API breaking changes, backward incompatible
- `MINOR`: new features
- `PATCH`: bug fixes

### Lockfile
lock the version of dependencies, faster and more reliable

## Continuous Integration
### Travis CI
### Github Actions
### Dependabot
### Github Pages
Jekyll: .md -> .html

## Tests
### Unit Tests
Small, self-contained test for one feature

### Integration Tests
Integration between subsystems

### Regression Tests
Test bugs, to ensure that bugs don't reappear

## Mocks

# Misc
### Reverse Debugging
### Installation paths
- `/bin': essential system utilities
- `/usr/bin`: user utilities
- `/usr/local/bin`: user compiled utilities
- `/etc`: configuration files
- `\var`: time varying data

# GCC
可以通过选项让 gcc 输出中间结果，如 `gcc -E` 输出预处理结果，`gcc -S` 输出汇编代码

#### \#include
`<>` 和 `""` 只是查找路径不一样，可以配置

