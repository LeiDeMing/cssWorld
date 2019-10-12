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
> 如果仅仅通过表象来确认，估计不少人会认为\<div>高度是由里面的文字撑开的，也就是font-size 决定的，但本质上是由 line-height 属性全权决定的，尽管某些场景确实与font-size 大小有关
> 对于非替换元素的纯内联元素，其可视高度完全由 line-height 决定
> 内联元素的高度由固定高度和不固定高度组成，这个不固定的部分就是这里的“行距”。换句话说，line-height 之所以起作用，就是通过改变“行距”来实现的
> 在 CSS 中，“行距”分散在当前文字的上方和下方，也就是即使是第一行文字，其上方也是有“行距”的，只不过这个“行距”的高度仅仅是完整“行距”高度的一半，因此，也被称为“半行距”
> 半行距,一般业界的共识是：行距 = 行高− em-box。转换成 CSS 语言就是：行距 = line-height - font-size。
> 中 em-box 是 CSS 世界中比较虚的一个概念，说“虚”并不是胡编乱造的意思，而是我们无法有效感知这个盒子具体的位置在哪里，但是有一点可以明确，就是其高度正好就是 1em。em是一个相对 font-size 大小的 CSS 单位，因此 1em 等用于当前一个 font-size 大小
> 内容区域（content area）出马了。在本书中，内容区域可以近似理解为 Firefox/IE浏览器下文本选中带背景色的区域。
> 内容区域和 em-box 是不一样的，内容区域高度受 font-family 和font-size 双重影响，而 em-box 仅受 font-size 影响，通常内容区域高度要更高一些。除了下面这种情况，也就是“当我们的字体是宋体的时候，内容区域和 em-box 是等同的”[半行距](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7u0zwxku1j30df064gmc.jpg)
> 网页中的设置的 line-height 大小，就能根据标注获取准确的间距值。举个例子，假设 line-height 是 1.5，font-size 大小是 14px，那么我们的半行距大小就是（套用上面的行距公式再除以 2）：(14px * 1.5 - 14px) / 2 = 14px * 0.25 = 3.5px
> border 以及 line-height 等传统 CSS 属性并没有小数像素的概念（从 CSS3 动画的细腻程度可以看出），因此，这里的 3.5px 需要取整处理，如果标注的是文字上边距，则向下取整；如果是文字下边距，则向上取整，因为绝大多数的字体在内容区域中都是偏下的
> 当line-height设为 2 的时候，半行距是一半的文字大小，两行文字中间的间隙差不多一个文字尺寸大小；如果 line-height 大小是 1 倍文字大小，则根据计算，半行距是 0，也就是两行文字会紧密依偎在一起；如果 line-height 值是 0.5，则此时的行距就是负值，虽然 line-height 不支持负值，但是行距可以为负值，此时，两行文字就是重叠纠缠在一起
+ A 替换元素
> + line-height不能影响替换元素（如图片的高度）
> + line-height 在这个混合元素的“行框盒子”中扮演的角色是决定这个行盒的最小高度，听上去似乎有点儿尴尬，对于纯文本元素，line-height 非常威风，直接决定了最终的高度。但是，如果同时有替换元素，则line-height 的表现一下子弱了很多，只能决定最小高度，对最终的高度表现有望尘莫及之感。为什么会这样呢？一是替换元素的高度不受 line-height 影响，二是 vertical-align属性在背后作祟。
+ B 块级元素
> + line-height 对其本身是没有任何作用的，我们平时改变 line-height，块级元素的高度跟着变化实际上是通过改变块级元素里面内联级别元素占据的高度实现的
> + 比方说，一个\<p>元素中有 10 行图文内容，则这个\<p>元素的高度就是由这 10 行内容产生的 10 个“行框盒子”高度累加而成；一个\<article>元素中可能会有 20 个\<p>元素，则这个\<article>元素又是由这 20 个块级\<p>元素高度累加而成，同时块级元素还可以通过height 和 min-height 以及盒模型中的 margin、padding 和 border 等属性改变占据的高度，所有这一切就构成了 CSS 世界完整的高度体系。

#### 为什么 line-height 可以让内联元素“垂直居中”
> 多行文本或者替换元素的垂直居中实现原理和单行文本就不一样了，需要 line-height属性的好朋友 vertical-align 属性帮助才可以，示例代码如下：

    .box { 
        line-height: 120px; 
        background-color: #f0f3f9; 
    } 
    .content { 
        display: inline-block; 
        line-height: 20px; 
        margin: 0 20px; 
        vertical-align: middle; 
    } 
    <div class="box"> 
        <div class="content">基于行高实现的...</div> 
    </div>

