

------

## **2. HTTP/2 的 5 大核心优化**

### **1️⃣ 采用二进制帧（Frame），提高解析效率**

- **HTTP/1.1** 采用 **文本格式**（需要逐行解析）。
- **HTTP/2** **使用二进制格式**，计算机解析更快，减少字符串处理开销。

------

### **2️⃣ 多路复用（Multiplexing），多个流并行**

- **HTTP/1.1：一个 TCP 连接一次只能处理一个请求**
    - **多个请求排队，等待前一个请求完成**（HOL 阻塞）。
    - **必须建立多个 TCP 连接** 才能并行处理多个请求，增加服务器负担。
- **HTTP/2：多路复用**
    - **多个请求共享一个 TCP 连接**。
    - **数据拆分成二进制帧，交错传输**，提高并发能力。
    - **解决了 HTTP 层面的队头阻塞问题**。

------

### **3️⃣ 头部压缩（HPACK），减少冗余数据**

- **HTTP/1.1：每个请求都带完整的 Header**
    - 例如，多个请求都要带 `User-Agent`、`Cookie` 等头部字段，浪费带宽。
- **HTTP/2：HPACK 头部压缩**
    - **只发送变化的头部信息**，相同的部分只需传输一次。
    - **减少带宽占用，加快请求速度**。

------

### **4️⃣ 服务器推送（Server Push）**

- **HTTP/1.1：客户端必须主动请求资源**
    - 例如，浏览器请求 `index.html` 后，解析发现需要 `style.css`，再发送请求获取 CSS。
- **HTTP/2：服务器推送**
    - 服务器可以**提前推送 CSS、JS 等资源**，客户端无需再发请求，减少等待时间。
    - **提升页面加载速度，优化用户体验**。

------

### **5️⃣ 请求优先级（Priority）**

- **HTTP/1.1**：所有请求的优先级基本一致，无法优化关键资源的加载顺序。
- **HTTP/2**：可以给**重要资源（如 CSS、HTML）更高的优先级**，确保它们先加载，提高渲染速度。

------

## **3. HTTP/1.1 的优化手段（但仍不如 HTTP/2）**

为了提高 HTTP/1.1 的性能，开发者通常采用：

1. **使用多个 TCP 连接**（浏览器一般允许 6-8 个并发 TCP 连接）。
2. **使用连接池（Keep-Alive）**，保持长连接，避免重复握手。
3. **合并文件**（CSS Sprites、合并 JS、CSS），减少 HTTP 请求次数。
4. **使用 CDN 加速**，降低服务器压力。
5. **使用域名分片（Sharding）**，绕过浏览器的 TCP 连接限制。

但这些方式都有一定的**局限性**，而 HTTP/2 通过协议级优化**根本上解决了问题**。

------

## **4. HTTP/2 的不足**

1. **仍然依赖 TCP**
    - TCP 层如果发生丢包，会导致所有 HTTP/2 流被阻塞（TCP 层的队头阻塞）。
    - **HTTP/3（基于 QUIC，UDP 协议）** 解决了这个问题。
2. **老旧设备或代理服务器不完全支持**
    - 例如，一些 **老的网络设备**（防火墙、代理服务器）可能不支持 HTTP/2，需要降级到 HTTP/1.1。
3. **服务器推送（Server Push）可能滥用**
    - **如果服务器推送了用户不需要的资源，反而浪费带宽**。

------

## **5. HTTP/1.1 vs HTTP/2 总结表**

| **对比项**                    | **HTTP/1.1**                         | **HTTP/2**                            |
| ----------------------------- | ------------------------------------ | ------------------------------------- |
| **连接数**                    | **多个 TCP 连接** 处理多个请求       | **一个 TCP 连接** 处理所有请求        |
| **数据格式**                  | **文本格式**（Header & Body）        | **二进制帧（Frame）**，计算机更易解析 |
| **多路复用**                  | ❌ **不支持**，必须等待前一个请求完成 | ✅ **支持**，多个请求并行传输          |
| **队头阻塞（HOL Blocking）**  | ✅ **存在**，影响并发性能             | ✅ **HTTP 层解决**，仍受 TCP 阻塞影响  |
| **Header 体积**               | ❌ **不压缩**，冗余数据多             | ✅ **HPACK 压缩**，减少带宽占用        |
| **服务器推送（Server Push）** | ❌ **不支持**                         | ✅ **支持**，服务器可主动推送资源      |
| **请求优先级**                | ❌ **不支持**                         | ✅ **支持**，可优化关键资源加载顺序    |
| **安全性**                    | HTTP 或 HTTPS                        | **必须使用 HTTPS**                    |
| **兼容性**                    | 兼容性好，所有设备都支持             | 需要浏览器、服务器都支持 HTTP/2       |

------

## **6. 结论：为什么 HTTP/2 更优？**

✅ **HTTP/2 彻底优化了 HTTP/1.1 的核心问题**：

