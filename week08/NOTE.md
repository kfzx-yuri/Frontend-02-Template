# 重学HTML

- HTML的定义
    + 实体定义：&后面跟一系列字符，结尾为;
    + 常用的实体定义：
        * &nbsp：no-break space，使用其去连接2个词时，会将它们连接为1个词，破坏了语义，可能造成排版问题
                 不推荐使用。推荐使用CSS的white-space属性来显示空格
        * &lambda
        * &quot：双引号
        * &amp：&符
        * &lt：<符
        * &gt: >符
    + HTML的三个常用namespace：
        * html namespace
        * MathML namespace
        * SVG namespace

- HTML标签语义
    + &lt;aside&gt;：定义article标签(页面主体)以外的内容，可用来包裹侧边栏
    + &lt;a&gt;：包裹超链接
    + &lt;main&gt;：定义页面的主体内容部分，整个HTML只有一个main标签
    + &lt;article&gt;：定义文章
    + &lt;hgroup&gt;：当有几个标题共同组成页面完整标题时，用其包裹
    + &lt;hr&gt;：横线，表示场景发生转换
    + &lt;abbr&gt;：表示缩写
    + &lt;strong&gt;：表示某个词在整篇文章中的重要性，不改变语义
    + &lt;em&gt;：表示在一个句子中重点强调的词
    + &lt;figure&gt;：带文字或者带说明的图表信息
        * <figure>
            <img></img>
            <figcaption>note</figcaption>
          </figure>
    + &lt;ol&gt;：定义有序列表，使用css的counter来改变列表序
    + &lt;nav&gt;：导航
    + &lt;dfn&gt;：表示定义项目
    + &lt;pre&gt;：表示预格式文本，这样不会被当成普通文本处理
    + &lt;samp&gt;：例子
    + &lt;code&gt;：代码文本
    + &lt;footer&gt;：不属于页面主体内容的部分，且位于页脚
    + 为了使html文本里的html标签不会被处理成html代码，需要使用&lt和&gt转义
    + 使用<p>标签和class结合，来处理缺乏对应标签的语义

- HTML语法
    + 合法元素：
        * Element：&lt;tagname&gt;...&lt;/tagname&gt;
        * Text：text
        * Comment：<!-- comments -->
        * DocumentType：&lt;!Doctype html&gt;
        * ProcessingInstruction：&lt;?a 1?&gt;，预处理语法，把1传给a处理器。只有第一个空格有分割作用，其余空格是数据
        * CDATA：&lt;![CDATA[]]&gt;，文本节点的另一种表示法
    + 字符引用：
        * &#161：代表使用ASCII码为161的字符
        * &amp;
        * &lt;
        * &quot;