> 实现的原理大致如下。
> + （1）多行文字使用一个标签包裹，然后设置 display 为 inline-block。好处在于既能重置外部的 line-height 为正常的大小，又能保持内联元素特性，从而可以设置vertical-align 属性，以及产生一个非常关键的“行框盒子”。我们需要的其实并不是这个“行框盒子”，而是每个“行框盒子”都会附带的一个产物—“幽灵空白节点”，即一个宽度为0、表现如同普通字符的看不见的“节点”。有了这个“幽灵空白节点”，我们的 lineheight:120px 就有了作用的对象，从而相当于在.content 元素前面撑起了一个高度为120px 的宽度为 0 的内联元素。
> + （2）因为内联元素默认都是基线对齐的，所以我们通过对.content 元素设置 verticalalign:middle 来调整多行文本的垂直位置，从而实现我们想要的“垂直居中”效果。如果是要借助 line-height 实现图片垂直居中效果，也是类似的原理和做法。
> + 这里实现的“垂直居中”确实也不是真正意义上的垂直居中，也是“近似垂直居中”。然后通过尺子工具一量就会发现，上面的留空是
41px，下面的留空是 39px，[如图](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7u1vuydv0j307903fjsm.jpg)

#### 深入 line-height 的各类属性值
> line-height 的默认值是 normal，还支持数值、百分比值以及长度值
> normal 实际上是一个和font-family 有着密切关联的变量值
> 比方说，一个\<div>元素，有两段对比 CSS 如下：

    div { 
        line-height: normal; 
        font-family: 'microsoft yahei'; 
    } 
    div { 
        line-height: normal; 
        font-family: simsun; 
    }

> 此时两段 CSS 中 line-height 的属性值 normal 的计算值是不一样的，[表](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7u2lzd4ytj30m703y74j.jpg)给出的是我在几个桌面浏览器的测试数据。
> + 数值，如 line-height:1.5，其最终的计算值是和当前 font-size 相乘后的值。例如，假设我们此时的 font-size 大小为 14px，则 line-height 计算值是1.5*14px=21px
> + 百分比值，如 line-height:150%，其最终的计算值是和当前 font-size 相乘后的值。例如，假设我们此时的 font-size 大小为 14px，则 line-height 计算值是150%*14px=21px。
> + 长度值，也就是带单位的值，如 line-height:21px 或者 line-height:1.5em等，此处 em 是一个相对于 font-size 的相对单位，因此，line-height:1.5em最终的计算值也是和当前font-size相乘后的值。例如，假设我们此时的font-size大小为 14px，则 line-height 计算值是 1.5*14px=21px。
> 实际上，line-height:1.5和另外两个有一点儿不同，那就是继承细节有所差别。如果使用数值作为 line-height 的属性值，那么所有的子元素继承的都是这个值；但是，如果使用百分比值或者长度值作为属性值，那么所有的子元素继承的是最终的计算值。
> line-height:150%、line-height:1.5em 要想有类似 line-height:1.5的继承效果，也是可以实现的，类似下面的 CSS 代码：
    
    * {
        line-height: 150%;
    }

> HTML 中的很多替换元素，尤其表单类的替换元素，如输入框、按钮之类的，很多具有继承特性的 CSS 属性其自己也有一套，如 font-family、font-size 以及这里的 line-height。由于继承是属于最弱的权重，因此 body 中设置的 line-height 是无法影
响到这些替换元素的，但是*作为一个选择器，就不一样了，会直接重置这些替换元素默认的line-height，这其实是我们需要的

#### 内联元素 line-height 的“大值特性”
> CSS 代码有所不同，分别为

    .box { 
        line-height: 96px; 
    } 
    .box span { 
        line-height: 20px; 
    } 
    //和

    .box { 
        line-height: 20px; 
    } 
    .box span { 
        line-height: 96px; 
    }

> 正确的答案是：全都是 96px 高！也就是说：无论内联元素 line-height 如何设置，最终父级元素的高度都是由数值大的那个 line-height 决定的，我称之为“内联元素 line-height 的大值特性”。
> 但是，在内联盒模型中，存在一些你看不到的东西，没错，就是多次提到的“幽灵空白节点”
> 这里的\<span>是一个内联
元素，因此自身是一个“内联盒子”，本例就这一个“内联盒子”，只要有“内联盒子”在，就一定会有“行框盒子”，就是每一行内联元素外面包裹的一层看不见的盒子。然后，重点来了，在每个“行框盒子”前面有一个宽度为 0 的具有该元素的字体和行高属性的看不见的“幽灵空白节点”，如果套用本案，则这个“幽灵空白节点”就在\<span>元素的前方

