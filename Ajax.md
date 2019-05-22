# # XMLHttpRequest 发起 Ajax 请求

```javascript
// 1. 创建请求对象
var xhr = new XMLHttpRequest();
// 2. 配置请求
var url = "http://50.103.122.71:8080/api/cities?address=成都";
xhr.timeout = 30000;
xhr.responseType = "json";
xhr.open("GET", url, true);
// 3. 发送请求
xhr.send(null);
// 4. 监听请求
xhr.onload = function() {
    console.log(this.response);
}
```

## 1、如何设置request header? 

在发送 Ajax 请求（实质是一个[HTTP](http://www.tutorialspoint.com/http/http_header_fields.htm)请求）时，我们可能需要设置一些请求头部信息，比如 `content-type`、`connection`、`cookie`、`accept-xxx` 等。xhr 提供了`setRequestHeader`来允许我们修改请求 header。

> ```c
> void setRequestHeader(DOMString header, DOMString value);
> ```

注意点：

- 方法的第一个参数 header 大小写不敏感，即可以写成 *content-type* ，也可以写成*Content-Type*，甚至写成 *content-Type*;
- *Content-Type* 的默认值与具体发送的数据类型有关
- *setRequestHeader* 必须在*open()*方法之后，*send()* 方法之前调用，否则会抛错；
- *setRequestHeader* 可以调用多次，最终的值不会采用覆盖 `override` 的方式，而是采用追加 `append` 的方式。下面是一个示例代码：

```javascript
// 1. 创建请求对象
var xhr = new XMLHttpRequest();
// 2. 配置请求
var url = "http://47.107.149.71:8081/api/my/info";
xhr.responseType = "json";
xhr.open("GET", url, true);
// 3. 设置头部参数
xhr.setRequestHeader("Authorization", "");
xhr.setRequestHeader("content-type", "application/x-www-form-urlencoded");
// 4. 发送请求
xhr.send(null);
// 5. 监听请求
xhr.onload = function() {
    console.log(this.response);
}  
```

## 2、如何获取response header?

xhr 提供了2个用来获取响应头部的方法：

> `DOMString getAllResponseHeaders();`
>
> `DOMString getResponseHeader(DOMString header);`

前者是获取 response 中的所有header 字段，后者只是获取某个指定 header 字段的值。

你需要注意的是：

[W3C在 xhr 标准中的限制](https://www.w3.org/TR/XMLHttpRequest/) 规定客户端无法获取 response 中的 `Set-Cookie`、`Set-Cookie2` 这2个字段，无论是同域还是跨域请求；

[W3C在 cors 标准中对于跨域请求的限制](https://www.w3.org/TR/cors/#access-control-allow-credentials-response-header) 规定对于跨域请求，客户端允许获取的response header字段只限于 

“`simple response header`”

“`Access-Control-Expose-Headers`” 

> a. *simple response header* 包括的 header 字段有：`Cache-Control`, `Content-Language`, `Content-Type`, `Expires`, `Last-Modified`, `Pragma`;
>
> b. *Access-Control-Expose-Headers*：首先得注意是"*Access-Control-Expose-Headers*"进行**跨域请求**时响应头部中的一个字段，对于同域请求，响应头部是没有这个字段的。这个字段中列举的 header 字段就是服务器允许暴露给客户端访问的字段。

所以  *getAllResponseHeaders()*  只能拿到**限制以外**（即被视为`safe`）的header字段，而不是全部字段；而调用 *getResponseHeader(header)* 方法时，`header` 参数必须是**限制以外**的header字段，否则调用就会报`Refused to get unsafe header` 的错误。

## 3、如何设置响应数据类型? 

有些时候我们希望 *xhr.response*  返回的就是我们想要的数据类型。比如：响应返回的数据是纯JSON字符串，但我们期望最终通过 *xhr.response*  拿到的直接就是一个 js 对象，我们该怎么实现呢？我们可以通过 *level 2*  提供的 *xhr.responseType*  属性实现。

`responseType` 可设置的格式如下：

| 值              | `xhr.response` 数据类型 | 说明                             |
| --------------- | ----------------------- | -------------------------------- |
| `""`            | `String` 字符串         | 默认值(在不设置`responseType`时) |
| `"text"`        | `String` 字符串         |                                  |
| `"document"`    | `Document` 对象         | 希望返回 `XML` 格式数据时使用    |
| `"json" `       | `javascript` 对象       | 存在兼容性问题，IE10/IE11不支持  |
| `"blob"`        | `Blob` 对象             |                                  |
| `"arrayBuffer"` | `ArrayBuffer` 对象      |                                  |

## 4、如何获取响应数据 ? 

xhr 提供了3个属性来获取请求返回的数据

- `xhr.response` *
  - 默认值：空字符串 `""`
  - 当请求完成时，此属性才有正确的值
  - 请求未完成时，此属性的值可能是`""`或者 `null`，具体与 *xhr.responseType*有关：当*responseType* 为`""`或`"text"`时，值为`""`；`responseType`为其他值时，值为 `null`
- `xhr.responseText`
  - 默认值为空字符串`""`
  - 只有当 *responseType*  为 `"text"`、`""` 时，xhr 对象上才有此属性，此时调用 *xhr.responseText*，否则抛错
  - 只有当请求成功时，才能拿到正确值。以下2种情况下值都为空字符串`""`：请求未完成、请求失败
- `xhr.responseXML`
  - 默认值为 `null`
  - 只有当 *responseType*  为`"text"`、`""`、`"document"`时，xhr 对象上才有此属性，此时才能调用 *xhr.responseXML*，否则抛错
  - 只有当请求成功且返回数据被正确解析时，才能拿到正确值。以下3种情况下值都为`null`：请求未完成、请求失败、请求成功但返回数据无法被正确解析时

## 5、如何追踪 ajax 请求的当前状态?

在发一个 ajax 请求后，如果想追踪请求当前处于哪种状态，该怎么做呢？

用 `xhr.readyState` 这个属性即可追踪到。这个属性是只读属性，总共有5种可能值，分别对应 xhr 不同阶段。每次 *xhr.readyState* 的值发生变化时，都会触发 *xhr.onreadystatechange* 事件，我们可以在这个事件中进行相关状态判断。

```javascript
xhr.onreadystatechange = function () {
    switch(xhr.readyState){
        case 1://OPENED
            //do something
            break;
        case 2://HEADERS_RECEIVED
            //do something
            break;
        case 3://LOADING
            //do something
            break;
        case 4://DONE
            //do something
            break;
    }
};
```

| 值    | 状态                         | 描述                                       |
| ---- | -------------------------- | ---------------------------------------- |
| `0`  | `UNSENT` (初始状态，未打开)        | 此时`xhr`对象被成功构造，`open()`方法还未被调用           |
| `1`  | `OPENED` (已打开，未发送)         | `open()`方法已被成功调用，`send()`方法还未被调用。注意：只有`xhr`处于`OPENED`状态，才能调用 *xhr.setRequestHeader()* 和*xhr.send()* , 否则会报错 |
| `2`  | `HEADERS_RECEIVED`(已获取响应头) | `send()`方法已经被调用, 响应头和响应状态已经返回            |
| `3`  | `LOADING` (正在下载响应体)        | 响应体(`response entity body`)正在下载中，此状态下通过`xhr.response`可能已经有了响应数据 |
| `4`  | `DONE` (整个数据传输过程结束)        | 整个数据传输过程结束，不管本次请求是成功还是失败                 |

## 6、如何设置请求的超时时间? 

如果请求过了很久还没有成功，为了不会白白占用网络资源，我们一般会主动终止请求。*XMLHttpRequest* 提供了`timeout` 属性来允许设置请求的超时时间。

> `xhr.timeout`
>
> 单位：milliseconds 毫秒
>
> 默认值：`0`，即不设置超时

从**请求开始** 算起，若超过 `timeout` 时间请求还没有结束（包括成功/失败），则会触发 *ontimeout* 事件，主动结束该请求。

那么到底什么时候才算是**请求开始** ？

> *xhr.onloadstart* 事件触发的时候，也就是你调用 *xhr.send()* 方法的时候。
>
> 因为 *xhr.open()* 只是创建了一个连接，但并没有真正开始数据的传输，而*xhr.send()*才是真正开始了数据的传输过程。只有调用了`xhr.send()`，才会触发`xhr.onloadstart` 。

那么什么时候才算是**请求结束** ？

> `xhr.loadend` 事件触发的时候。

另外，还有2个需要注意的坑儿：

> 1. 可以在 `send()`之后再设置此`xhr.timeout`，但计时起始点仍为调用`xhr.send()`方法的时刻。
> 2. 当 xhr为一个 `sync` 同步请求时，`xhr.timeout`必须置为`0`，否则会抛错。原因可以参考本文的如“何发一个同步请求”一节。

## 7、如何发送一个同步请求? 

xhr  默认发的是异步请求，但也支持发同步请求（当然实际开发中应该尽量避免使用）。到底是异步还是同步请求，由 *xhr.open()* 传入的 *async* 参数决定。

> `open(method, url [, async = true [, username = null [, password = null]]])`

- `method`: 请求的方式，如`GET/POST/HEADER`等，这个参数不区分大小写
- `url`: 请求的地址，可以是相对地址如`example.php`，这个**相对**是相对于当前网页的`url`路径；也可以是绝对地址如`http://www.example.com/example.php`
- `async`: 默认值为`true`，即为异步请求，若`async=false`，则为同步请求

W3C 的 xhr标准中关于`open()`方法有这样一段说明：

> Throws an "InvalidAccessError" exception if async is false, the JavaScript global environment is a document environment, and either the timeout attribute is not zero, the withCredentials attribute is true, or the responseType attribute is not the empty string.

从上面一段说明可以知道，当`xhr`为同步请求时，有如下限制：

- `xhr.timeout`必须为`0`
- `xhr.withCredentials`必须为 `false`
- `xhr.responseType`必须为`""`（注意置为`"text"`也不允许）

若上面任何一个限制不满足，都会抛错，而对于异步请求，则没有这些参数设置上的限制。

之前说过页面中应该尽量避免使用`sync`同步请求，为什么呢？
因为我们无法设置请求超时时间（`xhr.timeout`为`0`，即不限时）。在不限制超时的情况下，有可能同步请求一直处于`pending`状态，服务端迟迟不返回响应，这样整个页面就会一直阻塞，无法响应用户的其他交互。

另外，标准中并没有提及同步请求时事件触发的限制，但实际开发中确实会遇到部分应该触发的事件并没有触发的现象。如在 chrome中，当 xhr 为同步请求时，在 *xhr.readyState* 由`2`变成`3`时，并不会触发 *onreadystatechange* 事件，*xhr.upload.onprogress* 和 *xhr.onprogress* 事件也不会触发。

## 8、如何获取上传、下载的进度?

在上传或者下载比较大的文件时，实时显示当前的上传、下载进度是很普遍的产品需求。
我们可以通过`onprogress`事件来实时显示进度，默认情况下这个事件每50ms触发一次。需要注意的是，上传过程和下载过程触发的是不同对象的 *onprogress* 事件：

- 上传触发的是`xhr.upload`对象的 `onprogress`事件
- 下载触发的是`xhr`对象的`onprogress`事件

```javascript
xhr.onprogress        = updateProgress;
xhr.upload.onprogress = updateProgress;
function updateProgress(event) {
    if (event.lengthComputable) {
      var completedPercent = event.loaded / event.total;
    }
 }
```

## 9、可以发送什么类型的数据?

> `void send(data);`

*xhr.send(data)*  的参数data可以是以下几种类型：

- `ArrayBuffer`
- `Blob`
- `Document`
- `DOMString`
- `FormData`
- `null`

如果是 GET/HEAD请求，*send()* 方法一般不传参或传 `null`。不过即使你真传入了参数，参数也最终被忽略，*xhr.send(data)* 中的data会被置为 `null`.

*xhr.send(data)*  中data参数的数据类型会影响请求头部`content-type`的默认值：

- 如果data是 “Document” 类型，同时也是“HTML Document”类型，则 “content-type” 默认值为 *text/html;charset=UTF-8* ;否则为*application/xml;charset=UTF-8*；
- 如果data是 “DOMString” 类型，“content-type”默认值为“text/plain;charset=UTF-8”；
- 如果data是 “FormData” 类型，“content-type”默认值为“multipart/form-data; boundary=[xxx]”
- 如果data是其他类型，则不会设置“content-type”的默认值

当然这些只是`content-type`的默认值，但如果用*xhr.setRequestHeader()*手动设置了`content-type`的值，以上默认值就会被覆盖。

另外需要注意的是，若在断网状态下调用 *xhr.send(data)* 方法，则会抛错：

```javascript
Uncaught NetworkError: Failed to execute 'send' on 'XMLHttpRequest'
```

一旦程序抛出错误，如果不 catch 就无法继续执行后面的代码，所以调用 *xhr.send(data)* 方法时，应该用 `try-catch`捕捉错误。

```javascript
try{
    xhr.send(data)
}catch(e) {
    // doSomething...
};
```

## 10、xhr.withCredentials  与 CORS 什么关系

> 我们都知道，在发同域请求时，浏览器会将`cookie`自动加在`request header`中。但大家是否遇到过这样的场景：在发送跨域请求时，`cookie`并没有自动加在`request header`中。

造成这个问题的原因是：在 CORS 标准中做了规定，默认情况下，浏览器在发送跨域请求时，不能发送任何认证信息（`credentials`）如 “cookies” 和 “HTTP authentication schemes”。除非 “xhr.withCredentials” 为 `true`（xhr 对象有一个属性叫 *withCredentials*，默认值为`false`）。

所以根本原因是 cookies 也是一种认证信息，在跨域请求中，client 端必须手动设置 *xhr.withCredentials=true*，且server端也必须允许request能携带认证信息（即response header中包含*Access-Control-Allow-Credentials:true*），这样浏览器才会自动将cookie加在request header中。

另外，要特别注意一点，一旦跨域request能够携带认证信息，`server` 端一定不能将*Access-Control-Allow-Origin* 设置为`*`，而必须设置为请求页面的域名。

# # XMLHttpRequest 相关事件

## 1、事件分类

1. XMLHttpRequestEventTarget 接口定义了7个事件：
   - `onloadstart`
   - `onprogress`
   - `onabort`
   - `ontimeout`
   - `onerror`
   - `onload`
   - `onloadend`
2. 每一个XMLHttpRequest里面都有一个`upload`属性，而*upload*是一个*XMLHttpRequestUpload*对象
3. XMLHttpRequest和XMLHttpRequestUpload都继承了同一个XMLHttpRequestEventTarget接口，所以xhr和*xhr.upload* 都有第一条列举的7个事件
4. *onreadystatechange* 是XMLHttpRequest独有的事件

所以这么一看就很清晰了：
xhr 一共有8个相关事件：7个XMLHttpRequestEventTarget事件+1个独有的onreadystatechange事件；而`xhr.upload`只有7个XMLHttpRequestEventTarget事件。

## 2、事件触发条件

下面整理了一张xhr相关事件触发条件表，其中最需要注意的是 `onerror` 事件的触发条件。

| 事件                 | 触发条件                                                     |
| -------------------- | ------------------------------------------------------------ |
| `onreadystatechange` | 每当`xhr.readyState`改变时触发；但`xhr.readyState`由非`0`值变为`0`时不触发。 |
| `onloadstart`        | 调用`xhr.send()`方法后立即触发，若`xhr.send()`未被调用则不会触发此事件。 |
| `onprogress` *       | `xhr.upload.onprogress`在上传阶段(即`xhr.send()`之后，`xhr.readystate=2`之前)触发，每50ms触发一次；`xhr.onprogress`在下载阶段（即`xhr.readystate=3`时）触发，每50ms触发一次。 |
| `onload` *           | 当请求成功完成时触发，此时`xhr.readystate=4`                 |
| `onloadend`          | 当请求结束（包括请求成功和请求失败）时触发                   |
| `onabort`            | 当调用`xhr.abort()`后触发                                    |
| `ontimeout` *        | `xhr.timeout`不等于0，由请求开始即`onloadstart`开始算起，当到达`xhr.timeout`所设置时间请求还未结束即`onloadend`，则触发此事件。 |
| `onerror`            | 在请求过程中，若发生`Network error`则会触发此事件（若发生`Network error`时，上传还没有结束，则会先触发`xhr.upload.onerror`，再触发`xhr.onerror`；若发生`Network error`时，上传已经结束，则只会触发`xhr.onerror`）。**注意**，只有发生了网络层级别的异常才会触发此事件，对于应用层级别的异常，如响应返回的`xhr.statusCode`是`4xx`时，并不属于`Network error`，所以不会触发`onerror`事件，而是会触发`onload`事件。 |

## 3、事件触发顺序

当请求一切正常时，相关的事件触发顺序如下：

1. 触发`xhr.onreadystatechange`(之后每次`readyState`变化时，都会触发一次)
2. 触发`xhr.onloadstart`
   //上传阶段开始：
3. 触发`xhr.upload.onloadstart`
4. 触发`xhr.upload.onprogress`
5. 触发`xhr.upload.onload`
6. 触发`xhr.upload.onloadend`
   //上传结束，下载阶段开始：
7. 触发`xhr.onprogress`
8. 触发`xhr.onload`
9. 触发`xhr.onloadend`

> 发生`abort`/`timeout`/`error`异常的处理

在请求的过程中，有可能发生 `abort`/`timeout`/`error`这3种异常。那么一旦发生这些异常，xhr后续会进行哪些处理呢？后续处理如下：

1. 一旦发生`abort`或`timeout`或`error`异常，先立即中止当前请求
2. 将 `readystate` 置为`4`，并触发 `xhr.onreadystatechange`事件
3. 如果上传阶段还没有结束，则依次触发以下事件：
   - `xhr.upload.onprogress`
   - `xhr.upload.[onabort或ontimeout或onerror]`
   - `xhr.upload.onloadend`
4. 触发 `xhr.onprogress`事件
5. 触发 `xhr.[onabort或ontimeout或onerror]`事件
6. 触发`xhr.onloadend` 事件

> 在哪个`xhr`事件中注册成功回调？

从上面介绍的事件中，可以知道若xhr请求成功，就会触发*xhr.onreadystatechange*和*xhr.onload*两个事件。 那么我们到底要将成功回调注册在哪个事件中呢？我倾向于 *xhr.onload* 事件，因为*xhr.onreadystatechange* 是每次 *xhr.readyState* 变化时都会触发，而不是 *xhr.readyState=4* 时才触发。

```javascript
xhr.onload = function () {
    // 如果请求成功
    if(xhr.status == 200){
      // successCallback
    }
  }
```

上面的示例代码是很常见的写法：先判断 `http`状态码是否是`200`，如果是，则认为请求是成功的，接着执行成功回调。这样的判断是有坑儿的，比如当返回的`http`状态码不是`200`，而是`201`时，请求虽然也是成功的，但并没有执行成功回调逻辑。所以更靠谱的判断方法应该是：当`http`状态码为`2xx`或`304`时才认为成功。

```javascript
  xhr.onload = function () {
    // 如果请求成功
    if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
      //successCallback
    }
  }
```

# # jQuery 中的AJAX

## 1、$.get(url, [data], [fn], [type])

## 2、$.getJSON(url, [data], [fn])

## 3、$.getScript(url, [callback])

## 4、$.post(url, [data], [fn], [type])

## 5、$.ajax(url, [settings])





