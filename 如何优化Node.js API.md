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

### 2. 避免在 API 中使用 session 和 cookie，仅在 API 响应中发送数据
当我们使用 cookie 或者 session 来存储临时状态的时候，会占用非常多的服务器内存。

现在通用无状态 API，并且也有 JWT、OAuth 等验证机制。验证令牌保存在客户端以便服务器管理状态。

JWT 是基于 JSON 的用于 API 验证的安全令牌。JWT 可以被看到，但一旦发送就无法修改。JWT 只是一个序列并没有加密。OAuth 不是 API 或服务——相反，它是授权的开放标准。OAuth 是一组用于获取令牌的标准步骤。

同时，也不要把时间浪费在使用 Node.js 来服务静态文件。这方面 NGINX 和 Apache 做得更好。

使用 Node 搭建 API 的时候，不要在响应中发送完整的 HTML 页面。当仅有数据通过 API 发送的时候，Node 服务得会更好。大部分 Node 应用都使用 JSON 数据。
