1. postMessage用法问题：
```
event.source.postMessage(data,event.origin)
```
是什么意思？自己给自己发消息吗？
解答：`event.source.postMessage` 是给消息来源发回信，不是自己给自己发消息。

## 结构
![9a6187e83203f6d96acf577ca8955f1c.png](../_resources/9a6187e83203f6d96acf577ca8955f1c.png)

