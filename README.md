# 任务3-简单的移动端网页

## 一、任务效果

![target](https://github.com/lespric/MagicMirror/blob/master/gif/target.gif)

## 二、任务难点

* UI设计图的转化，以及合理布局规划；
* em、rem的应用，达到随浏览器设置的字体大小变化而变化；
* 不同移动端设备的适配，以及横屏适配竖屏效果，达到无滚动条效果；

## 三、成果展示

* 竖屏效果  
![portrait](https://github.com/lespric/MagicMirror/blob/master/gif/portrair.gif)
* 横屏效果  
![landscape](https://github.com/lespric/MagicMirror/blob/master/gif/landscape.gif)

## 四、开发思路及深度思考

### 1. 分析布局

参考：
[超全面的UI设计切图规范](http://www.sohu.com/a/227940322_100082415)

* **PS规划布局**:
  ![task3](https://github.com/lespric/MagicMirror/blob/master/Img/task3.png)

  * 根据上图，可以把布局分为header、main、footer，即导航栏、主体内容、标签栏，各栏都是占满一行，属于块状元素，用div的具体结构标签划分；
  * 屏幕左右两端增加安全距离间距，用padding表示来避免控件紧挨边缘，各结构标签及元素上下间距用margin表示来相对于上行元素边线，左右间距用padding表示相对于屏幕安全间距边线，从而确定块状元素控件的水平位置；
  * 其中，header只有一个返回按键控件，main有logo和正文两个控件，且各自也是占满一行，都属于块状元素，直接可以用margin确定垂直位置；
  * footer有两个功能入口组件，且并列一行，则应转化为内联块状元素进行布局，而功能入口组件为图标和文字组合，且二者垂直居中对齐，可先与屏幕垂直居中对齐，再由功能入口组件的flex布局为两端对齐；

* **PS切图**
  * 设计图为@2x即二倍图，开发时以一倍图进行开发，即开发单位为dp或pt，具体单位使用rem/em + %的组合方式，且一般以ios规范进行开发，再适配于Android；
  * 标注尺寸和切图尺寸如以px为单位，对于ios各尺寸为4的倍数，对于Android各尺寸为8的倍数，而对于按钮等可互动控件，在2倍图下最小的切图尺寸为44px，而图片的切图尺寸为链接图片本身的大小，或者为控制大小，采用局部切图；
  * 图片大小，如icon、img、展示文字等无法用样式绘制的元素由设计图导出，分别导出二倍、三倍图，可以用媒体查询密度值来应用相应倍率的图片，在一倍的设计图中，应用二倍图来保证清晰度；
  * 最后，根据标注尺寸进行单位换算，换算为自适应布局的单位（rem/em + %），应用在CSS设置中；

* **总结**：用PS切图可以用Cutterman插件辅助切图，也可以导入蓝湖网站进行切图标注，

### 2. 单位选择

参考：  
[各单位区别](https://www.cnblogs.com/jesse131/p/9079219.html)  
[rem和em的区别](http://caibaojian.com/rem-vs-em.html)  
[关于UI适配和开发单位之间的那点事](https://www.jianshu.com/p/9a5f8624da2e)  

* **px**
  * 描述：在确定的设备中，用px比较稳定和精确，但无法根据字体大小而变化，用户失去部分自由设置的体验，而且在不同dpi的设备中，px显示的实际长度也会不同，所以开发过程中都是用开发单位进行适配，Android为dp，ios为pt，在一倍图时，1dp=1px，1pt=1px；
  * 使用：在明确的设备中，以及不允许缩放的元素，使用px可以限制，也可以配合rem、em来设置文字大小的固定值；
  ![android](https://github.com/lespric/MagicMirror/blob/master/Img/android.jpg)  
  ![ios](https://github.com/lespric/MagicMirror/blob/master/Img/ios.jpg)

* **rem**
  * 描述：取决于html元素的文字大小，在默认的浏览器情况下，1rem=16px，62.5%rem=10px，一般由浏览器设置的文字大小决定，可以根据用户设置偏好来改变，也不影响布局的整体结构，除非显式设置固定值去覆盖；
  * 使用：一切扩展都可以使用rem做单位，布局可以整体变化，最好为html的子元素，应用于媒体查询可以快速调整各元素大小；
* **em**
  * 描述：取决于当前元素的文字大小，一般当前元素会继承父级元素的文字大小，除非设置font-size为固定值，尤其在同一个元素内设置font-size:2em后，margin:1em，这两个em值都不一样，前者em为父级元素的em值，后者em为当前文字大小2em的值，由此可见，font-size优先判定；
  * 使用：如需要组件根据文字大小的变化来整体缩放，组件各控件都可以用em单位，最好为body的子元素，在body进行62.5%rem的换算下，1em=10px来进行标注；

* **vw、vh**
  * 描述：vw为可视宽度的百分比，vh为可视高度的百分比，所以屏幕显示区域为100vw和100vh；
  * 使用：在宽度上，vw可以用%来代替表示，而在高度上，可以用vh来限制元素高度，尤其设置max-height值小于100vh时，可以调整高度时不出现竖直滚动条，来保证内容一页显示；

* **%**
  * 描述：相对于包含块的高宽或字体大小，包含块对于层模型中不同定位的元素都不一样，同样不同属性相对于包含块的属性也不一样，而继承效果为前个属性计算后的数值，如`font-size: 10px;line-height: 120%`
  * 使用：用于自适应布局中，尤其是弹性盒模型布局，在不知道元素的具体尺寸，来适配不同分辨率的屏幕时，既使元素变化可以等比缩放，又保证宽度不会出现滚动条；

* **总结**：由此，在一般的PC端网页制作过程中，一般直接用px为单位，在移动端由于vw,vh单位的兼容性，目前一般采用rem/em + %或者rem/em + vw/vh，因为本任务以移动端为主，所以采用rem/em + %的方式；

### 3. 自适应布局

参考：  
[H5项目常见问题及注意事项](https://blog.csdn.net/github_36487770/article/details/61200812)  
[自适应网页设计](http://www.ruanyifeng.com/blog/2012/05/responsive_web_design.html)  
[css 识别屏幕大小自适应](https://blog.csdn.net/u013378306/article/details/78840633)

* **布局思路**
  * 各栏高度由元素高度决定，且元素高度明确，上下间距使用margin表示，用em作为单位来固定距离，而对于文字宽度和功能入口的左右间距不明确，所以文字和功能入口的左右间距由屏幕的内边距决定，用padding表示，用%作为单位来弹性缩放；
  * 为使布局整体缩放，采用body用rem单位，body子元素用em单位，从而body声明font-size为0.625rem，来方便body子元素的单位换算，即1em=10px；
  * 为使布局在不同分辨率的设备中进行适配，尤其是手机、平板、电脑之间，采用媒体查询分辨率，设置分辨率的范围为触发条件，改变html的文字大小，从而缩放整体布局，使布局更加清楚合理；
    * 大屏幕:大于等于1200px ,`@media (min-width: 1200px) { ... }`
    * 默认: `@media (min-width: 980px){...`}
    * 平板电脑和小屏电脑之间的分辨率：`@media (min-width: 768px) and (max-width: 979px) { ... }`
    * 横向放置的手机和竖向放置的平板之间的分辨率 `@media (max-width: 767px) { ... }`
    * 横向放置的手机及分辨率更小的设备 `@media (max-width: 480px) { ... }`
  * 对于手机，一般设置为竖屏，而横屏时，因本任务页面为首页且内容不多，为使横屏时无滚动条，需要媒体查询检测为横屏时，应用相应的CSS样式——使上下间距缩小一倍；
    * 竖屏时样式：`@media all and (orientation:portrait){   }`
    * 横屏时样式：`@media all and (orientation:landscape){   }`

* **网页自适应设计**
  * 允许网页自动调整：即网页以100%可视区域展示，限制缩放功能，在在网页代码的头部，加入一行viewport元标签，`<meta name="viewport" content="width=device-width, initial-scale=1" />`
  * 不使用绝对宽度：即不能用px单位限制，应用rem、em、vw、vh、%这类单位表示，或者margin: 0 auto来水平居中；
  * 相对大小的文字，html的子元素用rem，body的子元素用em表示；
  * 自适应布局：元素的浮动模型和弹性盒模型，文本的多列布局，配合%的单位进行适配；
  * 媒体查询@media：设置不同分辨率的范围作为触发条件，应用相应的CSS样式；
  * media标签：如媒体查询，在`<meta name=”viewport” content=”” />`的content属性值，设置不同分辨率的范围，来外联相应的CSS样式；
  * 图片自适应：设置图片的最大宽度为100%，用 % 单位来保证图片完全覆盖且大小与父元素盒大小一样，而windows平台缩放图片时，可能出现图像失真现象，可用IE专有命令：`img { -ms-interpolation-mode: bicubic; }`

### 4. css选择器

参考：  
[CSS选择器优先级总结](https://www.cnblogs.com/zxjwlh/p/6213239.html)  
[十天精通CSS3](https://www.imooc.com/code/693)

* 常用选择器
  * **标签选择器**：对应html的代码标签，如`body{  };`
  * **类选择器**：对应标签中class的属性值，以声明多个class，可以多次使用，如`.nav{  };`
  * **ID选择器**：对应标签中id的属性值，只能声明一个id，即id选择器只能使用一次，如`#nav{  };`，
  * **子选择器**：即大于符号(>),用于选择指定标签元素的第一代子元素，如`.food>li{border:1px solid red;};`
  * **包含选择器**：即加入空格,用于选择指定标签元素下的后辈元素，如`.first  span{color:red;}`
  * **通用选择器**：功能最强大的选择器，使用一个（*）号指定，是匹配html中所有标签元素，如`* {color:red;};`
  * **分组选择符**：给html中多个标签元素设置同一个样式时，用 (,) 号隔开，如`h1,span{color:red;};`

* 特殊选择器
  * 目标选择
    * **属性选择器**：具有通配符遍历所有标签的功能，如下，E为标签名，att为属性名称
      * 对应属性为att，其属性值以val**开头**的任何字符串的E标签，如`E[att^="val"]`
      * 对应属性为att，其属性值以val**结尾**的任何字符串的E标签，如`E[att$="val"]`
      * 对应属性为att，其属性值在任意位置**包含**val的任何字符串的E标签，如`E[att*="val"]`
    * **:root选择器** :根选择器，匹配元素E所在文档的根元素。在HTML文档中，根元素始终是html，如`:root {  background:orange; }`
    * **:not选择器**：否定选择器，可以选择除某个元素之外的所有元素，input为标签名称，not（）里面为其中一条完整属性，如`input:not([type="submit"]){ border:1px solid red; }`
    * **:empty选择器**：空选择器，用来选择没有任何内容的元素，选择的是innerHTML没有任何内容的紧密靠近的标签，连空格都没有，即`<p></p>`，如`p:empty { display: none; }​`
    * **:target选择器**：目标选择器，用来匹配文档(页面)的url的某个标志符的目标元素，即href=#名称，target匹配的是id为同样名称的标签，如只有一个href和id，就直接：target{ }即可如`：#brand:target { background: orange; color: #fff;}`

  * 子元素选择
    * **:first-child选择器**：表示的是选择父元素的第一个子元素的元素E，不是后代元素，而是属性标签，如：通过“:first-child”选择器定位列表中的第一个列表项，并将序列号颜色变为红色：`ul > li:first-child{ color: red; }`
    * **:last-child选择器**：选择的是元素的最后一个子元素，如：`ul>li:last-child{background:blue;}`
    * **:nth-child(n)选择器**：用来定位某个父元素的一个或多个特定的子元素，如`ol > li:nth-child(2n){ background: orange; }`
      * 其中“n”是其参数，而且可以是整数值(1,2,3,4)，也可以是表达式(2n+1、-n+5)和关键词(odd、even)，但参数n的起始值始终是1，而不是0；
      * 当:nth-child(n)选择器中的n为一个表达式时，其中n是从0开始计算，当表达式的值为0或小于0的时候，不选择任何匹配的元素；
    * **:nth-last-child(n)选择器**：从某父元素的最后一个子元素开始计算，来选择特定的元素，如`ol > li:nth-last-child(5){ background: orange; }`
    * **:only-child选择器**：选择的是父元素中只有一个子元素，而且只有唯一的一个子元素，如没有父元素声明，自动匹配全局中仅有一个子元素，而且是一个唯一的子元素的父元素，的子元素，如`.post p:only-child { background: orange; }`

  * 子类型选择
    * **:first-of-type选择器**：类似于“:first-child”选择器，不同之处就是指定了元素的类型，其主要用来定位一个父元素下的某个类型的第一个子元素，p为指定的元素类型，其后代元素继承，如`.wrapper > p:first-of-type { background: orange; }`
    * **:nth-of-type(n)选择器**：只计算父元素中指定的某种类型的子元素，同样，"n"参数可以是具体的整数，也可以是表达式，还可以是关键词（如odd为单数行，even为偶数行），如`.wrapper > p:nth-of-type(2n){ background: orange; }`
    * **:last-of-type选择器**：选择是父元素下的某个类型的最后一个子元素，而不是父元素的最后一个子元素，如`.wrapper > p:last-of-type{ background: orange; }`
    * **:nth-last-of-type(n)选择器**：选择父元素中指定的某种子元素类型，但它的起始方向是从最后一个子元素开始，如`.wrapper > p:nth-last-of-type(3){ background: orange; }`
    * **:only-of-type选择器**：用来选择一个元素是它的父元素的唯一一个相同类型的子元素，匹配父元素的所有子元素中只有一种类型的子元素，如`.wrapper > div:only-of-type {  background: orange; }`

  * 状态选择
    * **:enabled选择器**：有些表单元素有可用（“:enabled”）和不可用（“:disabled”）状态，可以通过伪选择器“:enabled”对这些表单元素设置样式，“:disabled”选择器用来选择不可用表单元素，如`input[type="text"]:enabled { background: #ccc;border: 2px solid red; }`
    * **:checked选择器**：在表单元素中，单选按钮和复选按钮都具有选中和未选中状态，以通过状态选择器“:checked”配合其他标签实现自定义样式，而“:checked”表示的是选中状态，如`input[type="checkbox"]:checked + span { opacity: 1 }`
    * **::selection选择器**：是用来匹配突出显示的文本(用鼠标选择文本时的文本)，浏览器默认情况下，用鼠标选择网页文本是以“深蓝的背景，白色的字体”显示的，不过在Firefox浏览器还需要添加前缀，如`::selection { background: red;color: green; }`
    * **:read-only选择器**：用来指定处于只读状态元素的样式，即元素中设置了“readonly=’readonly’”，如`input[type="text"]:read-only{ border-color: #ccc; }`
  * **:read-write选择器**：指定当元素处于非只读状态时的样式，如`input[type="text"]:read-write{ border-color: #f36; }`

  * 插入选择
    * **::before选择器**和**::after选择器**：这两个主要用来给元素里面的子元素的前面或后面插入内容，这两个常和"content"配合使用，即添加一个空元素，对空元素添加样式，使用的场景最多的就是清除浮动，如：
      * 清除浮动：`.clearfix::before,.clearfix::after { content: ".";display: block;height: 0;visibility: hidden; }`
      * 添加box-shadow做投影层：`.box::after{ position:absolute;z-index:-1;box-shadow:0 0 20px rgba(0,0,0,0.8); }`
      * 改变悬浮透明度：`.slider li a:hover::before{ opacity:0; }`

* 选择器属性
  * **继承**：允许样式不仅应用于某个特定html标签元素，而且应用于其后代，注意有一些css样式是不具有继承性的，如边框样式：border:1px solid red;
  * **特殊性**：标签的权值为1，类选择符的权值为10，ID选择符的权值最高为100，内联样式表的权值为 1000，继承的权值最低为0.1，如多个选择器时，则需对权值相加，则样式选择最高权值和；
  * **层叠**：后面的样式覆盖前面的样式，属于就近原则，内联样式表（标签内部）> 嵌入样式表（当前文件中）> 外部样式表（外部文件中）；
  * **重要性**：设置具有最高权值，可以使用!important，写在；之前，!important高于用户设置的样式，一般不常用，只用于不清楚其他成员或第三方CSS样式时，避免被覆盖；

* 选择器优先级
  * 不同级别：!important > 行内样式>ID选择器 > 类选择器 > 标签 > 通配符 > 继承 > 浏览器默认属性
  * 同一级别：就近原则，后写的会覆盖先写的样式；
  * !important权重最高，继承的属性权重最低；
  * 建议使用低权重原则，方便模块化，组件化，充分发挥继承性；
