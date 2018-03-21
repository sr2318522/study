##ie6经典bug
1.ie6怪异磊子模型将padding和border算入宽高
>原因:未加文档声明造成非盒子模型解析
>
>解决办法:加入文档声明<!doctype html>

2.ie6在块元素,左右浮动,设定margin时造成margin双倍边距
>解决方法 :display:inline;

3.以下三种其实是同一种bug,其实也不算是bug,举个例子:父标签高度20,子标签11,垂直居中,20-11=9,9要分给文字上面与下面ie6就会不同所以尽量避免
>1字体大小为奇数之边框高度少1px
>解决方法:字体大小设置为偶数或者line-height为偶数
>2line-height,文本垂直居中差1px
>解决方法:padding-top代替line-height居中或者line-height+1或者-1
>3父标签的宽度的奇数偶数不同的居中造成1px的偏离
>解决方法:如果父标签是奇数宽度,则子标签也用奇数宽度;如果父标签是偶数宽度,则子标签也是偶数宽度

4.内部盒模型超出父级时,父级被撑大
>解决办法:父级标签使用overflow:hidden

5.line-height默认行高bug
>解决办法:line-height设置

6.行标签之间会有一小段空白
>解决办法:float或者结构并排

7.标签高度无法小于19px
>解决方法:overflow:hidden;

8.左浮元素margin-bottom失效
>解决方法:显示设置高度或者父标签设置padding-bottom代替子标签的margin-bottom或者再放个标签让父标签浮动,子标签margion-bottom,既(margin-bottom与float不同时作用域一个标签)

9.img于块元素中,底边多出空白
>解决方法:父级设置overflow:hidden或者img{display:black}或者margin:-5px;

10.li之间有间距
>解决方法:float:left;

11.块级元素中文字及有浮动的行元素,行元素换行
>解决方法:讲元素至于块元素内的文字前

12.postion下的left,bottom错误
>解决方法:为父级relative设置宽高或者添加zoom:1

13.子级中设置postion,则父级overflow失效
>解决方法:为父级设置position:relative;