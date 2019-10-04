# HTTP

## 缓存
Web缓存有：数据库缓存、服务器缓存、浏览器缓存。
浏览器缓存有：*HTTP缓存*、indexDB、cookie、localstorage等。

## 浏览器加载页面的简单流程
1. 浏览器先根据这个资源的http头信息来判断是否命中强缓存。如果命中则直接加在缓存中的资源，并不会将请求发送到服务器。
2. 如果未命中强缓存，则浏览器会将资源加载请求发送到服务器。服务器来判断浏览器本地缓存是否失效。若可以使用，则服务器并不会返回资源信息，浏览器继续从缓存加载资源。
3. 如果未命中协商缓存，则服务器会将完整的资源返回给浏览器，浏览器加载新资源，并更新缓存。
---
## 强缓存
* 使用HTTP返回头中的*Expires*和*Cache-Control*来控制缓存的有效时间。
* Expires用的是绝对时间，可能会随着系统时间的变化而出错。Cache-Control用的是相对时间。二者同时启用时，Cache-Control优先级更高。
---
## 协议缓存
若未命中强缓存，则浏览器会将请求发送至服务器。服务器根据http头信息中的Last-Modify / If-Modify-Since或Etag / If-None-Match来判断是否命中协商缓存。如果命中，则HTTP返回码为304，浏览器从缓存中加载资源。
### Last-Modify / If-Modify-Since
1. 浏览器第一次请求某个资源时，服务器返回的header中会加入Last-Modify，表示该资源的最后修改时间。
2. 浏览器再次请求时，请求头中会包含If-Modify-Since，值为之前收到的Last-Modify。服务器收到If-Modify-Since后，判断是否命中缓存。
3. 若命中缓存，则服务器返回HTTP304，并且不会返回Last-Modify。
4. 这种方式不是很准确，所以出现了ETag / If-None-Match。原因：
	1. Last-Modified只能精确到秒级。
	2. 某些文件会定期生成，内容没有变化但是Last-Modified变了，导致未命中。
	3. 服务器可能会无法获取准确的文件修改时间。
### ETag / If-Modify-Since
ETag / If-Modify-Since返回的是一个校验码（ETag：entity tag）。ETag保证每个资源是唯一的，资源变化会导致ETag的变化。服务器根据浏览器上发送的if-None-Match来判断是否命中。
---
## HTTP访问执行时的缓存流程
第一次访问
![]()
第二次访问
![]()