1. **多路复用（Multiplexing）** 解决 HTTP 层的队头阻塞，提高并发能力。
2. **二进制格式（Binary Frame）** 让数据传输更高效。
3. **HPACK 头部压缩** 减少了冗余数据，提高带宽利用率。
4. **服务器推送（Server Push）** 让关键资源提前加载，减少等待时间。
5. **请求优先级（Priority）** 让 CSS、HTML 先于 JS 加载，提高渲染速度。











作为一只前端开发🐶，HTTP是我们知识地图里面必不可少的一部分，也是面试必问知识点。HTTP2号称可以让我们的应用更快、更简单、更稳定，它完美解决了1.1版本的诸多问题，本文和大家一起聊聊HTTP2的改进点。

## HTTP发展史

正式讲HTTP2之前我们先讲一下HTTP的发展史。

![img](/Users/siwenfeng/siwenfeng/newBeginning/八股文/assets/167926d367568b74~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)



- HTTP/0.9 - 单行协议
    HTTP于1990年问世，那时候HTTP非常简单：只支持GET方法；没有首部；只能获取纯文本。
- HTTP/1.0 - 搭建协议的框架
    1996年，HTTP正式被作为标准公布，版本为HTTP/1.0。1.0版本增加了首部、状态码、权限、缓存、长连接（默认短连接）等规范，可以说搭建了协议的基本框架。
- HTTP/1.1 - 进一步完善
    1997年，1.1版本接踵而至。1.1版本的重大改进在于默认长连接；强制客户端提供Host首部；管线化；Cache-Control、ETag等缓存的相关扩展。

## 目前存在的问题

现在我们先不聊HTTP2, 看一下HTTP发展到1.1存在有哪些问题：

1. 线头阻塞：TCP连接上只能发送一个请求，前面的请求未完成前，后续的请求都在排队等待。
2. 多个TCP连接
    虽然HTTP/1.1管线化可以支持请求并发，但是浏览器很难实现，chrome、firefox等都禁用了管线化。所以1.1版本请求并发依赖于多个TCP连接，建立TCP连接成本很高，还会存在慢启动的问题。
3. 头部冗余，采用文本格式
    HTTP/1.X版本是采用文本格式，首部未压缩，而且每一个请求都会带上cookie、user-agent等完全相同的首部。
4. 客户端需要主动请求

## HTTP/2.0的时代来了

