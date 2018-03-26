###IFC
IFC为内敛格式化上下文 IFC的line 剥削 高度由其包含内行元素汇总最高的实际高度来计算,不受竖直方向上的padding margin影像

>IFC中的line box 一般左右都贴近整个IFC,但是会因为float元素而扰乱.float元素会位于IFC与line box之间 是的line box宽度缩短 同个IFC下的多个line box高度会不同.IFC中时不可能有会计元素的,当插入块级元素时,会产生两个匿名块与div分割开,即产生两个IFC,每个IFC对外表现为会计元素,与div垂直排列