# 重学CSS

- CSS2.1语法
    + @charset
    + @import
    + rules(对顺序没有要求):
        * @media
        * @page
        * ruleset

- CSS总体结构
    + at-rules:
        * @charset：用于提示CSS文件使用的字符编码方式，必须出现在最前。该规则只在语法解析阶段使用
        * @import：用于引入一个CSS文件，但是@charset规则不会被引入。
                    @import [ <url> | <string> ]
                            [ supports( [ <supports-condition> | <declaration> ] ) ]?
                            <media-query-list>? ;
        * @meida：能对设备的类型进行判断，对设备指定CSS样式
                    @media print {
                        body { font-size: 10pt }
                    }
        * @page：用于分页媒体访问网页时的表现设置，页面是一种特殊的盒模型结构，除了页面本身，还可以设置它周围的盒
        * @counter-style：用于定义列表项的表现
                    @counter-style triangle {
                    system: cyclic;
                    symbols: ‣;
                    suffix: " ";
                    }
        * @keyframes：用于定义动画关键帧
                    @keyframes diagonal-slide {
                        from {
                            left: 0;
                            top: 0;
                        }

                        to {
                            left: 100px;
                            top: 100px;
                        }
                    }
        * @fontface：用于定义一种字体
        * @support：检查环境特性，目前兼容性不好
        * @namespace：用于跟 XML 命名空间配合的一个规则，表示内部的 CSS 选择器全都带上特定命名空间
    + rule
        + Selector
            + selector_group
            + selector
                * >
                * <sp>
                * +
                * ~
            + simple_selector
                * type
                * *
                * .
                * #
                * []
                * :
                * ::
                * :not()
        + Declaration
            + key
                * variables
                * properties
            + value
                * calc
                * number
                * length

- CSS选择器
    + 简单选择器
        * *：通用选择器
        * div ：类型选择器，对应元素的tagName属性
        * .：class选择器
        * #：id选择器
        * [attr=value]：属性选择器
        * :：伪类选择器，表示元素的特殊状态，如:hover
            + 最早的一批伪类是针对链接/行为设置的
                * :any-link：可匹配任何的超链接
                * :link :visited：link匹配的是还没访问过的超链接，因此加是visited表示访问过的超链接
                * :hover：鼠标放置时的状态
                * :active：激活状态
                * :focus：焦点在的状态
                * :tager：给作为锚点的a使用的
            + 树结构
                * :empty：当前元素是否有子元素
                * :nth-child()：表示该元素是父元素的第几个子元素
                * :nth-last-child()：表示该元素是父元素的倒数第几个子元素
                * :first-child、:last-child、:only-child
            + 逻辑型
                * :not
        * ::：伪元素选择器
            * ::before：在元素内容之前
            * ::after：在元素内容之后
            * ::first-line：选中第一行，已经是排版完成的结果
            * ::first-letter：选中第一个字母
    + 复合选择器
        * <简单选择器><简单选择器>
        * 通用选择器或者类型选择器如果有，必须写在最前面，伪类/伪元素选择器必须写在最后
    + 复杂选择器
        * <复合选择器><sp><复合选择器>：子孙选择器，左边元素必须是右边元素的祖先元素
        * <复合选择器>">"<复合选择器>：父子选择器，左边元素必须是右边元素的直接上级元素
        * <复合选择器>"~"<复合选择器>：通用兄弟选择器
        * <复合选择器>"+"<复合选择器>：相邻兄弟选择器
        * <复合选择器>"||"<复合选择器>
    + 选择器的优先级
        * 是一个四元组,[0,0,0,0]。从左到右优先级递减
        * 第一位是inlie的位置：写在style标签上的CSS属性，记为1
        * 第二位是id的位置
        * 第三位是class的位置，包括属性选择器、伪类选择器
        * 第四位是tagName的位置，包括伪元素选择器
        * 通用选择器*以及+、-、~、<sp>、:not不会影响到选择器的优先级，但是:not()内部声明的选择器会影响到优先级