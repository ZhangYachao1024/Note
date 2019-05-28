# # 概述

CSS Grid 布局由两个核心组成部分是 **container**（网格容器）和 **items**（网格项）。
注意：`column`, `float`, `clear`, 以及 `vertical-align` 对一个 grid container 没有影响

# # 基本概念

1. Grid Container：设置了 `display: gird ` 的元素，是所有grid item的直接父项。
2. Grid Item：Grid 容器的直接子元素。
3. Grid Line：网格间的分界线。
4. Grid Track：两个相邻网格线之间的空间，即列或行。
5. Grid Cell：网格。
6. Grid Area：任意四条网格线包围的总空间。

# # 网格父容器属性

## 01. display

将元素定义为 grid container

```css
.container {
  display: grid | inline-grid | subgrid;   // 块级网格 | 行级网格 | 嵌套网格
}
```

## 02.  grid-template-rows/grid-template-columns

设置行/列

```js
.container {
    display: grid;
    grid-template-rows: [first] 100px [second] 100px [end];	// 行高
    grid-template-columns: 200px 200px auto 200px;  // 列宽
}
```

* 单位可使用px/fr/百分比; fr => 使用网格剩余空间；
* 中括号 => 网格线名；注意多个名称使用空格隔开；
* 如果行/列包含多个重复值，可使用 `repeat()`简化；如`repeat(3, 1fr);

## 03. grid-template-areas

- `<grid-area-name>`：由网格项的 `grid-area`指定的网格区域名称
- `.`（点号） ：代表一个空的网格单元
- `none`：不定义网格区域

## 04. grid-gap

指定网格线(grid lines)的大小，即行与行或列与列之间的间距。

```css
.container {
  grid-template-columns: 100px 50px 100px;
  grid-template-rows: 80px auto 80px; 
  grid-gap: 15px 10px;  // 行间距 列间距
}
```

## 05. justify-items / align-items / place-items

**网格内容**水平对齐 / 垂直对齐 / 复合简写

```css
.container {
  justify-items: stretch | start | center | end;
  align-items: stretch | start | center | end;
  place-items: <align-items> <justify-items> // 垂直对齐 水平对齐
}
```

> `place-items`如果省略第二个值，则将第一个值同时分配给这两个属性。

## 06. justify-content / align-content / place-content 

**网格**水平对齐 / 垂直对齐 / 复合简写，总网格大小 < 网格容器大小时适用

```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;
  place-content: <align-items> <justify-items> // 垂直对齐 水平对齐
