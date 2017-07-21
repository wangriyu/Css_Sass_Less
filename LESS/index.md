# LESS

Less 是一门 CSS 预处理语言，它扩展了 CSS 语言，增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展。
Less 可以运行在 Node 或浏览器端

官方事例
```less
  @base: #f938ab;
  
  .box-shadow(@style, @c) when (iscolor(@c)) {
    -webkit-box-shadow: @style @c;
    box-shadow:         @style @c;
  }
  .box-shadow(@style, @alpha: 50%) when (isnumber(@alpha)) {
    .box-shadow(@style, rgba(0, 0, 0, @alpha));
  }
  .box {
    color: saturate(@base, 5%);
    border-color: lighten(@base, 30%);
    div { .box-shadow(0 0 5px, 30%) }
  }
```
输出CSS
```css
  .box {
    color: #fe33ac;
    border-color: #fdcdea;
  }
  .box div {
    -webkit-box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
  }
```

### 安装

```
$ npm install -g less
```
转换文件
```
$ lessc styles.less > styles.css
```
如果有 clean-css 插件，还可以转为min.css文件
```
$ lessc --clean-css styles.less styles.min.css
```
项目引入less.js
```
<link rel="stylesheet/less" type="text/css" href="styles.less">
<script src="less.js" type="text/javascript"></script>

/* CDN src="//cdnjs.cloudflare.com/ajax/libs/less.js/2.5.3/less.min.js" */

/* 或者 npm install less */
```

## 语法

### 变量

变量允许我们单独定义一系列通用的样式，然后在需要的时候去调用.
所以在做全局样式调整的时候我们可能只需要修改几行代码就可以了

```less
// LESS
  @color: #4D926F;
  
  #header {
    color: @color;
  }
  h2 {
    color: @color;
  }
  
  // CSS
  #header {
    color: #4D926F;
  }
  h2 {
    color: #4D926F;
  }
```

### 混合

混合可以将一个定义好的class A轻松的引入到另一个class B中
从而简单实现class B继承class A中的所有属性。我们还可以带参数地调用，就像使用函数一样

```less
// LESS
  .rounded-corners (@radius: 5px) {
    border-radius: @radius;
    -webkit-border-radius: @radius;
    -moz-border-radius: @radius;
  }
  
  #header {
    .rounded-corners;
  }
  #footer {
    .rounded-corners(10px);
  }

// CSS
  #header {
   border-radius: 5px;
   -webkit-border-radius: 5px;
   -moz-border-radius: 5px;
  }
  #footer {
   border-radius: 10px;
   -webkit-border-radius: 10px;
   -moz-border-radius: 10px;
  }
```
```less
// LESS
.box-shadow (@x: 0, @y: 0, @blur: 1px, @color: #000) {
  box-shadow: @arguments;
  -moz-box-shadow: @arguments;
  -webkit-box-shadow: @arguments;
}
.box-shadow(2px, 5px);

// CSS
{
  box-shadow: 2px 5px 1px #000;
  -moz-box-shadow: 2px 5px 1px #000;
  -webkit-box-shadow: 2px 5px 1px #000;
}
```

### 嵌套规则

我们可以在一个选择器中嵌套另一个选择器来实现继承，这样很大程度减少了代码量，并且代码看起来更加的清晰

```less
// LESS
  #header {
    h1 {
      font-size: 26px;
      font-weight: bold;
    }
    p { font-size: 12px;
      a { text-decoration: none;
        &:hover { border-width: 1px }
      }
    }
  }

// CSS
  #header h1 {
    font-size: 26px;
    font-weight: bold;
  }
  #header p {
    font-size: 12px;
  }
  #header p a {
    text-decoration: none;
  }
  #header p a:hover {
    border-width: 1px;
  }
```
& 符号的使用: 如果你想写串联选择器，而不是写后代选择器，就可以用到&了. 这点对伪类尤其有用如 :hover 和 :focus

### 函数 & 运算

运算提供了加，减，乘，除操作；我们可以做属性值和颜色的运算，这样就可以实现属性值之间的复杂关系.
LESS中的函数一一映射了JavaScript代码，如果你愿意的话可以操作属性值

```less
// LESS
@the-border: 1px;
@base-color: #111;
@red:        #842210;

#header {
  color: @base-color * 3;
  border-left: @the-border;
  border-right: @the-border * 2;
}
#footer { 
  color: @base-color + #003300;
  border-color: desaturate(@red, 10%);
}

// CSS
#header {
  color: #333;
  border-left: 1px;
  border-right: 2px;
}
#footer { 
  color: #114411;
  border-color: #7d2717;
}
```

