## 流流的的破破坏坏与与保保护护
### 魔鬼属性 float
#### float 的本质与特性
> 认为 float 属性就是为各种块状布局而设计的，实际上不是的。在 Web 诞生之初，带宽就那么一点点，我们能够做到的也只是展示文字以及零星图片而已，怎么可能浮动设计的目的就是为了实现各种砖头式的复杂布局呢？
> 浮动的本质就是为了实现文字环绕效果。而这种文字环绕，主要指的就是文字环绕图片显示的效果。
> 浮动是魔鬼，少砌砖头、少浮动，要更多地去挖掘 CSS 世界本身的“流动性”和“自适应性”，以构建能够适用于各种环境的高质量的网页布局。
> float 都有哪些有意思的特性呢?
> + 包裹性；
> + 块状化并格式化上下文；
> + 破坏文档流；
> + 没有任何 margin 合并；
> 块状化的意思是，元素一旦 float 的属性值不为 none，则其 display 计算值就是 block或者 table。
> 因此，没有任何理由出现下面的样式组合：

    span { 
        display: block; /* 多余 */ 
        float: left; 
    } 
    span { 
        float: left; 
        vertical-align: middle; /* 多余 */ 
    }

> float 属性与 display 属性值转换关系[如表](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7xmt56tlcj30gi0erwf5.jpg)
> 除了 inline-table 计算为 table 外，其他全都是 block。至于 float 元素的块状格式化上下文特性，参见 6.3 节

