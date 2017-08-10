# Sass-note
Sass笔记

----------
#Sass
###Sass安装

* 安装Ruby用控制台输入`gem install sass`

* 更新sass命令`gem update sass`

###Sass编译

* 命令行编译`sass <要编译的Sass文件路径>:<要输出CSS文件路径>`，这条命令只能一次性编译。

* 开启“watch”功能，这样只要你的代码进行任保修改，都能自动监测到代码的变化，并且给你直接编译出来`sass --watch <要编译的Sass文件路径>:<要输出CSS文件路径>` 

* Sass 中编译出来的样式风格也可以按不同的样式风格显示，嵌套输出方式: nested，展开输出方式 expanded, 紧凑输出方式 compact,压缩输出方式 compressed

    > 嵌套输出方式nested,编译时加上`sass --watch <要编译的Sass文件路径>:<要输出CSS文件路径> --style nested`
    
###Sass变量声明

* sass的变量包含三个部分，1.声明变量的符号'$'，2.变量名称，3.赋予变量的值

* sass 的默认变量仅需要在值后面加上 !default 即可

* sass 的默认变量一般是用来设置默认值，然后根据需求来覆盖的，覆盖的方式也很简单，只需要在默认变量之前重新声明下变量即可

		$baseLineHeight: 2;
		$baseLineHeight: 1.5 !default;
		body{
    		line-height: $baseLineHeight; 
		}
		//line-height: 2;

###sass混合宏

* sass声明混合宏，在Sass中，使用“@mixin”来声明一个混合宏，`@minxin`是声明混合宏的关键字
		
		//不带参数的混合宏
		@mixin border-radius{   //@mixin声明宏的关键字，border-radius宏的名称
   	    	-webkit-border-radius: 5px;
        	border-radius: 5px;
		}			
		//带参数的混合宏
		@mixin border-radius($radius:5px){
    		-webkit-border-radius: $radius;
    		border-radius: $radius;
		}

* sass通过关键字`@include`来调用声明好的宏。
		
		//调用上面声明好的border-radius宏
		button {
    		@include border-radius;
		}

* sass混合宏传参
		
		//传不带值的参数
		@mixin border-radius($radius){
 			-webkit-border-radius: $radius;
  			border-radius: $radius;
		}
		.box {
  			@include border-radius(3px);//在调用时给参数设置一个值
		}
		//传带值的参数
		@mixin border-radius($radius:3px){
 			-webkit-border-radius: $radius;
  			border-radius: $radius;
		}
		.box {
  			@include border-radius;//直接调用，参数默认值3px
		}
		.box {
			@include boder-radius(50%);//调用时另外赋一个值，默认值会被修改为新值
		}
		//还可以传递多个参数，但参数过多时可以用'...'代替	

	Sass 在调用相同的混合宏时，并不能智能的将相同的样式代码块合并在一起。这也是 Sass 的混合宏最不足之处，会生成冗余的代码块

###sass扩展和继承

* Sass 中是通过关键词 `@extend`来继承已存在的类样式块，从而实现代码的继承。在 Sass 中的继承，可以继承类样式块中所有样式代码，而且编译出来的 CSS 会将选择器合并在一起，形成组合选择器

* Sass 中的占位符`%placeholder`功能，`%placeholder`声明的代码，如果不被 @extend 调用的话，不会产生任何代码
		
		//SCSS
		%mt5 {
  			margin-top: 5px;
		}
		%pt5{
  			padding-top: 5px;
		}
		.btn {
  			@extend %mt5;
 			@extend %pt5;
		}
		.block {
  			@extend %mt5;
  			span {
   			 	@extend %pt5;
  			}
		}	
		//编译出的css
		.btn, .block {
  			margin-top: 5px;
		}
		.btn, .block span {
  			padding-top: 5px;
		}
		//从编译出来的 CSS 代码可以看出，通过 @extend 调用的占位符，编译出来的代码会将相同的代码合并在一起

###关于混合宏、继承和占位符的使用

* 如果你的代码块中涉及到变量，建议使用混合宏来创建相同的代码块

* 如果你的代码块不需要专任何变量参数，而且有一个基类已在文件中存在，那么建议使用 Sass 的继承