```

> `place-content`如果省略第二个值，则将第一个值同时分配给这两个属性。

## 07. grid-auto-columns / grid-auto-rows
待研究


# # 网格项属性

## 1. grid-column-start / grid-column-end / grid-row-start / grid-row-end

通过引用特定网格线（grid lines） 来确定 网格项（grid item）在网格内的位置。 [`grid-column-start`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) / [`grid-row-start`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) 是网格项开始的网格线，[`grid-column-end`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) / [`grid-row-end`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) 是网格项结束的网格线。

值：

- `<line>` ：可以是一个数字引用一个编号的网格线，或者一个名字来引用一个命名的网格线
- `span <number>` ：该网格项将跨越所提供的网格轨道数量
- `span <name>` ：该网格项将跨越到它与提供的名称位置
- `auto`：表示自动放置，自动跨度，默认会扩展一个网格轨道的宽度或者高度

```css
.item {
  grid-column-start: <number> | <name> | span <number> | span <name> | auto
  grid-column-end: <number> | <name> | span <number> | span <name> | auto
  grid-row-start: <number> | <name> | span <number> | span <name> | auto
  grid-row-end: <number> | <name> | span <number> | span <name> | auto
}
```

代码示例：

```css
.item-a {
  grid-column-start: 2;
  grid-column-end: five;
  grid-row-start: row1-start
  grid-row-end: 3;
}
```

![](IMGS/grid-column-row-start-end-01.svg)

```css
.item-b {
  grid-column-start: 1;
  grid-column-end: span col4-start;
  grid-row-start: 2
  grid-row-end: span 2
}
```

![](IMGS/grid-column-row-start-end-02.svg)

如果没有声明指定 [`grid-column-end`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) / [`grid-row-end`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end)，默认情况下，该网格项将占据 1 个轨道。

项目可以相互重叠。您可以使用 `z-index` 来控制它们的重叠顺序。

## 2. grid-column / grid-row

分别为 [`grid-column-start`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) + [`grid-column-end`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) 和 [`grid-row-start`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) + [`grid-row-end`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) 的简写形式。

值：

- `<start-line> / <end-line>`：每个网格项都接受所有相同的值，作为普通书写的版本，包括跨度

语法如下：

```css
.item {
  grid-column: <start-line> / <end-line> | <start-line> / span <value>;
  grid-row: <start-line> / <end-line> | <start-line> / span <value>;
}
```

代码示例：

```css
.item-c {
  grid-column: 3 / span 2;
  grid-row: third-line / 4;
}
```

![](IMGS/grid-column-row.svg)

> 注意：如果没有声明分隔线结束位置，则该网格项默认占据 1 个网格轨道。

## 3. grid-area

为网格项提供一个名称，以便可以被使用网格容器 [`grid-template-areas`](https://www.css88.com/archives/8510#prop-grid-template-areas) 属性创建的模板进行引用。 另外，这个属性可以用作[`grid-row-start`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) + [`grid-column-start`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) + [`grid-row-end`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) + [`grid-column-end`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end)的简写。

值：

- `<name>`：你所选的名称
- `<row-start> / <column-start> / <row-end> / <column-end>`：数字或分隔线名称

语法：

```css
.item {
  grid-area: <name> | <row-start> / <column-start> / <row-end> / <column-end>;
}
```

示例：

作为为网格项分配名称的一种方法：

```css
.item-d {
  grid-area: "header"
}
```

作为[`grid-row-start`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) + [`grid-column-start`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) + [`grid-row-end`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) + [`grid-column-end`](https://www.css88.com/archives/8510#prop-grid-column-row-start-end) 属性的简写形式：

```css
.item-d {
    grid-area: 1 / col4-start / last-line / 6
}
```

![](IMGS/grid-area.svg)

## 4. justify-self

沿着 *inline*（行/水平/X）轴线对齐网格项（ 相反的属性是 [`align-self`](https://www.css88.com/archives/8510#prop-align-self) ，沿着 *block*（列）轴线对齐）。此值适用于单个网格项内的内容。

值：

- `start`：将网格项对齐到其单元格的左侧起始边缘（左侧对齐）
- `end`：将网格项对齐到其单元格的右侧结束边缘（右侧对齐）
- `center`：将网格项对齐到其单元格的水平中间位置（水平居中对齐）
- `stretch`：填满单元格的宽度（默认值）

语法形式：

```css
.item {
  justify-self: start | end | center | stretch;
}
```

该属性和容器中指定 *justify-items* 是一致，区别在于 *justify-items* 是统一设置所有items的布局，而 *justify-self* 是设置指定某个item的布局，这里不再演示具体效果。

## 5. align-self

沿着 *block*（列/垂直/Y）轴线对齐网格项(grid items)（ 相反的属性是 [`justify-self`](https://www.css88.com/archives/8510#prop-justify-self) ，沿着 *inline*（行）轴线对齐）。此值适用于单个网格项内的内容。

该属性和 *justify-self* 属性一致，二者都是设置某个指定item在网格中的位置，区别在于 *justify-self* 是设置水平位置，而 *align-self* 是设置垂直位置。

## 6. place-self

`place-self` 是设置 `align-self` 和 `justify-self` 的简写形式。

值：

- `auto` – 布局模式的 “默认” 对齐方式。
- `<align-self> <justify-self>`：第一个值设置 `align-self` 属性，第二个值设置 `justify-self`属性。如果省略第二个值，则将第一个值同时分配给这两个属性。



























