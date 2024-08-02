<a name="df368884"></a>
## 前言
本篇文章总结了浏览器端的二进制以及数据格式的转换方法，整体内容有：

1. 如何得到数据
2. 认识File、Blob
3. 如何对数据处理转换
4. 转换后数据相互之间转换
5. 如何将处理后的数据进行还原

为了更好的理解，总结了一张概括图，如下所示：<br />![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588743914-c2f2ca11-37dd-41d0-a8ec-79ddca48b15f.webp#averageHue=%23fbf8f5&clientId=uae1bbdb7-2c8b-4&from=paste&id=u36ce24aa&originHeight=4362&originWidth=3624&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=ud3a91cdd-f1d7-4641-b898-eddd424a8a3&title=)<br />下面针对每一个部分进行实践操作。
<a name="883583c3"></a>
## 1. 如何得到数据（输入）
通常我们得到`File`和`Blob`对象，总结有这几种方式：

1. 通过input标签
2. 拖拽方式
3. canvas获取
4. 接口返回

如图：<br />![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588743887-fde10673-cebf-4262-b0e1-deafe5526252.webp#averageHue=%23fdf0ee&clientId=uae1bbdb7-2c8b-4&from=paste&id=u03e4ea5f&originHeight=990&originWidth=3408&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u34780b4b-0a69-4163-a8a4-4eeb241a628&title=)
<a name="1.1-input"></a>
### 1.1 input
代码如下：
```html
<input type="file" name="file" id="file">
```
```javascript
const inputNode = document.getElementById('file')
inputNode.addEventListener('change', (e) => {
  const file = e.target.files[0]
})
```
得到File对象：<br />![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588743908-6c8cd73d-fcaa-4e09-87db-75cdddf7a2b3.webp#averageHue=%23f9f8f8&clientId=uae1bbdb7-2c8b-4&from=paste&id=u4a513259&originHeight=286&originWidth=1594&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=uebc09399-ca15-45b9-ab28-903cbbb4281&title=)
<a name="703f8420"></a>
### 1.2 拖拽
代码如下：
```html
<div id="drag"></div>
```
```javascript
const dragNode = document.getElementById("drag");
dragNode.ondragover = (e) => {
  e.preventDefault();
};

dragNode.ondrop = (e) => {
  e.preventDefault();
  const files = e.dataTransfer.files;
  console.log(files);
};
```
结果如下：<br />![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588743880-2b58615d-027d-4907-a0be-23e683ac1c1c.webp#averageHue=%23faf9f9&clientId=uae1bbdb7-2c8b-4&from=paste&id=u19693d0b&originHeight=366&originWidth=1196&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u95f145e3-6c21-4aac-b8e0-dcb7be10436&title=)
<a name="1.3-canvas"></a>
### 1.3 canvas
代码如下：
```html
<canvas id="canvas"></canvas>
```
```javascript
const canvas = document.getElementById("canvas");

canvas.toBlob((blob) => {
  console.log("打印***blob", blob);
});
```
结果如下：<br />![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588744083-4f7861ef-d903-47f1-8fb4-a75791e6b37a.webp#averageHue=%23f9f8f3&clientId=uae1bbdb7-2c8b-4&from=paste&id=u3f815293&originHeight=146&originWidth=750&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u380d56d7-2540-4a87-94ed-4368850f70c&title=)
<a name="b8396637"></a>
### 1.4 接口
接口获取需要设置响应头为`blob`,这里以`axios`和`fetch`为例：

- axios：
```javascript
axios.post(url,{
  // `responseType` 表示服务器响应的数据类型，可以是 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
  responseType: 'blob',
})
```

- fetch
```javascript
fetch(url).then(res=>{
  // 常用的有 json、text、arrayBuffer、blob
  return res.blob()
})
```
<a name="134b14f7"></a>
## 2 认识Blob、File（数据）
![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588744481-83d711ce-2376-4fd9-9670-bbdacc72b9ce.webp#averageHue=%23dfe2e2&clientId=uae1bbdb7-2c8b-4&from=paste&id=ud178e0b1&originHeight=452&originWidth=1982&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=uc08f075b-697b-4c4e-8fa9-a5964972a26&title=)<br />经过输入得到`File`或者`Blob`，那么两者是什么，有什么关系，之间如何转换。
<a name="2.1-blob"></a>
### 2.1 Blob
`Blob` 全称是`binary large object`，即二进制大对象，它是 JavaScript 中的一个对象，表示原始的类似文件的数据。<br />Blob 对象表示一个不可变、原始数据的类文件对象。它的数据可以按文本或二进制的格式进行读取，也可以转换成 [ReadableStream](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FReadableStream) 来用于数据操作。<br />实际上，Blob 对象是包含有只读原始数据的类文件对象。简单来说，Blob 对象就是一个不可修改的二进制文件。
<a name="7182677c"></a>
#### 2.1.1 创建Blob
可以使用 Blob() 构造函数来创建一个 Blob：
```
new Blob(array, options);
```
有两个参数：

- `array`：由 `ArrayBuffer`、`ArrayBufferView`、`Blob`、`DOMString` 等对象构成的，将会被放进 `Blob`；
- `options`：可选的 `BlobPropertyBag` 字典，它可能会指定如下两个属性 
   - `type`：默认值为 ""，表示将会被放入到 `blob` 中的数组内容的 MIME 类型。
   - `endings`：默认值为"`transparent`"，用于指定包含行结束符`\n`的字符串如何被写入，不常用。

常见的 MIME 类型如下：

| 类型 | 描述 |
| --- | --- |
| text/plain | 纯文本文档 |
| text/html | HTML文档 |
| text/javascript | JavaScript文件 |
| text/css | CSS文件 |
| application/json | JSON文件 |
| application/pdf | PDF文件 |
| application/xml | XML文件 |
| image/jpeg | JPEG图像 |
| image/png | PNG图像 |
| image/svg+xml | SVG图像 |
| audio/mpeg | MP3文件 |
| video/mpeg | MP4文件 |

<a name="2.2-file"></a>
### 2.2 File
`File`对象提供有关文件的信息，并允许网页中的 JavaScript 访问其内容。实际上，File 对象是特殊类型的 Blob，且可以用在任意的 Blob 类型的 context 中。Blob 的属性和方法都可以用于 File 对象。<br />注意：File 对象中只存在于浏览器环境中，在 Node.js 环境中不存在。
<a name="ifEy4"></a>
#### 2.2.1 创建File
在 JavaScript 中，主要有两种方法来获取 File 对象：

- `<input>` 元素上选择文件后返回的 FileList 对象；
- 文件拖放操作生成的 `DataTransfer` 对象；
<a name="fccb330a"></a>
### 2.3 总结

1. `File`是一种特殊的`Blob`,只存在于浏览器中，继承`Blob`中的方法和属性。
2. 两者如何转换 
   - File -> Blob：因为File是继承于Blob，不用进行转换
   - Blob -> File：通过`new File([blob],fileName,{type:MIME})`
<a name="1d364ad2"></a>
## 3. 如何对数据处理转换
![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588744568-d18f9834-b82d-410a-8e8e-4cc9955d86cc.webp#averageHue=%23eef7ef&clientId=uae1bbdb7-2c8b-4&from=paste&id=ue9314b9c&originHeight=738&originWidth=1982&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=ua40c7679-cbfd-404d-a1a2-bf5a3fc850e&title=)<br />通过Blob对象可以如何转换成自己想要的数据类型，有两种方式：

- 使用`createObjectURL`,生成`Object URL`
- 使用`FileReader`,生成`DataURL`、`BinaryString`、`Text`、`ArrayBuffer`

下面分别实操下：
<a name="3.1-createobjecturl"></a>
### 3.1 URL.createObjectURL()
[URL](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FURL) 接口的 `**createObjectURL()**`  静态方法创建一个用于表示参数中给出的对象的 URL 的字符串。<br />URL 的生命周期与其创建时所在窗口的 [document](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FDocument) 绑定在一起。新对象 URL 代表指定的 [File](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FFile) 对象或 [Blob](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FBlob) 对象。要释放对象 URL，需调用 [revokeObjectURL()](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FURL%2FrevokeObjectURL_static)。
<a name="yOVEI"></a>
#### 3.1.1 参数：File/Blob
无论是`File`还是`Blob`,可以直接调用`URL.createObjectURL`两者的结果是相同的。<br />使用同一张图片进行转换为例：

1. **File作为参数**

通过input标签获取file对象，再进行转换：
```html
<input type="file" name="file" id="file" />
```
```javascript
const inputNode = document.getElementById("file");
inputNode.addEventListener("change", (e) => {
  const file = e.target.files[0];
  console.log(
    "打印***file",
    URL.createObjectURL(file)
  );
})
```
结果：<br />![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588744610-cbe43fde-f439-4ee1-afe4-1e19a9a57766.webp#averageHue=%23f8fcf8&clientId=uae1bbdb7-2c8b-4&from=paste&id=ud20e9c9c&originHeight=56&originWidth=1122&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u3469bf05-fdb7-4402-b648-e1f48b9260a&title=)<br />输入浏览器预览： ![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588744608-573b5f5a-a935-4eee-994c-01c317b9e041.webp#averageHue=%23363636&clientId=uae1bbdb7-2c8b-4&from=paste&id=ud8b63464&originHeight=940&originWidth=1680&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u10e4b4fa-6c40-425b-a4e1-94a06efd1f6&title=)

2. **Blob作为参数**

使用canvas的`toBlob`方法得到blob，再进行转换
```javascript
<img src="./axios.png" alt="" srcset="" id="img" />
  const cvs = document.createElement("canvas");
const imageNode = document.getElementById("img");
const ctx = cvs.getContext("2d");
cvs.width = imageNode.width;
cvs.height = imageNode.height;
ctx.drawImage(imageNode, 0, 0, cvs.width, cvs.height);
cvs.toBlob((blob) => {
  console.log(
    "打印***blob",
    URL.createObjectURL(blob)
  );
});
```
结果： ![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588744701-bd5f0362-2be3-407d-a93c-0437a6a37975.webp#averageHue=%23fbfef9&clientId=uae1bbdb7-2c8b-4&from=paste&id=u65f672ed&originHeight=60&originWidth=1112&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=uf182a605-863b-422d-b905-11e854ac9db&title=)<br />输入浏览器预览： ![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588744943-6246b06f-b589-472e-b84d-8422f5072ca6.webp#averageHue=%23323232&clientId=uae1bbdb7-2c8b-4&from=paste&id=ub5a0cfc5&originHeight=928&originWidth=1806&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u3d4a607c-914c-442b-9a87-2c88b15868d&title=)
<a name="3.1.1-object-url"></a>
#### 3.1.2 生成对象：Object URL
Object URL又称Blob URL（W3C定义名称），是HTML5中的新标准。它是一个用来表示File Object 或Blob Object 的URL。在网页中，我们可能会看到过这种形式的 `blob: URL`<br />其实Blob URL/Object URL 是一种伪协议，允许将 Blob 和 File 对象用作图像、二进制数据下载链接等的 URL 源。<br />对于 Blob/File 对象，可以使用 URL构造函数的 `createObjectURL()` 方法创建将给出的对象的 URL。这个 URL 对象表示指定的 File 对象或 Blob 对象。我们可以在`<img>`、`<script>` 标签中或者 `<a>` 和 `<link>` 标签的 `href` 属性中使用这个 URL。<br />经典下载代码：
```javascript
// 使用 URL.createObjectURL 创建一个 URL 对象 
const url = URL.createObjectURL(blob); 
// 创建一个 <a> 标签并设置下载属性 
const a = document.createElement('a'); 
a.href = url; 
a.download = 'hello.txt'; // 设置下载的文件名 
// 模拟点击 <a> 标签 
a.style.display = 'none'; 
document.body.appendChild(a); 
a.click(); 
// 移除 <a> 标签并释放 URL 对象 
document.body.removeChild(a); 
URL.revokeObjectURL(url);
```
<a name="3.2-filereader"></a>
### 3.2 new FileReader()
`**FileReader**` 允许异步读取存储在用户计算机上的文件（或原始数据缓冲区）的内容，使用 [File](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FFile) 或 [Blob](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FBlob) 对象指定要读取的文件或数据。<br />![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588745009-351dc67b-b3ab-40e7-b6d3-9b291761d945.webp#averageHue=%23fbf8ec&clientId=uae1bbdb7-2c8b-4&from=paste&id=u93d9c34e&originHeight=450&originWidth=1734&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u54f06ea2-4618-4648-a954-cebed510336&title=)
<a name="3.2.1-data-url"></a>
#### 3.2.1 Data URL
`Data URL`是一种包含数据的URL，其中数据部分通常是使用`Base64`编码的。是一种将小数据嵌入到文档中的方式。它的格式如下：
```
data:[<mediatype>][;base64],<data>
```

- `<mediatype>`：表示数据的媒体类型（MIME 类型），例如 `text/plain` 或 `image/png`。如果省略，默认值是 `text/plain;charset=US-ASCII`。
- `;base64`：表示数据使用 Base64 编码。如果省略，数据部分应为 URL 编码的字符串。
- `<data>`：表示实际的数据部分。
<a name="769110df"></a>
##### 3.2.1.1 与Base64的关系
Base64 是一种基于64个可打印字符来表示二进制数据的表示方法。Base64 编码普遍应用于需要通过被设计为处理文本数据的媒介上储存和传输二进制数据而需要编码该二进制数据的场景。这样是为了保证数据的完整并且不用在传输过程中修改这些数据。<br />在 JavaScript 中，有两个函数被分别用来处理解码和编码 _base64_ 字符串：

- `atob()`：解码，解码一个 Base64 字符串；
- `btoa()`：编码，从一个字符串或者二进制数据编码一个 Base64 字符串。

例如:一个包含纯文本 "Hello, World!"，使用`btoa`转换后得到的`Data URL`可以表示为：
```
data:text/html;base64,SGVsbG8sIFdvcmxkIQ==
```
![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588745079-598d5be8-2792-4a46-9cea-c3a15b0a36ab.webp#averageHue=%23eff0ed&clientId=uae1bbdb7-2c8b-4&from=paste&id=u7a9ee1ac&originHeight=182&originWidth=1270&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=ua0f411b2-a8e3-4db7-8dd8-0ee2a0e9558&title=)<br />总结一下：

- **Data URL** 是一种将数据嵌入到文档中的 URL 格式，其中数据部分通常使用 Base64 编码。
- **Base64** 是一种将二进制数据转换为 ASCII 字符串的编码方式，用于在文本环境中传输二进制数据。
<a name="5d5b3d33"></a>
##### 3.2.1.2 生成Data URL的两种方式

1. **canvas生成DataURL**
```javascript
const dataUrl = canvas.toDataURL();
```
结果如下： ![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588745183-935c01d5-e34f-4e63-8238-96b65fae0740.webp#averageHue=%23f8faf5&clientId=uae1bbdb7-2c8b-4&from=paste&id=u40ab1b0e&originHeight=86&originWidth=1170&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u77e6b711-c76c-4728-8263-06ad83d4191&title=)

2. **FileReader的readAsDataURL**
```
js

 代码解读
复制代码const reader = new FileReader();

// Read as Data URL
reader.readAsDataURL(file);
reader.onload = function (e) {
    console.log("打印***e,reader", e, reader);
}
```
结果如下：<br />![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588745124-70597631-8870-45f4-a780-1b56935cb910.webp#averageHue=%23faf7f7&clientId=uae1bbdb7-2c8b-4&from=paste&id=u92c364ed&originHeight=808&originWidth=1574&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u79e0ed66-ac15-4dfd-b920-d7f12d2bf15&title=)<br />注意：使用e或者reader实例都可获取结果
<a name="3.2.2-text"></a>
#### 3.2.2 Text
```javascript
const reader = new FileReader();

// Read as Data URL  
reader.readAsText(file);
reader.onload = function () {
  console.log("打印***text", reader.result);
}
```
新建txt文件，写入`this is a text`。 结果如下：<br />![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588745323-72e5071b-b340-4932-a330-79cb8e87d3e0.webp#averageHue=%23fffffd&clientId=uae1bbdb7-2c8b-4&from=paste&id=u7f51b64a&originHeight=56&originWidth=474&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u03d731f2-2030-46bb-9141-765f421a0d1&title=)<br />注意：如果直接读取图片或其他文件，乱码<br />![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588745431-e2cb61a5-e54b-4fb9-92ae-11246c2ce06e.webp#averageHue=%23dddddd&clientId=uae1bbdb7-2c8b-4&from=paste&id=u4171d917&originHeight=240&originWidth=1544&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u3a9a3d58-2455-4574-8b47-517a292bf1b&title=)
<a name="3.2.3-binarystring"></a>
#### 3.2.3 BinaryString
```javascript
const reader = new FileReader();

//   Read as Binary String
reader.readAsBinaryString(file);
reader.onload = function () {
  console.log("打印***binaryString", reader.result);
}
```
结果如下： ![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588745513-68450f8d-2ac0-4370-bd64-c8c050dd0ac4.webp#averageHue=%23ebebe9&clientId=uae1bbdb7-2c8b-4&from=paste&id=uaa0fcdf5&originHeight=48&originWidth=604&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u59c870da-5115-4957-9798-798fb813192&title=)
<a name="3.2.4-arraybuffer"></a>
#### 3.2.4 ArrayBuffer
```javascript
const reader = new FileReader();

// Read as ArrayBuffer
reader.readAsArrayBuffer(file);
reader.onload = function () {
  console.log("打印***binaryString", reader.result);
}
```
结果如下：<br />![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588745552-14dadd92-787f-4365-b53c-9afd8f069cce.webp#averageHue=%23f6f6f6&clientId=uae1bbdb7-2c8b-4&from=paste&id=ud2d89d0b&originHeight=336&originWidth=886&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=ub56ce363-670c-430b-8b5e-79fa0cfa74b&title=)
<a name="1cdb2fdc"></a>
## 4. 四种格式相互转换
通过`FileReader`转换的四种数据之间如何相互转换。以下是将 `Data URL`、`ArrayBuffer`、`Binary String` 和 `Text` 之间相互转换的具体实现：
<a name="d6373ebf"></a>
### 4.1 Data URL转换其他格式
<a name="b79d3f6a"></a>
#### 4.1.1 Data URL -> ArrayBuffer
```javascript
function dataURLToArrayBuffer(dataURL) {
  const binaryString = atob(dataURL.split(',')[1]);
  const len = binaryString.length;
  const bytes = new Uint8Array(len);
  for (let i = 0; i < len; i++) {
    bytes[i] = binaryString.charCodeAt(i);
  }
  return bytes.buffer;
}
```
<a name="4fc8805e"></a>
#### 4.1.2 Data URL -> Binary String
```javascript
function dataURLToBinaryString(dataURL) {
  return atob(dataURL.split(',')[1]);
}
```
<a name="7bbdb253"></a>
#### 4.1.3 Data URL -> Text
```javascript
async function dataURLToText(dataURL) {
  const arrayBuffer = dataURLToArrayBuffer(dataURL);
  return arrayBufferToText(arrayBuffer);
}
```
<a name="dc63dbea"></a>
### 4.2 ArrayBuffer转换其他格式
<a name="76b9b10d"></a>
#### 4.2.1 ArrayBuffer -> Data URL
```javascript
function arrayBufferToDataURL(arrayBuffer, mimeType = 'application/octet-stream') {
  const bytes = new Uint8Array(arrayBuffer);
  const binaryString = bytes.reduce((data, byte) => data + String.fromCharCode(byte), '');
  return `data:${mimeType};base64,${btoa(binaryString)}`;
}
```
<a name="8b36cd37"></a>
#### 4.2.2 ArrayBuffer -> Binary String
```javascript
function arrayBufferToBinaryString(arrayBuffer) {
  const bytes = new Uint8Array(arrayBuffer);
  return bytes.reduce((data, byte) => data + String.fromCharCode(byte), '');
}
```
<a name="2ee61e1f"></a>
#### 4.2.3 ArrayBuffer -> Text
```javascript
function arrayBufferToText(arrayBuffer) {
  const decoder = new TextDecoder('utf-8');
  return decoder.decode(new Uint8Array(arrayBuffer));
}
```
<a name="1a827d4b"></a>
### 4.3 Binary String转换其他格式
<a name="5efee8b9"></a>
#### 4.3.1 Binary String -> ArrayBuffer
```javascript
function binaryStringToArrayBuffer(binaryString) {
  const len = binaryString.length;
  const bytes = new Uint8Array(len);
  for (let i = 0; i < len; i++) {
    bytes[i] = binaryString.charCodeAt(i);
  }
  return bytes.buffer;
}
```
<a name="a977faa3"></a>
#### 4.3.2 Binary String -> Data URL
```javascript
function binaryStringToDataURL(binaryString, mimeType = 'application/octet-stream') {
  return `data:${mimeType};base64,${btoa(binaryString)}`;
}
```
<a name="f12c1321"></a>
#### 4.3.3 Binary String -> Text
```javascript
function binaryStringToText(binaryString) {
  return decodeURIComponent(escape(binaryString));
}
```
<a name="bcb6a118"></a>
### 4.4 Text转换其他格式
<a name="9b392f79"></a>
#### 4.4.1 Text -> ArrayBuffer
```javascript
function textToArrayBuffer(text) {
  const encoder = new TextEncoder();
  return encoder.encode(text).buffer;
}
```
<a name="c7f84c71"></a>
#### 4.4.2 Text -> Binary String
```javascript
function textToBinaryString(text) {
  return unescape(encodeURIComponent(text));
}
```
<a name="d1b80271"></a>
#### 4.4.3 Text -> Data URL
```javascript
function textToDataURL(text, mimeType = 'text/plain') {
  const arrayBuffer = textToArrayBuffer(text);
  return arrayBufferToDataURL(arrayBuffer, mimeType);
}
```
通过上述函数，可以实现 `Data URL`、`ArrayBuffer`、`Binary String` 和 `Text` 之间的相互转换。注意在处理不同编码格式和数据类型时，需要根据具体需求调整 `TextDecoder` 和 `TextEncoder` 的编码参数。
<a name="5a5fe069"></a>
## 5. FileReader转换后四种格式转换为blob格式
![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1722588745617-4336c763-afc1-4439-85ab-0bb40cc46d19.webp#averageHue=%23f9f4ea&clientId=uae1bbdb7-2c8b-4&from=paste&id=u4b5c2881&originHeight=738&originWidth=1778&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u68fa6d1a-09f7-476d-9d47-52e391f1e10&title=)<br />要将 `FileReader` 转换后的四种格式（`Data URL`、`ArrayBuffer`、`Binary String` 和 `Text`）转换为 `Blob` 格式，可以使用以下方法：
<a name="67215890"></a>
### 5.1 Data URL -> Blob
```javascript
function dataURLToBlob(dataURL) {
  const byteString = atob(dataURL.split(',')[1]);
  const mimeString = dataURL.split(',')[0].split(':')[1].split(';')[0];
  const ab = new ArrayBuffer(byteString.length);
  const ia = new Uint8Array(ab);
  for (let i = 0; i < byteString.length; i++) {
    ia[i] = byteString.charCodeAt(i);
  }
  return new Blob([ia], { type: mimeString });
}
```
<a name="cebf02b0"></a>
### 5.2 ArrayBuffer -> Blob
```javascript
function arrayBufferToBlob(arrayBuffer, mimeType = 'application/octet-stream') {
  return new Blob([arrayBuffer], { type: mimeType });
}
```
<a name="a1690620"></a>
### 5.3 Binary String -> Blob
```javascript
function binaryStringToBlob(binaryString, mimeType = 'application/octet-stream') {
  const ab = new ArrayBuffer(binaryString.length);
  const ia = new Uint8Array(ab);
  for (let i = 0; i < binaryString.length; i++) {
    ia[i] = binaryString.charCodeAt(i);
  }
  return new Blob([ia], { type: mimeType });
}
```
<a name="31bae0c2"></a>
### 5.4 Text -> Blob
```javascript
function textToBlob(text, mimeType = 'text/plain') {
  return new Blob([text], { type: mimeType });
}
```
具体示例：
```javascript
// Data URL to Blob
const dataURL = 'data:text/plain;base64,SGVsbG8gd29ybGQ=';
const blobFromDataURL = dataURLToBlob(dataURL);

// ArrayBuffer to Blob
const arrayBuffer = new Uint8Array([72, 101, 108, 108, 111]).buffer;
const blobFromArrayBuffer = arrayBufferToBlob(arrayBuffer);

// Binary String to Blob
const binaryString = 'Hello';
const blobFromBinaryString = binaryStringToBlob(binaryString);

// Text to Blob
const text = 'Hello world';
const blobFromText = textToBlob(text);
```
<a name="25f9c7fa"></a>
## 总结
通过上述整理可得，对于从文件到数据的转换可以形成一个完整的闭环。从文件-> 输入-> 数据 -> 转换能够形成一个直观的感受。
