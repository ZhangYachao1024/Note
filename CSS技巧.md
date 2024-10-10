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
