# # 概念

  采用 Flex 布局的元素，称为 **Flex 父容器**（flex container）。它的所有子元素称为 **Flex 元素**（flex item)。注意：设为 Flex 布局以后，子元素的 `float`、`clear` 和 `vertical-align` 属性将失效。

![](IMGS/flex_concept.png)

   **Flex 父容器默认存在两根轴**：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做 `main start`，结束位置叫做 `main end`；交叉轴的开始位置叫做 `cross start`，结束位置叫做 `cross end`。

  **Flex元素默认沿主轴排列**。单个元素占据的主轴空间叫做 `main size`，占据的交叉轴空间叫做 `cross size`。

# # Flex父容器的属性

## 1、flex-direction -- 主轴方向(即子元素的排列方向)

```css
flex-direction: row | row-reverse | column | column-reverse;
```

## 2、flex-wrap -- 子元素是否换行

```CSS
flex-wrap: nowrap | wrap | wrap-reverse;
```


## 3、flex-flow -- 复合属性

`flex-flow` 属性是 `flex-direction` 属性和 `flex-wrap` 属性的简写形式，默认值为 `row nowrap`。

```css
flex-flow: <flex-direction> || <flex-wrap>;
```

## 4、justify-content -- 子元素在主轴上的对齐方式

```css
justify-content: flex-start | flex-end | center | space-between | space-around;
```

## 5、align-items -- 单行子元素在交叉轴上的对齐方式

```css
align-items: stretch | flex-start | flex-end | center | baseline;
```


- `stretch`（默认值）：如果子元素未设置高度或设为auto，将占满整个容器的高度。
- `baseline`: 子元素的第一行文字的基线对齐。

## 6、align-content -- 多行子元素在交叉轴上的对齐方式

```css
align-content: stretch | flex-start | flex-end | center | space-between | space-around;
```

# # Flex元素的属性

## 1、order -- 元素排列顺序,数值越小越靠前

```css
order: <number>;  // 默认0
```

## 2、flex-grow -- 元素放大比例

```css
flex-grow: <number>; // 默认0,即有剩余空间也不放大
```

## 3、flex-shrink -- 元素缩小比例

```css
flex-shrink: <number>; // 默认1,即剩余空间不足,此元素将缩小; 负值无效
```

## 4、flex-basis -- 元素基本空间,据此计算主轴剩余空间

```css
flex-basis: <length>; // 默认auto
```

## 5、flex -- 复合属性

`flex` 属性是 `flex-grow`,  `flex-shrink` 和 `flex-basis`的简写，默认值为 `0 1 auto`。后两个属性可选。

```css
flex: <'flex-grow'> <'flex-shrink'> <'flex-basis'>
```

该属性有两个快捷值：`auto` (`1 1 auto`) 和 none (`0 0 auto`)

## 6、align-self -- 单独元素对齐方式,覆盖align-items属性

```css
align-self: auto | flex-start | flex-end | center | baseline | stretch; // 默认auto
```











































