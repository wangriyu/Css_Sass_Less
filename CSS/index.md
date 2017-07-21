# CSS

### display

inline       | 默认。此元素会被显示为内联元素，元素前后没有换行符
:-------:    | :----:
none         | 不显示
block        | 此元素将显示为块级元素，此元素前后会带有换行符
inline-block | 行内块元素
table        | 此元素会作为块级表格来显示，这个元素的作用就像 table 标签元素.表格前后带有换行符
table-column | 这个元素的作用就像 col 标签一样
table-row    | 这个元素的作用就像 tr 标签一样
table-cell   | 这个元素的作用就像 td 标签一样
inline-table | 此元素会作为内联表格来显示，表格前后没有换行符
inherit      | 规定应该从父元素继承 display 属性的值
flex         | 作为块级元素的弹性盒模型Flex
inline-flex  | 作为行内元素的弹性盒模型Flex
grid         | 作为块级元素的栅格盒模型Grid
inline-grid  | 作为行内元素的栅格盒模型Grid

block元素通常被现实为独立的一块，会单独换一行；inline元素则前后不会产生换行，一系列inline元素都在一行内显示，直到该行排满
- 常见的块级元素有 div, form, table, p, pre, h1~h6, dl, ol, ul, li 等
- 常见的内联元素有 span, a, strong, em, label, input, select, textarea, img, br 等

1.display: block

- block元素会独占一行，多个block元素会各自新起一行。默认情况下，block元素宽度自动填满其父元素宽度。
- block元素可以设置width,height属性。块级元素即使设置了宽度,仍然是独占一行。
- block元素可以设置margin和padding属性。

2.display: inline

- inline元素不会独占一行，多个相邻的行内元素会排列在同一行里，直到一行排列不下，才会新换一行，其宽度随元素的内容而变化。
- inline元素设置width,height属性无效。
- inline元素的margin和padding属性，水平方向的padding-left, padding-right, margin-left, margin-right都产生边距效果；但竖直方向的padding-top, padding-bottom, margin-top, margin-bottom不会产生边距效果。

3.display: inline-block

- 简单来说就是将对象呈现为inline对象，但是对象的内容作为block对象呈现。之后的内联对象会被排列在同一行内。比如我们可以给一个link（a元素）inline-block属性值，使其既具有block的宽度高度特性又具有inline的同行特性

4.display: flex

- 父元素设置display: flex，所有子元素横向排列
- 父元素设置flex-direction: row横向排列(默认)、column纵向排列、row-reverse反向排列、column-reverse反向排列
- 父元素设置justify-content: flex-start沿主轴开头、flex-end沿主轴末尾、center居中、space-between均匀排开、space-around间隔均匀(开头末尾也有间距)
- 父元素设置align-items: flex-start沿次轴开头、flex-end沿次轴末尾、center居中、stretch拉伸、baseline子元素第一行对齐
- 父元素设置flex-wrap: nowrap所有元素排在一行(默认)、wrap超出换行、wrap-reverse从底向上换行
- 子元素设置order: integer 按数字大小排列先后顺序，可以负数
- 子元素设置flex-grow: number 按数字比例占据容器，正整数


### position

position: static/relative/fixed/absolute

- static是默认值，任意position: static的元素不会被特殊的定位，仍位于原来的文本流中
- relative不设置top等属性时跟static表现的一样，
  在一个相对定位(position属性的值为relative)的元素上设置top、right、bottom和left属性会使其偏离原文本流位置，
  其他的元素的位置则不会受该元素的影响发生位置改变来弥补它偏离后剩下的空隙
- fixed,一个固定定位（position属性的值为fixed）元素会相对于视窗来定位，这意味着即便页面滚动
  它还是会停留在相同的位置,不会保留它原本在页面应有的空隙（脱离文档流）
- absolute相对于最近的“positioned”祖先元素。如果绝对定位（position属性的值为absolute）的元素没有“positioned”祖先元素,
  那么它是相对于文档的body元素(窗口)，并且它会随着页面滚动而移动
  
### 分列式布局Multi-column

- column-count: 列数目
- column-gap: 各列之间间隙宽度
- column-rule-style: 列之间分割线风格
- column-rule-width: 列之间分割线宽度
- column-rule-color: 列之间分割线颜色
- column-rule: width style color 指定分割线
- column-span: 1/all/initial/inherit 允许一个元素的宽度跨越多列
- column-width: 建议宽度；未必会使用，浏览器基于此数值进行计算

```
/* 3 列，每列之间10px间距, 带有金色的隔离线 */
.col-3 {
  column-count: 3;
  column-gap: 10px;
  column-rule: 1px solid #fc0;
}
```

### 媒体查询media queries

使用 @media 查询，你可以针对不同的媒体类型定义不同的样式
```
/* 如果浏览器窗口小于 500px, 背景将变为浅蓝色 */
@media only screen and (max-width: 500px) {
    body {
        background-color: lightblue;
    }
}
```

**添加断点**

使用媒体查询在 768px 添加断点，改变不同设备分辨率下的效果

桌面设备

![浏览器](http://www.runoob.com/wp-content/uploads/2015/06/rwd_desktop.png)

移动设备

![手机](http://www.runoob.com/wp-content/uploads/2015/06/rwd_phone.png)

平板设备

![平板](http://www.runoob.com/wp-content/uploads/2015/06/rwd_tablet.png)

**方向：横屏/竖屏**

结合CSS媒体查询,可以创建适应不同设备的方向(横屏landscape、竖屏portrait等)的布局

```
/* orientation：portrait | landscape */
/* 如果是竖屏背景将是浅蓝色 */
@media only screen and (orientation: landscape) {
    body {
        background-color: lightblue;
    }
}
```

### CSS符号

右尖括号(>)作用于子元素，加号(+)作用于兄弟元素
单冒号(:)用于CSS3伪类，双冒号(::)用于CSS3伪元素