* 编译出来的 CSS 代码和使用继承基本上是相同，只是不会在代码中生成占位符的选择器。那么占位符和继承的主要区别的，“占位符是独立定义，不调用的时候是不会在 CSS 中产生任何代码；继承是首先有一个基类存在，不管调用与不调用，基类的样式都将会出现在编译出来的 CSS 代码中”
![混合宏、继承和占位符的区别](http://img.mukewang.com/55263aa30001913307940364.jpg)	
###sass运算符
* 加法运算是 Sass 中运算中的一种，在变量或属性中都可以做加法运算，但对于携带不同类型的单位时，在 Sass 中计算会报错

* 减法运算和加法运算相似

* Sass 中的乘法运算和前面介绍的加法与减法运算还略有不同。虽然他也能够支持多种单位（比如 em ,px , %），但当一个单位同时声明两个值时会有问题。如果进行乘法运算时，两个值单位相同时，只需要为一个数值提供单位即可。Sass 的乘法运算和加法、减法运算一样，在运算中有不同类型的单位时，也将会报错

* Sass 的乘法运算规则也适用于除法运算。不过除法运算还有一个特殊之处。众所周知“/”符号在 CSS 中已做为一种符号使用。因此在 Sass 中做除法运算时，直接使用“/”符号做为除号时，将不会生效，编译时既得不到我们需要的效果，也不会报错。要修正这个问题，只需要给运算的外面添加一个小括号( )即可。当用变量进行除法运算时，“/”符号也会自动被识别成除法，综合上述，”/  ”符号被当作除法运算符时有以下几种情况：
	- 如果数值或它的任意部分是存储在一个变量中或是函数的返回值。
	- 如果数值被圆括号包围。
	- 如果数值是另一个数学表达式的一部分
	
		    //SCSS	
    		p {
    		  font: 10px/8px; // 纯 CSS，不是除法运算
    		  $width: 1000px;
    		  width: $width/2;// 使用了变量，是除法运算
    		  width: round(1.5)/2;// 使用了函数，是除法运算
    		  height: (500px/2);  // 使用了圆括号，是除法运算
    		  margin-left: 5px + 8px/2px; // 使用了加（+）号，是除法运算
    		} 

* 在除法运算时，如果两个值带有相同的单位值时，除法运算之后会得到一个不带单位的数值

* 所有算数运算都支持颜色值，并且是分段运算的。也就是说，红、绿和蓝各颜色分段单独进行运算

        p {
      		color: #010203 + #040506; // 计算公式为 01 + 04 = 05、02 + 05 = 07 和 03 + 06 = 09
    	}
    		
* 字符运算,有引号的字符串被添加了一个没有引号的字符串 （也就是，带引号的字符串在 + 符号左侧）， 结果会是一个有引号的字符串。 同样的，如果一个没有引号的字符串被添加了一个有引号的字符串 （没有引号的字符串在 + 符号左侧）， 结果将是一个没有引号的字符串
		
		p:before { 
			content: "Foo " + Bar; // content: "Foo Bar";
			font-family: sans- + "serif"; // font-family: sans-serif;
		}

* @if 指令是一个 SassScript，它可以根据条件来处理样式块，如果条件为 true 返回一个样式块，反之 false 返回另一个样式块。在 Sass 中除了 @if 之，还可以配合 @else if 和 @else 一起使用

* Sass 的 @for 循环中有两种方式：
		
		@for $i from <start> through <end>
		@for $i from <start> to <end>
		
		// $i 表示变量,start 表示起始值,end 表示结束值,两个的区别是关键字 through 表示包括 end 这个数，而 to 则不包括 end 这个数

* @while 指令也需要 SassScript 表达式（像其他指令一样），并且会生成不同的样式块，直到表达式值为 false 时停止循环。这个和 @for 指令很相似，只要 @while 后面的条件为 true 就会执行

		//SCSS
		$types: 4;
		$type-width: 20px;
		
		@while $types > 0 {
		    .while-#{$types} {
		        width: $type-width + $types;
		    }
		    $types: $types - 1;
		}

