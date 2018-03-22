##理解BFC特效下的多栏自适应布局
>之前一直没有听说过bfc布局系统今天好好研究一下
>
>先来概念:
>
>BFC全称是'block formatting context'中文为 块级格式化上下文,原则是BFC元素表现的特性原则是内部子元素再怎么翻江倒海都不会影响外部元素.所以可以避免margin穿透清楚浮动;
>
>BFC的触发常见的属性如下
>
>float值不为none
>
>overflow:的值为auto,scroll或者hidden
>
>display的值为table-cell,table-caption,inline-block中任何一个
>
>postion值部位relatice和static;
>
####BFC的特性有非常多 但是这里我们之关心最重要的一个 就是和float元素相邻的时候
>今天先写到这里 突然发现这个东西好像用html写好些 哈哈哈..先这样明天改成html