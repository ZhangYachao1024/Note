# # 栅格系统

|        | 超小屏幕 | 小屏幕   | 中等屏幕 | 大屏幕   | 超大屏幕  |
| ------ | -------- | -------- | -------- | -------- | --------- |
| 条件   | <576px   | \>=576px | \>=768px | \>=992px | \>=1200px |
| 类前缀 | .col-    | .col-sm- | .col-md- | .col-lg- | .col-xl-  |

# # 排版

## 1、标题

-  h1~ h6
-  .h1~.h6：为内联元素提供标题样式
-  .small/\<small>：子标题

## 2. lead

通过 .lead 可以让段落突出显示

```html
<p class="lead">Vivamus sagittis lacus vel augue laoreet rutrum faucibus dolor auctor. Duis mollis, est non commodo luctus.</p>
```

## 3、内联文本元素

- \<mark>：标记
- \<del>：被删除的文本
- \<s>：无用文本
- \<ins>：额外插入的文本
- \< u>：下划线
- \<small>：小号文本
- \<strong>：着重
- \<em>：斜体

## 4、对齐

- .text-left：居左对齐
- .text-center：居中对齐
- .text-right：居右对齐
- .text-nowrap：不换行

## 5、改变大小写

- .text-lowercase：小写
- .text-uppercase：大写
- .text-capitalize：首字母大写

## 6、引用

```html
<blockquote>
    岱宗夫如何？齐鲁青未了。
    <footer class="blockquote-footer">
        Someone famous in 
        <cite title="Source Title">
            Source Title
        </cite>
    </footer>
</blockquote>
```

## 7、列表

- .list-unstyled：无样式列表
- .list-inline：内联列表
- .list-inline-item：内联列表成员

```html
<dl class="row">
    <dt class="col-1">HTML</dt>
    <dd class="col-11">结构层：超文本标记怨言。</dd>

    <dt class="col-1">CSS</dt>
    <dd class="col-11">渲染层：层叠样式表</dd>

    <dt class="col-1">JavaScript</dt>
    <dd class="col-11">行为层：轻量级脚本语言</dd>
</dl>
```

# # 代码

## 1、内联代码

- \<code>：内联代码

## 2、用户输入

- \<kbd>：键盘输入的内容

## 3、代码块

- \<pre>：代码块
- .pre-scrollable：设置 max-height 为 350px ，并在垂直方向展示滚动条。

## 4、程序输入

- \<samp>：标记程序输出内容

# # 表格

- .table：添加padding 并设置水平方向的分割线
- .table-dark：黑色主题
- .thead-dark：头主题(黑色)
- .thead-light：头主题(高亮)
- .table-striped：斑马线
- .table-bordered：边框
- .table-hover：响应鼠标悬浮在行上
- .table-condensed：让表格更加紧凑
- .table-responsive：响应式表格（在表格外嵌套一层容器指定该class）
- .active：鼠标悬停在行或单元格上时所设置的颜色
- .success：标识成功或积极的动作
- .info：标识普通的提示信息或动作
- .warning：标识警告或需要用户注意
- .danger：标识危险或潜在的带来负面影响的动作

# # 表单

- .form-group：表单组（将表单元素放置在表单组中可以实现更好的排列）
- .form-control：\<input>、\<textarea>、\<select> 宽度设置为 100%，结合 \<label> 放置在 `.form-group` 中可以获得更好的排列。
- .form-inline：可使表单内容左对齐并且表现为 `inline-block` 级别的控件。只适用于视口（viewport）至少在 768px 宽度时（视口宽度再小的话就会使表单折叠）。
- .input-group：如果要为输入框添加修饰元素，需要为输入框添加该 class 进行包裹。
- .input-group-addon：在输入框左右添加修饰元素
- .form-horizontal：结合栅格类，可以将 `label` 标签和控件组水平并排布局。这样做将改变 `.form-group` 的行为，使其表现为栅格系统中的行（row），因此就无需再额外添加 `.row` 了。
- .checkbox：多选框
- .disabled：禁用
- .checkbox-inline：内联多选
- .radio-inline：内联单选

## 1、静态控件

- .form-control-static：如果需要在表单中将一行纯文本和 `label` 元素放置于同一行，为 `<p>` 元素添加 `.form-control-static` 类即可。

## 2、Help text

- .help-block：结合表单使用时需在input上通过 `aria-describedby` 属性绑定 `.help-block` 元素的id值。

# # 按钮

- .btn：应用按钮样式

> 提示：链接 \<a> 被作为按钮时，务必为其设置 `role="button"` 属性。尽可能使用 \<button> 标签设置按钮。

## 1、预定义样式

- .btn-default：默认样式
- .btn-primary：首选项
- .btn-success：成功
- .btn-info：一般信息
- .btn-warning：警告
- .btn-danger：危险
- .btn-link：链接

## 2、尺寸

- .btn-lg：大按钮
- .btn-sm：小按钮
- .btn-xs：超小按钮
- .btn-block：将按钮拉伸至父元素100%的宽度，而且按钮也变为了块级（block）元素。

## 3、激活状态

- .active

## 4、禁用状态

- .disabled

# # 图片

## 1、响应式图片

- .img-responsive：让图片支持响应式
- .center-block：让响应式图片水平居中

## 2、图片形状

- .img-rounded：圆角
- .img-corcle：圆形
- .img-thumbnail：相册

# # 辅助类

## 1、情景文本颜色

- .text-muted：柔和
- .text-primary：首选项
- .text-succedd：成功
- .text-info：信息
- .text-warning：警告
- .text-danger：危险

## 2、情景背景颜色

- .bg-primary
- .bg-success
- .bg-info
- .bg-warning
- .bg-danger

## 3、关闭按钮

- .close

```html
<button type="button" class="close" aria-label="Close">
    <span aria-hidden="true">&times;</span>
</button>
```

## 4、三角符号

- .caret

## 5、快速浮动

- .pull-left：向左浮动
- .pull-right：向右浮动

## 6、让内容块居中

- .center-block

## 7、清除浮动

- .clearfix

## 8、显示或隐藏内容

- .show：显示内容
- .hidden：隐藏内容
- .sr-only：隐藏内容

# # 响应式工具

为了加快对移动设备友好的页面开发工作，利用媒体查询功能并使用这些工具类可以方便的针对不同设备展示或隐藏页面内容。另外还包含了针对打印机显示或隐藏内容的工具类。

有针对性的使用这类工具类，从而避免为同一个网站创建完全不同的版本。相反，通过使用这些工具类可以在不同设备上提供不同的展现形式。

## 1、可用的类

通过单独或联合使用以下列出的类，可以针对不同屏幕尺寸隐藏或显示页面内容。

| 类组                | CSS `display`            |
| ------------------- | ------------------------ |
| `.d-*-block`        | `display: block;`        |
| `.d-*-inline`       | `display: inline;`       |
| `.d-*-inline-block` | `display: inline-block;` |
| `.d-*-none`         | `display: none;`         |

以小屏幕（`sm`）为例，可用的 `.d-*-*` 类是：`.d-sm-block`、`.d-sm-inline`和 `.d-sm-inline-block`。









  