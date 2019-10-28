## 强大的文本处理能力
### line-height 的另外一个朋友 font-size
#### font-size 和 vertical-align 的隐秘故事
> line-height 的部分类别属性值是相对于 font-size 计算的，vertical-align 百分比值属性值又是相对于 line-height 计算的，于是，看上去八辈子都搭不上边的 verticalalign 和 font-size 属性背后其实也有有着关联的。
> 例如，下面的 CSS 代码组合：此时，p > img 选择器对应元素的 vertical-align 计算值应该是:16px * 1.5 * -25% = -6px

    p { 
        font-size: 16px; 
        line-height: 1.5; 
    } 
    p > img { 
        ertical-align: -25%; 
    }

> 但是两者又有所不同，很显然，-25%是一个相对计算属性值，如果此时元素的 font-size发生变化，则图片会自动进行垂直位置调整。我们可以看一个无论 font-size 如何变化、后面图标都垂直居中对齐的例子，(手动输入)(http://demo.cssworld.cn/8/1-1.php) 或者扫下面的二维码。可以看到，无论文字字号是大还是小，后面的图标都非常良好地垂直居中对齐，[如图](https://ws1.sinaimg.cn/large/0060ZzrAgy1g8eazi2gosj309y048wel.jpg)