- 浏览器API
    + DOM API：分为4个部分：
        * traversal系列的API：可以访问DOM树所有节点的自动迭代工具，不推荐使用
        * 节点部分的API：
            * DOM树上的节点都统一继承自Node类
            * 导航类操作：分为节点的导航和元素的导航
                节点的导航：
                    parentNode：寻找父节点
                    childNodes：寻找子节点
                    firstChild：寻找第一个子节点
                    lastChild：寻找最后一个子节点
                    nextSibling：寻找下一个邻居节点
                    previousSibling：寻找上一个邻居节点
                元素的导航：会忽略文本节点，因此可避免获得空白文本
                    parentElement：寻找父元素，实际上与parentNode等价。因为非Element的Node不会有子节点
                    children：寻找子元素
                    firstElementChild：寻找第一个子元素
                    lastElementChild：寻找最后一个子元素
                    nextElementSibling：寻找下一个邻居元素
                    previousElementSibling：寻找上一个邻居元素
            * 修改类操作：实际上针对的也是Element，因为非Element的Node不会有子节点
                appendChild(node)：往最后插入元素
                insertBefore(newNode,referenceNode)：将newNode插入到referenceNode之前
                removeChild(node)：移除node节点，但仍保留在内存中，可以用额外变量保存此引用。一个节点的移除只能在其父节点上进行，无法移除本身
                replaceChild(newNode,oldNode)：先删除后替换
            * 高级操作：
                compareDocumentPosition：用于比较两个节点中关系的函数。
                ![image](https://github.com/kfzx-yuri/Frontend-02-Template/blob/master/week08/compareDocumentPosition-1.png)
                ![image](https://github.com/kfzx-yuri/Frontend-02-Template/blob/master/week08/compareDocumentPosition-2.png)
                contains：检查一个节点是否包含另一个节点
                ![image](https://github.com/kfzx-yuri/Frontend-02-Template/blob/master/week08/contains.png)
                isEqualNode：检查两个节点是否完全相同
                ![image](https://github.com/kfzx-yuri/Frontend-02-Template/blob/master/week08/isEqualNode.png)
                isSameNode：检查2个节点是否是同一个节点(MDN上标注已被DOM level4废弃)，可以用"==="代替
                cloneNode：复制一个节点，如果传入参数true，会连同子元素一起做深拷贝
                ![image](https://github.com/kfzx-yuri/Frontend-02-Template/blob/master/week08/cloneNode.png)

        * 事件部分的API：JS与HTML元素做交互
           * addEventListener：三个参数：type,listener,options
            其中,type为事件类型，listener为监听器，options有2种形态：
            一种是true或false，表示事件模式是冒泡模式或捕获模式，另一种可以传入3个参数：capture，once和passive。
                once表示事件只捕获一次，
                passive表示只监听事件，不会产生副作用。但是当在事件发生时想阻止事件的默认行为，需要置为true

            * 事件的冒泡与捕获：与事件的监听无关，2个过程在事件触发过程中都会发生。
                             捕获过程是从外向内，是计算机处理事件的逻辑；
                             冒泡过程是从内向外，是人类处理事件的逻辑
            
        * Range API：比节点部分的API功能更强大，性能更好，但应用性较差
            * HTML文档流中有起始点和终止点的一段范围，可以跨层级，起点先于终点即可
            * 创建Range：
                var range = new Range();
                range.setStart(element,offset);
                range.setEnd(element,offset);
                var range = document.getSelection().getRangeAt(0);
                range.setStartBefore();
                range.setEndBefore();
                range.serStartAfter();
                range.setEndAfter();
                range.selectNode();
                range.selectNodeContents():
                range.extractContents()：返回一个fragment。对应内容会从DOM树中移除
                range.insertNode(document.createTextNode("aaa"))
    + CSSOM：CSS对象模型
        * document.styleSheets：获取当前页面的CSS，包括以style标签嵌入和以link标签引入的CSS
        * document.styleSheets[0].cssRules
        * document.styleSheets[0].insertRule("p {color:green}",0)
        * document.styleSheets[0].removeRule(0)
        * CSSStyleRule对象：分为2部分
            * selectorText String
            * KV对
        * window.getComputedStyle(ele,presudoElt)：
            * 获取ele元素经过CSS计算后的合成样式
            * presudoElt是可选参数，传入的是一个伪元素，如"::before"
    * CSSOM View：
        * windowAPI：
            * window.innerHeight/innerWidth：viewport使用的宽高，即HTML内容渲染所使用的区域宽高
            * window.outerHeight/outerWidth：浏览器窗口(包含浏览器所带工具栏及其他内容)的宽高，基本无用
            * window.devicePixelRatio：物理设备的屏幕上的像素与逻辑像素px的比值。重要，有些设备不是1:1
            * window.screen.width/height/availWidth/availHeight：屏幕相关的信息，与物理硬件相关
            * window.open("about:blank","_blank","width=100,height=100,left=100,right=100")
                * moveTo(x,y)：将窗口移动到对应位置
                * moveBy(x,y)：在现有位置的基础上移动
                * resizeTo(x,y)：调整窗口大小
                * resizeBy(x,y)：在现有大小的基础上调整窗口大小
        * scrollAPI：分为scroll元素相关的API 和window相关的scroll API
            * scroll元素的API：
                * scrollTop：获取滚动条所在位置
                * scrollLeft：获取滚动条所在位置
                * scrollWidth：可滚动内容最大的宽度
                * scrollHeight：可滚动内容最大的高度
                * scroll(x,y)：滚动到特定位置
                * scrollBy(x,y)：在当前位置基础上滚动
                * scrollIntoView()：滚动到当前屏幕可见区域
            * window相关的API：
                * scrollX：等价于scrollTop
                * scrollY：等价于scrollLeft
                * scroll(x,y)
                * scrollBy(x,y)
        * layout相关的API：
            * getClientRects()：在每个元素上调用该方法获取它生成的所有盒
            * getBoundingClientRect()：取包裹当前元素的盒，可以用来计算父元素和子元素间的相对位置
        


