<!--
 * @Author: your name
 * @Date: 2019-11-05 12:25:13
 * @LastEditTime: 2020-08-11 15:18:17
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \cssWorld\第十一章\note.md
-->
## 用户界面样式
> 用户界面样式指的是 CSS 世界中用来帮助用户进行界面交互的一些 CSS 样式，主要有outline 和 cursor 等属性。

### 和 border 形似的 outline 属性
> outline 表示元素的轮廓，语法和 border 属性非常类似，分宽度、类型和颜色，支持的关键字和属性值与 border 属性一模一样,两者表现也类似，都是给元素的外面画框。但是，设计的作用却大相径庭。

#### 万万不可在全局设置 outline:0 none
> 好在所有的浏览器原生就有键盘访问网页的能力，对于按钮或者链接，通常的键盘操作是：Tab 键按次序不断 focus 控件元素，包括链接、按钮、输入框等表单元素，或者 focus 设置了 tabindex 的普通元素，按 Shift+Tab 组合键反方向访问

#### 真正的不占据空间的 outline 及其应用
> outline 是本书介绍到现在出现的第一个真正意义上的不占据任何空间的属性
> 用于实现一些看似棘手的布局效果
> + 1．案例一：头像剪裁的矩形镂空效果[demo](http://demo.cssworld.cn/ 11/1-1.php)
> + 2．案例二：自动填满屏幕剩余空间的应用技巧

### 光标属性 cursor
#### 琳琅满目的 cursor 属性值
> cursor 属性值几乎可以认为是当下支持的关键字属性值最多的 CSS 属性，没有之一。下面就是按照功能特性对其进行的分类以及具体解释描述。
> 见书 308~314

#### 自定义光标
> 从 IE6 开始，我们就可以自定义网页中的光标样式，因此，对于 cursor 属性，兼容性都不是问题。例如，IE8 不支持上面提到的 cursor:none，就是通过自定义手段实现兼容的：

    .cur-none { 
        cursor: url(transparent.cur); 
    }

> 对于 Chrome 等浏览器，可以直接使用 PNG 图片作为光标，但是 IE 浏览器不行。IE 仅支持专门的.cur 格式的光标文件，需要使用工具进行格式转换，至于什么工具，大家可以自行搜索一下，还是有很多的