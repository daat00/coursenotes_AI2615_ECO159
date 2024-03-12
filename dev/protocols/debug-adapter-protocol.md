https://microsoft.github.io/debug-adapter-protocol/

#### 合适的阅读方法
Overview 为主，overview 不清晰的部分去 spec 里查一下，尽量避免 spec。spec 太罗嗦了

# Base Protocol

类似 http，前面 header，后面 JSON
```json
Content-Length: 119\r\n
\r\n
{
    "seq": 153,
    "type": "request", /* request, response, event */
    "command": "next",
    "arguments": {
        "threadId": 3
    }
}
```

Request, response, event 有不同的 JSON 字段，见文档

Capabilities 用于协商 feat（version 目前不变）


```
Threads
   StackTrace
      Scopes(locals, registers, args)
         Variables(name value pair)
            ...
               Variables
```



----

## Cancel Request
- 表示 client 不再关心这个 request 的结果
- 要求关闭 progress

这是一个 best effort 消息，不论成功与否都有可能有后续消息

# Event
## Initialized Event
完成 initialize request 后，adapter 表示准备好接收 configuration（breakpoint）

## Stopped Event
## Continued Event
## Exited Event
## Terminated Event
不同于 exit， terminate 表示 debugging 结束，但 debuggee undefined behavior

<!-- Indicates thread begin -->
## Thread Event
## Output Event
## Breakpoint Event
## Module Event
## Loaded Source Event
## Process Event
## Capabilities Event
## ProgressStart Event
## ProgressUpdate Event
## ProgressEnd Event
<!-- Indicates data change. Runtime change ignored -->
## Invalidated Event
## Memory Event

# Reverse Request
## RunInTerminal Request
## StartDebugging Request

# Request
## Initialize Request
## Configuration Done Request
## Launch Request
## Attach Request
参数的 convention 不由 DAP 定义，由 IDE 通过插件等方式找到后填入 arguments: 

## Restart Request
## Disconnect Request
## Terminate Request

<!-- Breakpoints -->
<!-- Real breakpoints may not set in request lines  -->
## Breakpoint Locations Request
## Set Breakpoints Request
## Set Function Breakpoints Reques  t
## Set Exception Breakpoints Request
## DataBreakpointInfo Request
## Set Data Breakpoints Request
## Continue Request

<!-- Execute one thread, other run freely/suspended -->
## Next Request
## StepIn Request
## StepOut Request

## stepBack Request
## Reverse Continue Request
## RestartFrame Request
## Goto Request
## Pause Request
## StackTrace Request
## Scopes Request
## Variables Request
## setVariables Request
## Source Request
## Threads Request
## TerminateThreads Request
## Modules Request
## Loaded Sources Request

<!-- Compute -->
## Evaluate Request
## setExpression 

## Read Memory
## Write Memory
## Disassemble


# SDK