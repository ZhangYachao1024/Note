## 使用`all: unset;`重置所有属性

```css
button {
  all: unset;
}
```

## 使用 `:not()` 选择器来决定表单是否显示边框

```css
.nav li:not(:last-child) {
  border-right: 1px solid #666;
}
```

## 水平垂直居中任何元素

```css
/* grid + place-items */
div {
    diplay: grid;
    place-items: center;
}

/* flex + margin */
.parent {
    display: flex;
}
.child {
    margin: auto;
}

/* 绝对定位 + insert */
.parent {
    display: relative;
}
.child {
    position: absolute;
    insert: 0;
}
```

## 使用`marker`伪元素改变列表项样式

```css
li::marker { color: #f00; }
```

## 使用 SVG 图标

```css
.logo {  background: url("logo.svg");}
```

## 使用 `:empty` 隐藏空 HTML 元素

```css
:empty {
  display: none;
}
```

## 使用`inline` `block` 设置边距

```css
div {
    margin-inlne: 10px; // = margin-left: 10px;margin-right: 10px;
    margin-block: 10px; // = margin-top: 10px;margin-bottom: 10px;
    padding-inline: 10px;
    padding-block: 10px;
}
```

## 使用`Object-fit` 设置图片位置

```css
.image {
    object-fit: cover; // cover | scale-down: 保持纵横比 | 缩小
    aspect-ratio: 1; // 纵横比
    max-block-size: 250px; // image size
}
```

## 下划线样式设置

```css
a {
    text-decoration-color: red; // 下划线颜色
    text-decoration-thickness: 2px; // 下划线粗细
    text-underline-offset: .25em; // 下划线偏移文字的距离
}
```

## 轮廓设置

```css
div {
    // 轮廓参与盒子大小计算，类似box-shadow
    outline: 1px solid #333;
    outline-offset: 5px;
    border-radius: 2px;
}
```

## 使用`width:fit-content`调整元素大小

```css
div {
    // 相比于display: inline-block，它的优势是不会更改元素在布局中的位置
    // 比如给div设置width: fit-content，它的display仍然是block
    width: fit-content;
}
```

## 改变浏览器默认滚动行为

```css
div {
    // 滚动只限于当前盒子，滚动条到末尾后，会停止滚动。不会继续触发父容器滚动
    overscroll-behavior: contain;
}
```
