# 浏览器工作原理


- 浏览器渲染的5个步骤
    + 对目标URL发起HTTP请求，并解析HTTP响应，得到对应的HTML文本
    + 对HTML文本进行parse，转换为对应的DOM树
    + 进行CSS COMPUTING，得到带样式的DOM树
    + 计算DOM树上所有元素产生的盒的位置，进行layout
    + 进行渲染，转换为BitMap，提供给显卡驱动

- 有限状态机
    + 每一个状态都是一个机器
        * 所有机器接收的输入是一致的
        * 用函数来表示，其是纯函数。编写有限状态机的代码时，可以完全忽略其他状态机的逻辑
    + 每一个机器都知道下一个状态
        * 摩尔状态机：每个机器都有确定的下一个状态
        * 米利状态机：每个机器根据输入决定下一个状态
    + 可以让状态机的结束状态永远返回结束状态，避免其进入到其他状态

- HTTP协议
    + HTTP协议是文本协议，协议里的内容都是字符串，与二进制协议相对。在TCP通道中传输的，完全是字符
    + HTTP在TCP全双工通讯的基础上，规定了Request-Response的模式。决定了通讯必定是由客户端发起的
    + HTTP的Request分为三部分：
        * Request line：请求的方法、请求的路径和请求的协议和版本号
            * 请求的方法：
                * GET：浏览器通过地址访问页面都是GET方法
                * POST：表单提交产生POST方法
                * HEAD：与GET类似，但只返回请求头，多数由JS发起
                * PUT：添加资源
                * DELETE：删除资源
                * CONNECT：多用于HTTPS和WebSocket
                * OPTIONS、TRACE：多用于调试
        * Request Headers：包含多行，每行是以冒号分割的KV对，行数不固定，以一个空行标注结束
        * Request Body：也是KV对，由Content-Type规定其分隔符。因此HTTP里Content-Type是一个必要的字段，需要有默认值，常见的有：
            * application/json
            * application/x-www-form-urlencoded：用HTML的form标签提交产生的HTML请求，默认产生此格式
            * multipart/form-data：当有文件上传时产生
            * text/xml
        * 所有HTTP里的换行规定都是\r \n
    + HTTP的Response也分为三部分：
        * status line：HTTP协议的版本号、HTTP状态码和HTTP状态文本
            * HTTP状态码：
                * 1xx：临时回应，表示客户端请继续
                * 2xx：请求成功，200
                * 3xx：表示请求的目标有变化，希望客户端进一步处理
                    * 301：永久性跳转
                    * 302：临时性跳转
                    * 304：客户端缓存没有更新。产生这个状态的前提是，客户端本地已近有缓存的版本，并且在Request中告诉了服务端，当服务端通过时间或者tag，发现没有更新的时候，就会返回一个不含body的304状态
                * 4xx：客户端请求错误
                    * 403：无权限
                    * 404：请求的页面不存在
                * 5xx：服务端请求错误
                    * 500：服务端错误
                    * 503：服务端暂时性错误
        * Response Headers：包含多行，每行是以冒号分割的KV对，行数不固定，以一个空行标注结束
        * Response Body：内容中允许存在任何字符，由Content-Type规定其格式。
            * node默认格式是chunk body。由一个16进账的数字单独占一行，后面跟着内容部分，再后面又是一个16进制的数字。直到最后一个16进制的0，得到空的chunk，标注Body的结束