## 内联元素与流
> 内联元素默认是基线对齐的
### 字母 x——CSS 世界中隐匿的举足轻重的角色
> 我们这里的字母 x 就是 26 个英文字母中的 x。由于自身形态的一些特殊性，这个小小的不起眼的字母担当大任，在 CSS 世界中扮演了一个重要的角色。可能有人的第一反应是：“我知道，可以模拟关闭按钮的那个叉叉效果！”这位朋友思维很活跃，但是，我们这里说的并不是字母 x 在 CSS 世界中的奇技淫巧，而是正统的术语上的紧密联系。

#### 字母 x 与 CSS 世界的基线
> 内联相关模型中，凡是涉及垂直方向的排版或者对齐的，都离不开最基本的基线
> 基线又是如何定义的吗？基线的定义就离不开本文的主角 x。字母 x 的下边缘（线）就是我们的基线。参见[如图](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7tzjryecnj30cd05nq35.jpg)所示的标示

#### 字母 x 与 CSS 中的 x-height
> CSS 中有一个概念叫作 x-height，指的是字母 x 的高度,通俗地讲，x-height 指的就是小写字母 x 的高度，术语描述就是基线和等分线（mean line）（也称作中线，midline）之间的距离
> 最典型的代表就是 verticalalign:middle。这里的 middle 是中间的意思。注意，跟上面的 median（中线）不是一个意思,我们可以近似理解为字母 x 交叉点那个位置
> 对于内联元素垂直居中应该是对文字，而非居外部的块级容器所

#### 字母 x 与 CSS 中的 ex
> ex 是 CSS 中地地道道的一个尺寸单位
> 连 IE6 都老早支持的 ex 是 CSS 中的一个相对单位，指的是小写字母 x 的高度，没错，就是指 x-height。
>  [如图](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7u0iwr069j3048019743.jpg) 所示的文字后面跟着一个小三角形图标的效果是非常常见的。现在，要让该图标和文字中间位置对齐，你会如何实现？设定好尺寸，然后使用 vertical-align:middle？这样虽然也有效果，但是，实际上啰嗦了，借助 ex 单位，我们直接利用默认的 baseline 基线对齐就可以实现这个效果

    icon-arrow { 
        display: inline-block; 
        width: 20px; 
        height: 1ex; 
        background: url(arrow.png) no-repeat center; 
    }

### 内联元素的基石 line-height
#### 内联元素的高度之本—line-height
