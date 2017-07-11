# Sass-note
Sass笔记

----------
#Sass
###Sass安装
* 安装Ruby用控制台输入`gem install sass`
* 更新sass命令`gem update sass`
###Sass编译
* 命令行编译`sass <要编译的Sass文件路径>:<要输出CSS文件路径>`，这条命令只能一次性编译。
* 开启“watch”功能，这样只要你的代码进行任保修改，都能自动监测到代码的变化，并且给你直接编译出来`sass --watch <要编译的Sass文件路径>:<要输出CSS文件路径>` 。
* Sass 中编译出来的样式风格也可以按不同的样式风格显示，嵌套输出方式: nested，展开输出方式 expanded, 紧凑输出方式 compact,压缩输出方式 compressed。
    >嵌套输出方式nested,编译时加上`sass --watch <要编译的Sass文件路径>:<要输出CSS文件路径> --style nested`。
###Sass变量声明
* sass的变量包含三个部分，1.声明变量的符号'$'，2.变量名称，3.赋予变量的值。
* sass 的默认变量仅需要在值后面加上 !default 即可。
* sass 的默认变量一般是用来设置默认值，然后根据需求来覆盖的，覆盖的方式也很简单，只需要在默认变量之前重新声明下变量即可。

		$baseLineHeight: 2;
		$baseLineHeight: 1.5 !default;
		body{
    		line-height: $baseLineHeight; 
		}
		//line-height: 2;
###sass混合宏
* sass声明混合宏，在Sass中，使用“@mixin”来声明一个混合宏，`@minxin`是声明混合宏的关键字。
		
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

	Sass 在调用相同的混合宏时，并不能智能的将相同的样式代码块合并在一起。这也是 Sass 的混合宏最不足之处，会生成冗余的代码块。
###sass扩展和继承
* Sass 中是通过关键词 `@extend`来继承已存在的类样式块，从而实现代码的继承。在 Sass 中的继承，可以继承类样式块中所有样式代码，而且编译出来的 CSS 会将选择器合并在一起，形成组合选择器。
* Sass 中的占位符`%placeholder`功能，`%placeholder`声明的代码，如果不被 @extend 调用的话，不会产生任何代码。
		
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
		//从编译出来的 CSS 代码可以看出，通过 @extend 调用的占位符，编译出来的代码会将相同的代码合并在一起。
###关于混合宏、继承和占位符的使用
* 如果你的代码块中涉及到变量，建议使用混合宏来创建相同的代码块。
* 如果你的代码块不需要专任何变量参数，而且有一个基类已在文件中存在，那么建议使用 Sass 的继承。
* 编译出来的 CSS 代码和使用继承基本上是相同，只是不会在代码中生成占位符的选择器。那么占位符和继承的主要区别的，“占位符是独立定义，不调用的时候是不会在 CSS 中产生任何代码；继承是首先有一个基类存在，不管调用与不调用，基类的样式都将会出现在编译出来的 CSS 代码中”。
![混合宏、继承和占位符的区别](http://img.mukewang.com/55263aa30001913307940364.jpg)	
###sass运算符
* 加法运算是 Sass 中运算中的一种，在变量或属性中都可以做加法运算，但对于携带不同类型的单位时，在 Sass 中计算会报错。
* 减法运算和加法运算相似。
* Sass 中的乘法运算和前面介绍的加法与减法运算还略有不同。虽然他也能够支持多种单位（比如 em ,px , %），但当一个单位同时声明两个值时会有问题。如果进行乘法运算时，两个值单位相同时，只需要为一个数值提供单位即可。Sass 的乘法运算和加法、减法运算一样，在运算中有不同类型的单位时，也将会报错。
* Sass 的乘法运算规则也适用于除法运算。不过除法运算还有一个特殊之处。众所周知“/”符号在 CSS 中已做为一种符号使用。因此在 Sass 中做除法运算时，直接使用“/”符号做为除号时，将不会生效，编译时既得不到我们需要的效果，也不会报错。要修正这个问题，只需要给运算的外面添加一个小括号( )即可。
		
		//给运算的外面添加一个小括号
		.box {
  			width: (100px / 2);  
		}
		//除了上面情况带有小括号，“/”符号会当作除法运算符之外，如果“/”符号在已有的数学表达式中时，也会被认作除法符号
		.box {
  			width: 100px / 2 + 2in;  
		}
		//在 Sass 除法运算中，当用变量进行除法运算时，“/”符号也会自动被识别成除法
		$width: 1000px;
		.item {
  			width: $width / 10;  
		}

