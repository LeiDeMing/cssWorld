<!--
 * @Author: your name
 * @Date: 2020-07-23 10:36:32
 * @LastEditTime: 2020-07-23 10:52:35
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \cssWorld\第二章\note.md
--> 

## 2.1 务必了解的 CSS 世界的专业术语
+ 1．属性
> 属性对应的是平常我们书面或交谈时对 CSS 的中文称谓。例如，上面示意 CSS 代码中的height 和 color 就是属性。当我们聊天或者分享时说起 CSS 的时候，嘴里冒出来的都是“这个元素高度 99 像素”，或者“这个文字颜色透明”，对吧？这里提到的“高度”和“颜色”就是CSS 世界的属性，感觉有点儿像现实世界里人的姓氏。

+ 2．值
> “值”大多与数字挂钩。例如，上面的 99px 就是典型的值。在 CSS 世界中，值的分类非常广泛，下面是一些常用的类型。
> + 整数值，如 z-index:1 中的 1，属于<integer>，同时也属于<number>。 
> + 数值，如 line-height:1.5 中的 1.5，属于<number>。 
> + 百分比值，如 padding:50%中的 50%，属于<percent>。 
> + 长度值，如 99px。 • 颜色值，如#999。

+ 3．关键字
> 顾名思义，关键字指的是 CSS 里面很关键的单词，这里的单词特指英文单词，abc 是单词吗？不是，因此，如果 CSS 中出现它，一定不是关键字。上面示例 CSS 代码中的 transparent就是典型的关键字，还有常见的 solid、inherit 等都是关键字，其中 inherit 也称作“泛关键字”，所谓泛关键字，可以理解为“公交车关键字”，就是“所有 CSS 属性都可以使用的关键字”的意思。

+ 4．变量
> CSS 中目前可以称为变量的比较有限，CSS3 中的 currentColor 就是变量，非常有用。不过，这属于《CSS 新世界》的内容，本书不会详细阐述，有兴趣的读者可以访问 http://www.zhangxinxu.com/wordpress/?p=4385 或者扫右侧的二维码，做简单的了解。

+ 5．长度单位
> CSS 中的单位有时间单位（如 s、ms），还有角度单位（如 deg、rad 等），但最常见的自然还是长度单位（如 px、em 等）。需要注意的是，诸如 2%后面的百分号%不是长度单位。再说一遍，%不是长度单位！因为 2%就是一个完整的值，就是一个整体，我想你一定认为 0.02 是值，没错，2%也同样是值。
> 如果继续细分，长度单位又可以分为相对长度单位和绝对长度单位。
> + （1）相对长度单位。相对长度单位又分为相对字体长度单位和相对视区长度单位。
> + + 相对字体长度单位，如 em 和 ex，还有 CSS3 新世界的 rem 和 ch（字符 0 的宽度）。
> + + 相对视区长度单位，如 vh、vw、vmin 和 vmax。
> + （2）绝对长度单位：最常见的就是 px，还有 pt、cm、mm、pc 等了解一下就可以，在我看来，它们实用性近乎零，至少我这么多年一次都没用过。

+ 6．功能符
> 值以函数的形式指定（就是被括号括起来的那种），主要用来表示颜色（rgba 和 hsla）、背景图片地址（url）、元素属性值、计算（calc）和过渡效果等，如 rgba(0,0,0,.5)、url('css-world.png')、attr('href')和 scale(-1)。

> 7．属性值
> 属性冒号后面的所有内容统一称为属性值。例如，1px solid rgb(0,0,0)就可以称为属性值，它是由“值+关键字+功能符”构成的。属性值也可以由单一内容构成。例如，z-index:1的 1 也是属性值。

+ 8．声明
> 属性名加上属性值就是声明，例如：color: transparent;

+ 9．声明块
>声明块是花括号（{}）包裹的一系列声明，例如：
    
    { 
        height: 99px; 
        color: transparent; 
    }

+ 10．规则或规则集
> 出现了选择器，而且后面还跟着声明块，比如本小节一开始的那个例子，就是一个规则集：
    
    .vocabulary { 
        height: 99px; 
        color: transparent; 
    }

+ 11．选择器
> 选择器是用来瞄准目标元素的东西，例如，上面的.vocabulary 就是一个选择器。
> + 类选择器：指以“.”这个点号开头的选择器。很多元素可以应用同一个类选择器。“类”，天生就是被公用的命。
> + ID 选择器：“#”打头，权重相当高。ID 一般指向唯一元素。但是，在 CSS 中，ID样式出现在多个不同的元素上并不会只渲染第一个，而是雨露均沾。但显然不推荐这么做。
> + 属性选择器：指含有[]的选择器，形如[title]{}、[title= "css-world"]{}、
[title~="css-world"]{}、[title^= "css-world"]{}和[title$="cssworld"]{}等。
> + 伪类选择器：一般指前面有个英文冒号（:）的选择器，如:first-child 或:lastchild 等。
> + 伪元素选择器：就是有连续两个冒号的选择器，如::first-line::firstletter、::before 和::after。

+ 12．关系选择器
> 关系选择器是指根据与其他元素的关系选择元素的选择器，常见的符号有空格、>、~，还有+等，这些都是非常常用的选择器。
> + 后代选择器：选择所有合乎规则的后代元素。空格连接。
> + 相邻后代选择器：仅仅选择合乎规则的儿子元素，孙子、重孙元素忽略，因此又称“子
选择器”。>连接。适用于 IE7 以上版本。
> + 兄弟选择器：选择当前元素后面的所有合乎规则的兄弟元素。~连接。适用于 IE7 以上
版本。
> + 相邻兄弟选择器：仅仅选择当前元素相邻的那个合乎规则的兄弟元素。+连接。适用于
IE7 以上版本。

+ 13．@规则
> @规则指的是以@字符开始的一些规则，像@media、@font-face、@page 或者@support，诸如此类。

## 2.2 了解 CSS 世界中的“未定义行为”
+ 当某个浏览器中出现与其他浏览器不一样的行为或样式表现的时候，我们总会习惯把这种不一样的表现认为是浏览器的 bug。但在 CSS 世界，这种认识是狭隘的。
+ 在现实世界中，有法律来约束我们的行为，如果越界，就称为违法；同样地，在 CSS 世界里，有 Web 标准来约束元素的行为，如果越界，就称为 bug。但是，法律总是人制定的，世间万象是不可能面面俱到的，会存在法律空白；同样地，Web 应用场景千变万化，Web 标准也是
不可能面面俱到的，也会存在规范描述以外的场景，此时，各大浏览器厂家只能根据自己的理解与喜好去实现，一旦个性化就会出现差异，就会遇到“火狐火狐，你怎么啦？平时表现挺好的，今天怎么被 IE 带坏了？”的情景。实际上，此时遇到的表现差异并不是浏览器的 bug，用
计算机领域的专业术语描述应该是“未定义行为”（undefined behavior）