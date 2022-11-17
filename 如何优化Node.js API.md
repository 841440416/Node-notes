# API 优化到底指的是什么

优化包含了改善 `API 的响应时间`。响应时间越短，API 的速度越快。
我将在本文分享一些技巧，帮助你`缩短响应时间`、`降低延迟`、`管理错误和吞吐量`，并且最大限度地`减少 CPU 和内存`的使用

## 如何优化 Node.js 的 API

### 1. 始终使用异步函数

异步函数就像 JavaScript 的心脏。因此，优化 CPU 使用率的最佳方法就是编写异步函数来执行非阻塞 I/O 操作。
I/O 操作包括对数据的读和写。它可以在数据库、云存储或者任何本地磁盘上进行。
在大量使用 I/O 操作的应用使用异步函数可以提高效率。因为由于没有阻塞 I/O，当一个请求在做输入/输出操作的时候，CPU 可以同时处理多个请求。
举例如下：
```js
var fs = require('fs');
// 执行阻塞I/O
var file = fs.readFileSync('/etc/passwd');
console.log(file);

// 执行非阻塞I/O
fs.readFile('/etc/passwd', function(err, file) {
    if (err) return err;
    console.log(file);
});
```
- 使用 Node 包 fs 来处理文件
- readFileSync() 是同步函数，会在执行完成前阻塞线程
- readFile() 是异步函数，会立刻返回并在后台运行