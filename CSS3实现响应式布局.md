# # CSS3-Media Queries

## 1、自适应视窗

```html
<head>
  	<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no">
</head>
```

  `<meta viewport>` 这个属性是在移动设备上设置原始大小显示和是否缩放的声明，其主要参数设置如下所示：

- width-viewport：viewport 的宽度
- height-viewport：viewport 的高度
- initial-scale：初始的缩放比例
- minimum-scale：允许用户缩放到的最小比例
- maximum-scale：允许用户缩放到的最大比例
- user-scalable：用户是否可以手动缩放

## 2、媒体查询语法

```css
@media 设备名 only(选取条件)|not(选取条件)|and(设备选取条件) {}
```

## 3、引入方式

- 外链样式

```html
<link rel="stylesheet" media="mediaType and|not|only (media feature)" href="styles.css">
```

> 通过设定屏幕的判断条件，调用对应的css文件，该实例多用于页面不同风格的css调用与选取，使用该方法可能需要为一个页面制作多个css文件。

- 内嵌样式

```css
@media mediaType and|not|only (media feature) {
      /* css code... */
}
```

> 上述实例可以出现在外部样式表与内部样式表中。`mediaType` 指定媒体类型，`mediaFeature `指定判断条件。

## 4、相关信息

- not|only|all

| 类型   | 描述                                       |
| ---- | ---------------------------------------- |
| not  | 排除某种指定的媒体类型，换句话来说就是用于排除符合表达式的设备，比如`@media not print and （max-width:1200px）`表示排除宽度小于等于1200px的打印设备; |
| only | 用来指定某种特定的媒体类型，可以用来排除不支持媒体查询的浏览器。 其实only很多时候是用来对那些不支持Media Query但却支持Media Type的设备隐藏样式表的。 其主要有：支持媒体特性（Media Queries）的设备，正常调用样式，此时就当only不存在；对于不支持媒体特性(Media Queries)但又支持媒体类型(Media Type)的设备，就会忽略样式，因为其先读only而不是screen；另外不支持Media Queries的浏览器，不论是否支持only，样式都不会被采用； |
| all  | all：所有设备，如果在Media Query 中没有明确指定Media Tpe，那么其默认值就为all; |

- media type

| 值      | 描述              |
| ------ | --------------- |
| all    | 用于所有多媒体类型设备     |
| print  | 用于打印机           |
| screen | 用于电脑屏幕，平板，智能手机等 |
| speech | 用于屏幕阅读器         |

- 可用设备参数

| 值                       | 描述                                       |
| ----------------------- | ---------------------------------------- |
| aspect-ratio            | 定义输出设备中的页面可见区域宽度与高度的比率                   |
| color                   | 定义输出设备每一组彩色原件的个数。如果不是彩色设备，则值等于0          |
| color-index             | 定义在输出设备的彩色查询表中的条目数。如果没有使用彩色查询表，则值等于0     |
| device-aspect-ratio     | 定义输出设备的屏幕可见宽度与高度的比率。                     |
| device-height           | 定义输出设备的屏幕可见高度。                           |
| device-width            | 定义输出设备的屏幕可见宽度。                           |
| grid                    | 用来查询输出设备是否使用栅格或点阵。                       |
| height                  | 定义输出设备中的页面可见区域高度。                        |
| max-aspect-ratio        | 定义输出设备的屏幕可见宽度与高度的最大比率。                   |
| max-color               | 定义输出设备每一组彩色原件的最大个数。                      |
| max-color-index         | 定义在输出设备的彩色查询表中的最大条目数。                    |
| max-device-aspect-ratio | 定义输出设备的屏幕可见宽度与高度的最大比率。                   |
| max-device-height       | 定义输出设备的屏幕可见的最大高度。                        |
| max-device-width        | 定义输出设备的屏幕最大可见宽度。                         |
| max-height              | 定义输出设备中的页面最大可见区域高度。                      |
| max-monochrome          | 定义在一个单色框架缓冲区中每像素包含的最大单色原件个数。             |
| max-resolution          | 定义设备的最大分辨率。                              |
| max-width               | 定义输出设备中的页面最大可见区域宽度。                      |
| min-aspect-ratio        | 定义输出设备中的页面可见区域宽度与高度的最小比率。                |
| min-color               | 定义输出设备每一组彩色原件的最小个数。                      |
| min-color-index         | 定义在输出设备的彩色查询表中的最小条目数。                    |
| min-device-aspect-ratio | 定义输出设备的屏幕可见宽度与高度的最小比率。                   |
| min-device-width        | 定义输出设备的屏幕最小可见宽度。                         |
| min-device-height       | 定义输出设备的屏幕的最小可见高度。                        |
| min-height              | 定义输出设备中的页面最小可见区域高度。                      |
| min-monochrome          | 定义在一个单色框架缓冲区中每像素包含的最小单色原件个数              |
| min-resolution          | 定义设备的最小分辨率。                              |
| min-width               | 定义输出设备中的页面最小可见区域宽度。                      |
| monochrome              | 定义在一个单色框架缓冲区中每像素包含的单色原件个数。如果不是单色设备，则值等于0 |
| orientation             | 定义输出设备中的页面可见区域高度是否大于或等于宽度。               |
| resolution              | 定义设备的分辨率。如：96dpi, 300dpi, 118dpcm        |
| scan                    | 定义电视类设备的扫描工序。                            |
| width                   | 定义输出设备中的页面可见区域宽度。                        |

- 示例 *

```css
/*1、当设备屏幕小于1180px时会采用该样式*/
@media screen and (max-width:1180px) { css codes }

/* 2、当设备屏幕大于850px时会采用该样式 */
@media screen and (min-width:850px) {  css codes }

/* 3、当设备屏幕大于850px,小于1180px时会采用该样式 */
@media screen and (min-width:850px) and (max-width:1180px) {  css codes }

/* 4、仅当电脑、手机、平板设备屏幕小于1180px时会采用该样式 */
@media only screen and (max-width:1180px) { css codes }
```

## 5、注意事项

有的时候你会发现一个奇怪的问题，就是你的 @media 没有起作用。我们知道`min-width`表示最小即大于等于（`>=`），`max-width` 表示最大即小于等于（`<=`），代码从上往下依次执行，后面重复代码会覆盖之前的代码。正确的适配顺序如下：

> `max-width:num0;`：小于等于，分辨率从大写到小，如果同一选择器在更小分辨率下没有重写则会沿用css中定义的基本样式；

> `(min-width:num1) and (max-width:num2)` :大于等于num1，同时满足小于等于num2；写完 `max-width`则开始写其中间值。num1必须在num0的基础上+1px，以免覆盖之前width<=num0的样式，num2 则不要求必须在 num3 的基础上 -1px (因为后面定义的 `width >= num3` 就算 width 的 num 相等也会根据先后原则覆盖这个样式) 。z

> `min-wdith: num3` 大于等于 分辨率从小写到大 如果同一选择器样式在更大分辨率下没有重写则会沿用之前 `@media` 定义的样式 其次再是 CSS中定义的基本样式