### line-height 的好朋友 vertical-align
> 比方说，对于下面非常简单的 CSS 和 HTML 代码：

    .box { line-height: 32px; } 
    .box > span { font-size: 24px; } 
    <div class="box"> 
        <span>文字</span> 
    </div>

> 但是事实上，高度并不是 32px，而是要大那么几像素（受不同字体影响，增加高度也不一样），比方说 36px，[如图](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7v4uydyksj303o01w3yc.jpg)

#### vertical-align 家族基本认识
> vertical-align 属性值分为以下 4 类：
> + 线类，如 baseline（默认值）、top、middle、bottom；
> + 文本类，如 text-top、text-bottom；
> + 上标下标类，如 sub、super；
> + 数值百分比类，如 20px、2em、20%等。(实际上，“数值百分比类”应该是两类，分别是“数值类”和“百分比类”，这里之所以把它们合在一起归为一类，是因为它们有不少共性，包括“都带数字”和“行为表现一致”。)(根据计算值的不同，相对于基线往上或往下偏移，到底是往上还是往下取决于 vertical- align 的计算值是正值还是负值，如果是负值，往下偏移，如果是正值，往上偏移)
> 关键是 vertical-align 的数值属性值在实际开发的时候实用性非常强
> + 一是其兼容性非常好
> + 二是其可以精确控制内联元素的垂直对齐位置
> + [demo](http://demo.cssworld.cn/5/3-3.php)
> 在 CSS 世界中，凡是百分比值，均是需要一个相对计算的值，例如，margin 和 padding是相对于宽度计算的，line-height 是相对于 font-size 计算的，而这里的 verticalalign 属性的百分比值则是相对于 line-height 的计算值计算的

#### vertical-align 作用的前提
> 只能应用于内联元素以及 display 值为 table-cell 的元素,换句话说，vertical-align 属性只能作用在 display 计算值为 inline、inlineblock，inline-table 或 table-cell 的元素上
> 因此，默认情况下，\<span>、\<strong>、\<em>等内联元素，\<img>、\<button>、\<input>等替换元素，非 HTML 规范的自定义标签元素，以及\<td>单元格，都是支持 vertical-align 属性的，其他块级元素则不支持
> CSS 世界中，有一些 CSS 属性值会在背后默默地改变元素 display 属性的计算值，从而导致 vertical-align 不起作用
> 下面这种情况:

    .box { 
    height: 128px; 
    } 
    .box > img { 
    height: 96px; 
    vertical-align: middle; 
    } 
    <div class="box"> 
    <img src="1.jpg"> 
    </div>

> + 这种情况看上去是 vertical-align:middle 没起作用，实际上，vertical-align是在努力地渲染的，只是行框盒子前面的“幽灵空白节点”高度太小，如果我们通过设置一个足够大的行高让“幽灵空白节点”高度足够，就会看到 vertical-align:middle 起作用了，
> + CSS 代码如下：

    .box { 
        height: 128px; 
        line-height: 128px; /* 关键 CSS 属性 */ 
    } 
    .box > img { 
        height: 96px; 
        vertical-align: middle; 
    }

> 是因为对 table-cell 元素而言，vertical-align 起作用的是 table-cell元素自身

    .cell { 
        height: 128px; 
        display: table-cell; 
    } 
    .cell > img { 
        height: 96px; 
        vertical-align: middle; 
    } 
    <div class="cell"> 
        <img src="1.jpg"> 
    </div>

> + 结果[图片](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7v5hzqh8mj30az03r74i.jpg)并没有要垂直居中的迹象，还是紧贴着父元素的上边缘
> + 如果 vertical-align:middle 是设置在 table-cell 元素上，CSS 代码如下：

    .cell { 
        height: 128px; 
        display: table-cell;
        vertical-align: middle; 
    } 
    .cell > img { 
        height: 96px; 
    }

> + 那么图片就有了明显的变化，[如图](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7v5hq2dgpj309504edg4.jpg)
> + 所以，大家一定要明确，虽然就效果而言，table-cell 元素设置 vertical-align 垂直对齐的是子元素，但是其作用的并不是子元素，而是 table-cell 元素自身。就算 table-cell 元素的子元素是一个块级元素，也一样可以让其有各种垂直对齐表现

#### vertical-align 和 line-height 之间的关系