先来一个demo感受一下吊炸天的HTTP/2.0，这个demo是加载379张图片，来对比HTTP/1.1和HTTP/2.0的性能。 [HTTP/1.1 与2.0 性能比较](https://link.juejin.cn?target=https%3A%2F%2Fhttp2.akamai.com%2Fdemo)



![img](/Users/siwenfeng/siwenfeng/newBeginning/八股文/assets/1679297f1e8fa222~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)



理论上HTTP/2.0会比HTTP/1.1有一倍多的性能提升，弱网环境下，性能提升会更加明显。 下面两张图是我在设置网络在fast 3G 和slow 3G的性能对比。



![img](/Users/siwenfeng/siwenfeng/newBeginning/八股文/assets/16792a212d285ac5~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)

![img](/Users/siwenfeng/siwenfeng/newBeginning/八股文/assets/16792a30e45442fd~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)

是不是被HTTP/2.0的速度亮瞎了双眼？2333，接下来我们正式开始聊聊2.0。看看2.0 相比与1.1的一些重大改进。



### 二进制分帧层

HTTP2性能提升的核心就在于二进制分帧层。HTTP2是二进制协议，他采用二进制格式传输数据而不是1.x的文本格式。



![img](/Users/siwenfeng/siwenfeng/newBeginning/八股文/assets/16792b2d88c55af5~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0-2789030.awebp)

看图吧！很清晰的表达了HTTP/1.1的响应和2.0的区别。1.1响应是文本格式，而2.0把响应划分成了两个帧，图中的HEADERS（首部）和DATA（消息负载） 是帧的类型。[了解更多帧的类型](https://link.juejin.cn?target=https%3A%2F%2Fwww.ibm.com%2Fdeveloperworks%2Fcn%2Fweb%2Fwa-http2-under-the-hood%2Findex.html)也就是说一条HTTP响应，划分成了两个帧来传输，并且采用二进制来编码。



这里我们来提三个概念。

- 流（Stream）：已建立的TCP连接上的双向字节流，可以承载一个或多个消息。
- 消息（Message）：一个完整的HTTP请求或响应，由一个或多个帧组成。特定消息的帧在同一个流上发送，这意味着一个HTTP请求或响应只能在一个流上发送。
- 帧（Frame）：通信的基本单位。
    一个TCP连接上可以有任意数量的流。

### 多路复用

上面提到HTTP/1.1的线头阻塞和多个TCP连接的问题，HTTP2的多路复用完美解决。HTTP2让所有的通信都在一个TCP连接上完成，真正实现了请求的并发。我们来看一下HTTP2具体是怎么实现的：



![img](/Users/siwenfeng/siwenfeng/newBeginning/八股文/assets/16795da0e0340fcb~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)

HTTP2建立一个TCP连接，一个连接上面可以有任意多个流（stream），消息分割成一个或多个帧在流里面传输。帧传输过去以后，再进行重组，形成一个完整的请求或响应。这使得所有的请求或响应都无法阻塞。 我们再来回看上面的那个demo:





![img](/Users/siwenfeng/siwenfeng/newBeginning/八股文/assets/16795df3ae0fe562~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)

打开控制台可以看到，HTTP/1.1的方式，后面的图片的加载时间主要耗时在stalled，stalled的意思是从TCP连接建立完成，到真正可以传输数据之间的时间差。这就是队头阻塞，前面的请求没有处理，后面的请求都在排队等待。



这里例子我们能很直观的看到就是多路复用起到的优化作用。因为HTTP2 实现了请求并发，后面的请求不用再等待，加载时长当然少了很多。截一张HTTP2的图片加载耗时详情来看看（要看比较靠后的请求）：

![img](/Users/siwenfeng/siwenfeng/newBeginning/八股文/assets/16795e9624ceb826~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)

咦？？什么情况？我们发现后面的很多请求依旧有在排队哎，只是排队的时间相对1.1少了很多。一个TCP连接可以有任意数量的流，也就是同时可以并发任意数量的请求啊，为啥还会排队呢？原因就是请求太多时，浏览器或服务器会受不了，这超出了它的处理能力。流控制帮我们解决了这个问题，流控制会管理数据的传输，允许接收者停止或减少发送的数据量，免得接收方不堪重负。所以请求太多时，还是会存在排队等待的问题，因为不管是客户端或服务器端，能同时处理请求或响应都是有限的。



### 头部压缩

头部压缩也是HTTP2的一大亮点。在1.X版本中，首部用文本格式传输，通常会给每个传输增加500-800字节的开销。现在打开一个网页上百个请求已是常态，而每个请求带的一些首部字段都是相同的，例如cookie、user-agent等。HTTP2为此采用HPACK压缩格式来压缩首部。头部压缩需要在浏览器和服务器端之间：

- 维护一份相同的静态字典，包含常见的头部名称，以及常见的头部名称和值的组合
- 维护一份相同的动态字典，可以动态的添加内容
- 通过静态Huffman编码对传输的首部字段进行编码

HTTP2的静态字典是长这个样子的（只截取了部分，[完整表格在这里](https://link.juejin.cn?target=https%3A%2F%2Fhttpwg.org%2Fspecs%2Frfc7541.html%23static.table.definition)）：



![img](/Users/siwenfeng/siwenfeng/newBeginning/八股文/assets/16797167fe0328dc~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)

所以我们在传输首部字段的时候，例如要传输method:GET,那我们只需要传输静态字典里面method:GET对应的索引值就可以了，一个字节搞定。像user-agent、cookie这种静态字典里面只有首部名称而没有值的首部，第一次传输需要user-agent在静态字典中的索引以及他的值，值会采用静态Huffman编码来减小体积。



第一次传输过user-agent 之后呢，浏览器和服务器端就会把它添加到自己的动态字典中。后续传输就可以传输索引了，一个字节搞定。

我们用WireShark来抓包验证一下：
 HTTP2目前都是HTTPS的请求，WireShark对HTTPS网站抓包解密请参考[这里](https://link.juejin.cn?target=https%3A%2F%2Fimququ.com%2Fpost%2Fhttp2-traffic-in-wireshark.html)。

- 首次传输user-agent和第二次传输user-agent

    ![img](/Users/siwenfeng/siwenfeng/newBeginning/八股文/assets/167971ecaa21c01d~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)



![img](/Users/siwenfeng/siwenfeng/newBeginning/八股文/assets/167971f539251b99~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)

由于第一次传输的时候，字典里面并没有user-agent的值，这时候user-agent是63字节，第二次传输时，他已经在动态字典里面了，只传索引，一个字节搞定。



- HPACK的首部压缩力度



![img](/Users/siwenfeng/siwenfeng/newBeginning/八股文/assets/16797238b3484918~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)

Header解码后的长度有471个字节，而HEADERS流只有246个字节。这只是第一个请求，后续的请求压缩力度会更大，因为前面请求用到的首部（静态字典中没有的）会添加到动态字典中，使得后续请求只需要传输字典里面的索引。





![img](/Users/siwenfeng/siwenfeng/newBeginning/八股文/assets/1679728cf8cbf534~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0-20250324123740288.awebp)



### 服务器端推送

服务器端推送使得服务器可以预测客户端需要的资源，主动推送到客户端。
 例如：客户端请求index.html，服务器端能够额外推送script.js和style.css。 实现原理就是客户端发出页面请求时，服务器端能够分析这个页面所依赖的其他资源，主动推送到客户端的缓存，当客户端收到原始网页的请求时，它需要的资源已经位于缓存。

针对每一个希望发送的资源，服务器会发送一个PUSH_PROMISE帧，客户端可以通过发送RST_STREAM帧来拒绝推送（当资源已经位于缓存）。这一步的操作先于父响应（index.html），客户端了解到服务器端打算推送哪些资源，就不会再为这些资源创建重复请求。当客户端收到index.html的响应时，script.js和style.css已经位于缓存。