**颜色运算**

LESS 提供了一系列的颜色运算函数. 颜色会先被转化成 HSL 色彩空间, 然后在通道级别操作
```
lighten(@color, 10%);     // return a color which is 10% *lighter* than @color
darken(@color, 10%);      // return a color which is 10% *darker* than @color

saturate(@color, 10%);    // return a color 10% *more* saturated than @color
desaturate(@color, 10%);  // return a color 10% *less* saturated than @color

fadein(@color, 10%);      // return a color 10% *less* transparent than @color
fadeout(@color, 10%);     // return a color 10% *more* transparent than @color
fade(@color, 50%);        // return @color with 50% transparency

spin(@color, 10);         // return a color with a 10 degree larger in hue than @color
spin(@color, -10);        // return a color with a 10 degree smaller hue than @color

mix(@color1, @color2);    // return a mix of @color1 and @color2
```

**Math 函数**

LESS提供了一组方便的数学函数，你可以使用它们处理一些数字类型的值
```less
round(1.67); // returns `2`
ceil(2.4);   // returns `3`
floor(2.6);  // returns `2`
percentage(0.5); // returns `50%`
```

### 模式匹配和导引表达式


when关键字用以定义一个导引序列(此例只有一个导引)
```less
.mixin (@a) when (lightness(@a) >= 50%) {
  background-color: black;
}
.mixin (@a) when (lightness(@a) < 50%) {
  background-color: white;
}
.mixin (@a) {
  color: @a;
}
```
```less
.class1 { .mixin(#ddd) }
.class2 { .mixin(#555) }

// CSS
.class1 {
  background-color: black;
  color: #ddd;
}
.class2 {
  background-color: white;
  color: #555;
}
```

---

导引中可用的全部比较运算有： > >= = =< <。此外，关键字true只表示布尔真值，下面两个混合是相同的
```less
.truth (@a) when (@a) { ... }
.truth (@a) when (@a = true) { ... }
```
除去关键字true以外的值都被视示布尔假

---

导引序列使用逗号‘,’—分割，当且仅当所有条件都符合时，才会被视为匹配成功
```less
.mixin (@a) when (@a > 10), (@a < -10) { ... }
```

---

导引可以无参数，也可以对参数进行比较运算
```
@media: mobile;

.mixin (@a) when (@media = mobile) { ... }
.mixin (@a) when (@media = desktop) { ... }

.max (@a, @b) when (@a > @b) { width: @a }
.max (@a, @b) when (@a < @b) { width: @b }
```

---

如果想基于值的类型进行匹配，我们就可以使用is*函式

常见的检测函式：
- iscolor
- isnumber
- isstring
- iskeyword
- isurl
```less
.mixin (@a, @b: 0) when (isnumber(@b)) { ... }
.mixin (@a, @b: black) when (iscolor(@b)) { ... }
```
如果你想判断一个值是纯数字，还是某个单位量，可以使用下列函式
- ispixel
- ispercentage
- isem

---

在导引序列中可以使用and关键字实现与条件, not关键字实现或条件
```less
.mixin (@a) when (isnumber(@a)) and (@a > 0) { ... }

.mixin (@b) when not (@b > 0) { ... }
```

### 作用域

LESS 中的作用域跟其他编程语言非常类似，首先会从本地查找变量或者混合模块，如果没找到的话会去父级作用域中查找，直到找到为止
```less
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}

#footer {
  color: @var; // red  
}
```

### 导入Importing

你可以在main文件中通过下面的形势引入 .less 文件, .less 后缀可带可不带
```
@import "lib.less";
@import "lib";
```
如果你想导入一个CSS文件而且不想LESS对它进行处理，只需要使用.css后缀就可以
```
@import "lib.css";
```

### 字符串插值

变量可以用类似ruby和php的方式嵌入到字符串中，像@{name}这样的结构
```
@base-url: "http://assets.fnord.com";
background-image: url("@{base-url}/images/bg.png");
```

### JavaScript 表达式

JavaScript 表达式也可以在.less 文件中使用. 可以通过反引号的方式使用:
```less
@var: `"hello".toUpperCase() + '!'`; // -> @var: "HELLO!";
```

### 避免编译

有时候我们需要输出一些不正确的CSS语法或者使用一些 LESS不认识的专有语法.

要输出这样的值我们可以在字符串前加上一个 ~
```less
.class {
  filter: ~"ms:alwaysHasItsOwnSyntax.For.Stuff()";
}
```