* @each 循环就是去遍历一个列表，然后从列表中取出对应的值，@each 循环指令的形式：`@each $var in <list>`

		$list: adam john wynn mason kuroir;//$list 就是一个列表
		
		@mixin author-images {
		    @each $author in $list {
		        .photo-#{$author} {
		            background: url("/images/avatars/#{$author}.png") no-repeat;
		        }
		    }
		}
		
		.author-bio {
		    @include author-images;
		}

* 字符串函数顾名思意是用来处理字符串的函数。Sass 的字符串函数主要包括两个函数：

	- unquote($string)：删除字符串中的引号
    - quote($string)：给字符串添加引号，使用 quote() 函数只能给字符串增加双引号，而且字符串中间有单引号或者空格时，需要用单引号或双引号括起，同时 quote() 碰到特殊符号，比如： !、?、> 等，除中折号 - 和 下划线_ 都需要使用双引号括起

* 字符串函数-To-upper-case()、To-lower-case()

* 数字函数提要针对数字方面提供一系列的函数功能：
	- percentage($value)：将一个不带单位的数转换成百分比值；转换的值是一个带有单位的值会报错
	- round($value)：将数值四舍五入，转换成一个最接近的整数；
	- ceil($value)：将大于自己的小数转换成下一位整数；
	- floor($value)：将一个数去除他的小数部分；
	- abs($value)：返回一个数的绝对值；
	- min($numbers…)：找出几个数值之间的最小值；同时出现两种不同类型的单位
	- max($numbers…)：找出几个数值之间的最大值；同时出现两种不同类型的单位
	- random(): 获取随机数

* 列表函数主要包括一些对列表参数的函数使用，主要包括以下几种：
	- length($list)：返回一个列表的长度值；函数中的列表参数之间使用空格隔开，不能使用逗号
	- nth($list, $n)：返回一个列表中指定的某个标签值，1 是指列表中的第一个标签值
	- join($list1, $list2, [$separator])：将两个列给连接在一起，变成一个列表；只能将两个列表连接成一个列表。join() 函数中 $separator 除了默认值 auto 之外，还有 comma 和 space 两个值，其中 comma 值指定列表中的列表项值之间使用逗号（,）分隔，space 值指定列表中的列表项值之间使用空格（ ）分隔
		- 如果列表中的第一个列表中每个值之间使用的是逗号（,），那么 join() 函数合并的列表中每个列表项之间使用逗号
		- 如果列表中的第一个列表中每个值之间使用的是空格，那么 join() 函数合并的列表中每个列表项之间使用空格分隔
		- 如果当两个列表中的列表项小于1时，将会以空格分隔
	- append($list1, $val, [$separator])：将某个值放在列表的最后；	
    	- 如果列表只有一个列表项时，那么插入进来的值将和原来的值会以空格的方式分隔。
    	- 如果列表中列表项是以空格分隔列表项，那么插入进来的列表项也将以空格分隔；
    	- 如果列表中列表项是以逗号分隔列表项，那么插入进来的列表项也将以逗号分隔。
    	- append() 函数中，可以显示的设置 $separator 参数
    		- 如果取值为 comma 将会以逗号分隔列表项
    		- 如果取值为 space 将会以空格分隔列表项
	- zip($lists…)：将几个列表结合成一个多维的列表；使用zip()函数时，每个单一的列表个数值必须是相同的
	- index($list, $value)：返回一个值在列表中的位置值。index() 函数中，如果指定的值不在列表中（没有找到相应的值），那么返回的值将是 false，相反就会返回对应的值在列表中所处的位置

* ntrospection 函数包括了几个判断型函数：
    - type-of($value)：返回一个值的类型
    	- number 为数值型。
		- string 为字符串型。
		- bool 为布尔型。
		- color 为颜色型
    - unit($number)：返回一个值的单位。碰到复杂的计算时，其能根据运算得到一个“多单位组合”的值，不过只充许乘、除运算
    - unitless($number)：判断一个值是否带有单位
    - comparable($number-1, $number-2)：判断两个值是否可以做加、减和合并

* 三元条件函数，`if($condition,$if-true,$if-false)`

