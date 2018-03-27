###IFC
IFC为内敛格式化上下文 IFC的line 剥削 高度由其包含内行元素汇总最高的实际高度来计算,不受竖直方向上的padding margin影像
>####IFC如何生成:IFC只有在一个块级元素中仅包含内敛级别元素才会生成
>
>##布局规则
>1.内部的盒子会在水平方向,一个接一个的放置
>
>2.这些盒子垂直方向上的起点从包含块盒子的顶部开始
>
>3.摆放这些盒子的时候,它们会在水平方向上的padding border margin 所占用的空间都会被考虑在内
>
>4.垂直方向上,这些框可能会以不同的形式来对齐 它们可能会会使用底部或者顶部对齐,也可能通过其内部的文本基线对齐
>
>5.能再一行上的框都完全包含进去的一个矩形区域,被称为该行的行框,行框的高度由包含块和存在的浮动来决定
>
>6.IFC中的line box 一般左右都贴紧齐包含快,但是会因为float元素的存在发生变化.float回溯会位于IFC与line box 之间 使得line box宽度缩短
>
>7.IFC中的line box高度由css行高计算规则来确定,同一个ifc下多个line box 高度可能会不同
>
>8.当inline level boxes 的总宽度少于包含他们的line box时 其文字渲染规则由 text-align属性来确定
>
>9.当一个inline box 超过line box总宽度时他会被分割成多个boxes 这些boxea 被分布在多个line box 里 如果一个inline box 不能被分割那么这个inline box] 将溢出这个linebox
>
>10.IFC中的line box 一般左右都贴近整个IFC,但是会因为float元素而扰乱.float元素会位于IFC与line box之间 是的line box宽度缩短 同个IFC下的多个line box高度会不同.IFC中时不可能有会计元素的,当插入块级元素时,会产生两个匿名块与div分割开,即产生两个IFC,每个IFC对外表现为会计元素,与div垂直排列
>
>##那么IFC的具体实用在何处呢
>
>1.水平居中:当块要在环境中水平居中时,设置其为inline-block 则会在外层产生一个IFC通过设置父容器的text-align:center则可以使其水平居中
>
>2垂直居中:创建一个IFC 用其中一个元素撑开父元素高度,然后设置其vertical-align:middle 其他行内元素可以在此父元素下垂直居中;