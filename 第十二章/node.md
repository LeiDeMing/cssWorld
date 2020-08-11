<!--
 * @Author: your name
 * @Date: 2019-11-05 12:44:07
 * @LastEditTime: 2020-08-11 15:43:42
 * @LastEditors: your name
 * @Description: In User Settings Edit
 * @FilePath: \cssWorld\第十二章\node.md
-->
## 流向的改变

### 改变水平流向的 direction

#### direction 简介
> 基本上，大家只要关心下面这两个属性值就好了：

    direction: ltr; // 默认值
    direction: rtl;

> 但是，Web 开发接触多语言的场景其实非常有限，但这其实并不妨碍 direction 属性在实际项目中的应用，因为 direction 改变水平流向的特性在网页布局的时候非常有用。
> 当然，direction 属性的作用远不止这些。通常，我们让单行文字溢出用点显示，这个点通常都是在右边的，省略的都是最后的文字，配合 direction 属性，我们可以让这个点打在开头，让前面的文字省略，CSS 和 HTML 代码如下：[demo](http://demo.cssworld.cn/12/1-2.php)

    .ell { 
        width: 240px; 
        white-space: nowrap; 
        text-overflow: ellipsis; 
        overflow: hidden; 
    } 
    <p class="ell" dir="ltr">开头是我，这是中间，然后就是结束</p> 
    <p class="ell" dir="rtl">开头是我，这是中间，然后就是结束</p>

> direction 属性还可以轻松改变表格中列的呈现顺序
> directionL:rtl 还可以让 text-justify 两端对齐元素，最后一行落单的元素右对齐显示。[demo](http://demo.cssworld.cn/12/1-3.php)
> 对了，还有一个小点值得一提。在不支持 text-align:start/end 的浏览器中（如 IE），不同的 direction 属性值会改变 text-align 属性的初始值：当 direction 值为 ltr 的时候，text-align 的初始值是 left；当 direction 值为 rtl 的时候，text-align 的初始值是 right。

#### direction 的黄金搭档 unicode-bidi
> 单看 unicode-bidi 的名称，你可能会觉得有点儿怪，会觉得它可能是哪个浏览器私有的属性，实际上不是的，unicode-bidi 是一个所有浏览器都支持的良好的 CSS 属性。至于它为什么会有这么奇怪的名称，我这里解释一下，你可以把 unicode 理解为“字符集”， 而 bidi 则是单词 bidirectionality 的简写，中文意思是“双向性”。网页中的字符很多时候是混合的，例如中文和英文夹杂，或者阿拉伯文和英文夹杂，此时就会出现文本阅读方向不一样的情况，阿拉伯文是从右往左读，英文是从左往右，而这种混合方向同时出现的现象就称为“双向性”，因此 unicode-bidi 作用就是明确字符出现“双向性”时应当有的表现。
> unicode-bidi 兼容性比较好的几个属性值如下：[demo](http://demo.cssworld.cn/12/1-4.php)

    unicode-bidi: normal; // 默认值
    unicode-bidi: embed; 
    unicode-bidi: bidi-override;

### 改变 CSS 世界纵横规则的 writing-mode
#### writing-mode 原本的作用
> 和 float 属性有些类似，writing-mode 原本是为控制内联元素的显示而设计的（即所谓的文本布局）。因为在亚洲，尤其像中国这样的东亚国家，存在文字的排版不是水平而是垂直的情况，如中国的古诗文,因此，writing-mode 就是用来实现文字竖向呈现的
> + 1．writing-mode 的语法
> + 补充说明如下。
> + + 相同的 writing-mode 属性值并不会累加。例如，如果父子元素均设置了 writing-mode:tb-rl，只会渲染一次，子元素并不会两次“旋转”。
> + + 在 IE 浏览器下，如果一个自身具有布局的元素（不是纯文本之类元素）writing-mode属性值和父元素不同，那么当子元素的布局流变化的时候，其父元素坐标系统的可用空间会被充分利用。这段文字太过术语化，我解释一下就是：在 IE 浏览器下，当布局元素从水平变成垂直的时候（举个例子），你就想象为元素在垂直方向是 100%自适应父元素高度的。因此，IE 浏览器下（不包括 Edge 13 及以上版本），元素 vertical流的时候你会发现高度高得吓人，布局和其他现代浏览器不一样，正是这个原因。
> + + 虽然 Chrome 和 Opera 认识 tb-rl 等老的 IE 属性值，但也仅仅是认识而已，并没有任何实际效果！

> + 2．需要关注的 writing-mode 属性值

#### writing-mode 不经意改变了哪些规则
> 1．水平方向也能 margin 合并(换句话说，如果元素是默认的水平流，则垂直 margin会合并；如果元素是垂直流，则水平 margin 会合并)[demo](http://demo.cssworld.cn/12/2-1.php)
> 2．普通块元素可以使用 margin:auto 实现垂直居中
> + [（1）图片元素](http://demo.cssworld.cn/12/2-2.php)
> + [（2）普通块状元素](http://demo.cssworld.cn/12/2-3.php)
> 3．可以使用 text-align:center 实现[图片垂直居中](http://demo.cssworld.cn/12/2-4.php)
> 4．可以使用 text-indent 实现[文字下沉效果](http://demo.cssworld.cn/12/2-5.php)
> 5．可以实现全兼容的 icon fonts [图标的旋转效果](http://demo.cssworld.cn/12/2-6.php)
> 6．充分利用高度的高度自适应布局

#### writing-mode 和 direction 的关系
> writing-mode、direction 和 unicode-bidi 是 CSS 世界中三大可以改变文本布局流向的属性，其中 direction 和 unicode-bidi 属于近亲，经常一起使用，也是仅有的两个不受 CSS3 的 all 属性影响的 CSS 属性，基本上就是和内联元素一起使用。它貌似是为阿拉伯文字设计的
> 乍一看，writing-mode 似乎包含了 direction 和 unicode-bidi 的某些功能和行为，例如，vertical-rl 的 rl 和 direction 的 rtl 值有相似之处，都是从右往左。然而，实际上两者是没有交集的。因为 vertical-rl 此时的文档流为垂直方向，rl 表示水平方向，此时再设置 direction:rtl，实际上值 rtl 改变的是垂直方向的内联元素的文本方向，一横一纵，没有交集。而且 writing-mode 可以对块状元素产生影响，直接改变了 CSS 世界的纵横规则，要比 direction 强大得多。它貌似是为东亚文字设计的。