* Sass 的 map 常常被称为数据地图，也有人称其为数组，因为他总是以 key:value 成对的出现，但其更像是一个 JSON 数据

		$map: (
		    $key1: value1,
		    $key2: value2,
		    $key3: value3
		)
		// 对于 Sass 的 map，还可以让 map 嵌套 map
		$theme-color: ( 
			default: ( bgcolor: #fff, text-color: #444, link-color: #39f ), 
			primary:( bgcolor: #000, text-color:#fff, link-color: #93f ), 
			negative: ( bgcolor: #f36, text-color: #fefefe, link-color: #d4e ) 
		);

* Sass 中 map 自身带了七个函数：
    - map-get($map,$key)：根据给定的 key 值，返回 map 中相关的值。
    - map-merge($map1,$map2)：将两个 map 合并成一个新的 map。
    - map-remove($map,$key)：从 map 中删除一个 key，返回一个新 map。
    - map-keys($map)：返回 map 中所有的 key。
    - map-values($map)：返回 map 中所有的 value。
    - map-has-key($map,$key)：根据给定的 key 值判断 map 是否有对应的 value 值，如果有返回 true，否则返回 false。
    - keywords($args)：返回一个函数的参数，这个参数可以动态的设置 key 和 value

* RGB 颜色只是颜色中的一种表达式，其中 R 是 red 表示红色，G 是 green 表示绿色而 B 是 blue 表示蓝色。在 Sass 中为 RGB 颜色提供六种函数：
    - rgb($red,$green,$blue)：根据红、绿、蓝三个值创建一个颜色；rgb() 函数只能快速的将一个 rgb 颜色转换成十六进制颜色表达
    - rgba($red,$green,$blue,$alpha)：根据红、绿、蓝和透明度值创建一个颜色；
    - red($color)：从一个颜色中获取其中红色值；
    - green($color)：从一个颜色中获取其中绿色值；
    - blue($color)：从一个颜色中获取其中蓝色值；
    - mix($color-1,$color-2,[$weight])：把两种颜色混合在一起，$weight 为 合并的比例（选择权重）

* HSL 颜色函数包括哪些具体的函数，所起的作用是什么：
	- hsl($hue,$saturation,$lightness)：通过色相（hue）、饱和度(saturation)和亮度（lightness）的值创建一个颜色；
	- hsla($hue,$saturation,$lightness,$alpha)：通过色相（hue）、饱和度(saturation)、亮度（lightness）和透明（alpha）的值创建一个颜色；
	- hue($color)：从一个颜色中获取色相（hue）值；
	- saturation($color)：从一个颜色中获取饱和度（saturation）值；
	- lightness($color)：从一个颜色中获取亮度（lightness）值；
	- adjust-hue($color,$degrees)：通过改变一个颜色的色相值，创建一个新的颜色；
	- lighten($color,$amount)：通过改变颜色的亮度值，让颜色变亮，创建一个新的颜色；
	- darken($color,$amount)：通过改变颜色的亮度值，让颜色变暗，创建一个新的颜色；
	- saturate($color,$amount)：通过改变颜色的饱和度值，让颜色更饱和，从而创建一个新的颜色
	- desaturate($color,$amount)：通过改变颜色的饱和度值，让颜色更少的饱和，从而创建出一个新的颜色；
	- grayscale($color)：将一个颜色变成灰色，相当于desaturate($color,100%);
	- complement($color)：返回一个补充色，相当于adjust-hue($color,180deg);
	invert($color)：反回一个反相色，红、绿、蓝色值倒过来，而透明度不变。

* @import，引入 SCSS 和 Sass 文件
    - 如果文件的扩展名是 .css。
    - 如果文件名以 http:// 开头。
    - 如果文件名是 url()。
    - 如果 @import 包含了任何媒体查询（media queries）。

* @media 指令

* Sass 中的 @extend 是用来扩展选择器或占位符

* @at-root 从字面上解释就是跳出根元素。当你选择器嵌套多层之后，想让某个选择器跳出，此时就可以使用 @at-root

* @debug 在 Sass 中是用来调试的，当你的在 Sass 的源码中使用了 @debug 指令之后，Sass 代码在编译出错时，在命令终端会输出你设置的提示 Bug

* @warn 和 @debug 功能类似，用来帮助我们更好的调试 Sass

* @error 和 @warn、@debug 功能是如出一辙



	
      
