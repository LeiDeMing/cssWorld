## 3.1
+ 标签分为两类 块级元素(block-level element) 和 内联元素(inline element)

### 块级元素
> 基本特征，也就是一个水平流上只能单独显示一个元素，多个块级元素则换行显示
+ 正是由于“块级元素”具有换行特性，因此理论上它都可以配合 clear 属性来清除浮动带来的影响(清除浮动原理？)
    
    .clear:after { 
        content: ''; 
        display: table; // 也可以是 block，或者是 list-item 
        clear: both; 
    }

+ list-item——出现项目符号,这个符号被称为“标记盒子”（marker box），专门用来放圆点、数字这些项目符号
+ 按照 display 的属性值不同，值为 block 的元素的盒子实际由外在的“块级盒子”和内在的“块级容器盒子”组成(满足display:inline-block属性)，外在盒子负责元素是可以一行显示，还是只能换行显示；内在盒子负责宽高、内容呈现什么的

### width/height 作用在内在盒子上