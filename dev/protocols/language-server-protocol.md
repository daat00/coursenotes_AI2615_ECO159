https://microsoft.github.io/language-server-protocol/overviews/lsp/overview/

LSP 在设计上尽量保证 programming language neutral，即使用 text based 参数来通信，规避语法树和编译类型。
- text document URI 
- cursor position 

```json
// Request
{
    "jsonrpc": "2.0",
    "id" : 1,
    "method": "textDocument/definition",
    "params": {
        "textDocument": {
            "uri": "file:///p%3A/mseng/VSCode/Playgrounds/cpp/use.cpp"
        },
        "position": {
            "line": 3,
            "character": 12
        }
    }
}

// Response
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "uri": "file:///p%3A/mseng/VSCode/Playgrounds/cpp/provide.cpp",
        "range": {
            "start": {
                "line": 0,
                "character": 4
            },
            "end": {
                "line": 0,
                "character": 11
            }
        }
    }
}
```

Supports:
- Goto Definition
- autocomplete
- hover info

#### User Open Document

IDE notifies srever `textDocument/didOpen`. 

> Truth about the document content is no longer on the file system but kept by in IDE memory

Sychronize between IDE and server is needed

#### IDE Edits

IDE notifies server `textDocument/didChange`.

Then language server analyses and notifies IDE with the detected errors and warnings `textDocument/publishDiagnostics`.

#### Go to Definition on a Symbol

IDE sends `textDocument/definition` request with two parameters: 
1. Document URI
2. Text position from where the ‘Go to Definition’ request was initiated to the server. 

Server responds with the document URI and the position of the symbol’s definition inside the document.

#### Close the document (file)

`textDocument/didClose` notification 

The current contents are now up to date on the file system.