#### float 的作用机制
> 甚至有些人会问这样的问题：“如何解决浮动让父元素高度塌陷的 bug？”bug？别逗了。一定要明确这一点，浮动使高度塌陷不是 bug，而是标准！有人可能会有疑问了：怎么会有规范让人“干坏事”的？
> float 属性让父元素高度塌陷的原因就是为了实现文字环绕效果。
>要想实现真正的“环绕效果”，就需要另外一个平时大家不太在意的特性，那就是“行框盒子和浮动元素的不可重叠性”，也就是“行框盒子如果和浮动元素的垂直高度有重叠，则行框盒子在正常定位状态下只会跟随浮动元素，而不会发生重叠”。
> 也就是“行框盒子”的区域永远就这么大，只要不改变当前布局方式，我们是无法通过其他 CSS 属性改变这个区域大小的。这就是在 4.3 节提到的浮动后面元素 margin 负无穷大依然无效的原因。就会发现，只有外部的块状容器盒子尺寸变大，而和浮动元素垂直方向有重叠的“行框盒子”依然被限死在那里.
> 我们不妨看下面这个很有学习价值的例子。很多人会有这样的想法，就是认为一个元素只要设置了具体的高度值，就不需要担心 float 属性造成的高度塌陷的问题了，既然有了高度，何来“高度塌陷”。这句话对不对呢？是对的。但是，其中也隐含了陷阱，因为“文字环绕效果”是由两个特性（即“父级高度塌陷”和“行框盒子区域限制”）共同作用的结果，定高只能解决“父级高度塌陷”带来的影响，但是对“行框盒子区域限制”却没有任何效果，结果导致的问题是浮动元素垂直区域一旦超出高度范围，或者下面元素 margin-top 负值上偏移，就很容易使后面的元素发生“环绕效果”
> 虽然肉眼看上去容器和图片一样高，但是，大家都读过5.3 节，应该都知道内联状态下的图片底部是有间隙的，也就是.float 这个浮动元素的实际高度并不是 64px，而是要比 64px 高几像素，带来的问题就是浮动元素的高度超出.father 几像素。
>[实例](http://demo.cssworld.cn/6/1-1.php)

#### float 更深入的作用机制
> + 浮动锚点是 float 元素所在的“流”中的一个点，这个点本身并不浮动，就表现而言更像一个没有 margin、border 和 padding 的空的内联元素
> + 浮动参考指的是浮动元素对齐参考的实体
> 在 CSS 世界中，float 元素的“浮动参考”是“行框盒子”，也就是 float 元素在当前“行框盒子”内定位
> 参考书6.1.3

#### float 与流体布局
> float 通过破坏正常 CSS 流实现 CSS 环绕，带来了烦人的“高度塌陷”的问题，然而，凡事都具有两面性，只要了解透彻，说不定就可以变废为宝、化腐朽为神奇。例如。我们可以利用 float 破坏 CSS 正常流的特性，实现两栏或多栏的自适应布局
> 还记不记得之前小动物环绕的例子？其实我们稍加改造，就能变成一侧定宽的两栏自适应布局，HTML 和 CSS 代码如下：

    <div class="father"> 
        <img src="me.jpg"> 
        <p class="animal">小猫 1，小猫 2，...</p> 
    </div> 
    .father { 
        overflow: hidden; 
    } 
    .father > img { 
        width: 60px; height: 64px; 
        float: left; 
    } 
    .animal { 
        margin-left: 70px; 
    }

> 和文字环绕效果相比，区别就在于.animal 多了一个 margin-left:70px，也就是所有小动物都要跟男主保持至少 70px 的距离，由于图片宽度就 60px，因此不会发生环绕，自适应效果达成。
> 原理其实很简单，.animal 元素没有浮动，也没有设置宽度，因此，流动性保持得很好，设置 margin-left、border-left 或者 padding-left 都可以自动改变 content box 的尺寸，继而实现了宽度自适应布局效果。
> [demo](http://demo.cssworld.cn/6/1-2.php)
> 上面的技巧适用于一侧定宽一侧自适应：如果是宽度不固定，也有办法处理，这会在 6.3.2 节中介绍。如果是百分比宽度，则也是可以的，例如：

    .left { 
        float: left; 
        width: 50%; 
    } 
    .right { 
        margin-left: 50%; 
    }

> 如果是多栏布局，也同样适用，尤其[图](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7yrz1vkhcj30aq01hdfq.jpg)所示的这种布局
> 假设 HTML 结构如下：

    <div class="box"> 
    <a href class="prev">&laquo; 上一章</a> 
    <a href class="next">下一章 &raquo;</a> 
    <h3 class="title">第 112 章 动物环绕</h3> 
    </div>

> 则 CSS 可以如下：

    .prev { 
        float: left; 
    } 
    .next { 
        float: right; 
    } 
    .title { 
        margin: 0 70px; 
        text-align: center; 
    }

### float 的天然克星 clear
#### 什么是 clear 属性
> clear 属性值:
> + none：默认值，左右浮动来就来。
> + left：左侧抗浮动。
> + right：右侧抗浮动。
> + both：两侧抗浮动。
> 我的答案非常直白：没错，确实没有什么用！凡是 clear:left 或者 clear:right 起作用的地方，一定可以使用 clear:both 替换！

> 举个例子，假设容器宽度足够宽，有 10 个\<li>元素，设置了如下 CSS 代码：[如图](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7ys7dkkqbj306801w0km.jpg)

    li { 
        width: 20px; height: 20px; 
        margin: 5px; 
        float: left; 
    } 
    li:nth-of-type(3) { 
        clear: both; 
    }

> 原因在于，clear 属性是让自身不能和前面的浮动元素相邻，注意这里“前面的”3 个字，也就是 clear 属性对“后面的”浮动元素是不闻不问的，因此才 2 行显示而非 3 行。

#### 成事不足败事有余的 clear
> clear 属性只有块级元素才有效的，而::after 等伪元素默认都是内联水平，这就是借助伪元素清除浮动影响时需要设置 display 属性值的原因。
> 由于 clear:both 的作用本质是让自己不和 float 元素在一行显示，并不是真正意义上的清除浮动，因此 float 元素一些不好的特性依然存在，于是，会有类似下面的现象
> + （1）如果 clear:both 元素前面的元素就是 float 元素，则 margin-top 负值即使设成-9999px，也不见任何效果。
> + （2）clear:both 后面的元素依旧可能会发生文字环绕的现象。举个例子，如下 HTML和 CSS：[如图](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7ysde4bl9j307102pt90.jpg)

    <div class="father"> 
        <img src="zxx.jpg"> 
        我是帅哥，好巧啊，我也是帅哥，原来看这本书的人都是帅哥~ 
    </div> 
    <div>虽然你很帅，但是我对你不感兴趣。</div>
    .father:after {
        content: ''; 
        display: table; 
        clear: both; 
    } 
    .father img { 
        float:left; 
        width: 60px; height: 64px; 
    } 
    .father + div { 
        margin-top: -2px; 
    }

>由此可见，clear:both 只能在一定程度上消除浮动的影响，要想完美地去除浮动元素的影响，还需要使用其他 CSS 声明。那应该使用哪些 CSS 声明呢？请看 6.3 节

### CSS 世界的结界——BFC
#### BFC 的定义
> BFC 全称为 block formatting context，中文为“块级格式化上下文”。相对应的还有 IFC，也就是 inline formatting context，中文为“内联格式化上下”
> 大家请记住下面这个表现原则：如果一个元素具有 BFC，内部子元素再怎么翻江倒海、翻云覆雨，都不会影响外部的元素。所以，BFC 元素是不可能发生 margin 重叠的，因为 margin重叠是会影响外面的元素的；BFC 元素也可以用来清除浮动的影响，因为如果不清除，子元素浮动则父元素高度塌陷，必然会影响后面元素布局和定位，这显然有违 BFC 元素的子元素不会影响外部元素的设定。
> 那什么时候会触发 BFC 呢？常见的情况如下：
> + \<html>根元素；
> + float 的值不为 none；
> + overflow 的值为 auto、scroll 或 hidden；
> + display 的值为 table-cell、table-caption 和 inline-block 中的任何一个；
> + position 的值不为 relative 和 static。

#### BFC 与流体布局
> BFC 的结界特性最重要的用途其实不是去 margin 重叠或者是清除 float 影响，而是实现更健壮、更智能的自适应布局。
> 我们还是从最基本的文字环绕效果说起。还是那个小动物环绕的例子：我们还是从最基本的文字环绕效果说起。还是那个小动物环绕的例子：

    <div class="father"> 
    <img src="me.jpg"> 
    <p class="animal">小猫 1，小猫 2，...</p> 
    </div> 
    img { float: left; }
    
