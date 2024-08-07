# 原生观察者接口

## 1. EventTarget (事件监听器)

`EventTarget` 是一个基础接口，几乎所有的 DOM 元素都继承自它，用于实现事件监听和触发机制，是观察者模式的基础。

```javascript
document.addEventListener('click', (event) => {
  console.log('Document was clicked', event);
});
```

## 2. MutationObserver

[MutationObserver](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FMutationObserver) 接口提供了监视对 DOM 树所做更改的能力。
用于监听DOM对象的变化，通过配置（需要观察什么变动），包括节点属性发生变化、增删改时执行相应的回调。

```javascript
const observer = new MutationObserver((mutations) => {
  mutations.forEach((mutation) => {
    console.log(mutation);
  });
});

const config = { attributes: true, childList: true, subtree: true };
observer.observe(document.body, config);

// 停止观察
// observer.disconnect();
```

## 3. IntersectionObserver

**IntersectionObserver** 接口（从属于 [Intersection Observer API](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FIntersection_Observer_API)）提供了一种异步观察目标元素与其祖先元素或顶级文档[视口](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FGlossary%2FViewport)（viewport）交叉状态的方法。其祖先元素或视口被称为根（root）。
当一个 `IntersectionObserver` 对象被创建时，其被配置为监听根中一段给定比例的可见区域。一旦 `IntersectionObserver` 被创建，则无法更改其配置，所以一个给定的观察者对象只能用来监听可见区域的特定变化值；然而，你可以在同一个观察者对象中配置监听多个目标元素。
实现懒加载、无限滚动等功能时非常有用。

```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      console.log('Element is in viewport');
    } else {
      console.log('Element is out of viewport');
    }
  });
});

const target = document.querySelector('#myElement');
observer.observe(target);

// 停止观察
// observer.unobserve(target);
// observer.disconnect();
```

## 4. ResizeObserver

**ResizeObserver** 接口监视 [Element](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FElement) 内容盒或边框盒或者 [SVGElement](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FSVGElement) 边界尺寸的变化。 用于观察元素尺寸变化。

```javascript
const observer = new ResizeObserver((entries) => {
  entries.forEach((entry) => {
    console.log('Element size changed:', entry.contentRect);
  });
});

const target = document.querySelector('#myElement');
observer.observe(target);

// 停止观察
// observer.unobserve(target);
// observer.disconnect();
```

## 5. PerformanceObserver

**PerformanceObserver()**  构造函数使用给定的观察者 `callback` 生成一个新的 [PerformanceObserver](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FPerformanceObserver) 对象。当通过 [observe()](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FPerformanceObserver%2Fobserve) 方法注册的 [条目类型](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FPerformanceEntry%2FentryType) 的 [性能条目事件](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FPerformanceEntry) 被记录下来时，调用该观察者回调。

```javascript
const observer = new PerformanceObserver((list) => {
  const entries = list.getEntries();
  entries.forEach((entry) => {
    console.log(`Entry name: ${entry.name}`);
    console.log(`Entry type: ${entry.entryType}`);
    console.log(`Start time: ${entry.startTime}`);
    console.log(`Duration: ${entry.duration}`);
  });
});

observer.observe({ type: 'resource', buffered: true });
```

## 6. ReportingObserver

`ReportingObserver` 用于监控浏览器报告的问题，如弃用 API 使用、浏览器干预等。这有助于提前发现和解决潜在问题。

```javascript
const observer = new ReportingObserver((reports) => {
  reports.forEach((report) => {
    console.log('Report type:', report.type);
    console.log('Report URL:', report.url);
    console.log('Report body:', report.body);
  });
}, { buffered: true });

observer.observe();
```

![](https://cdn.nlark.com/yuque/0/2024/webp/43732286/1723020488921-d74f94c2-1a81-47cd-b9e1-fa997c87e831.webp#averageHue=%23fafaf6&clientId=ueba8196f-c514-4&from=paste&id=u0f605234&originHeight=197&originWidth=574&originalType=url&ratio=1.5&rotation=0&showTitle=false&status=done&style=none&taskId=u0d09cd29-4116-4936-a467-8b9450bf205&title=)

## 7. BroadcastChannel

`BroadcastChannel` 接口允许在同一源下不同浏览器上下文（如不同的浏览器窗口、标签页、iframe 等）之间进行简单的消息传递。

```javascript
const channel = new BroadcastChannel('my_channel');
channel.onmessage = (event) => {
  console.log('Received:', event.data);
};

// 在另一个上下文中发送消息
const channel2 = new BroadcastChannel('my_channel');
channel2.postMessage('Hello from another context');
```

## 8. Channel Messaging API

`MessageChannel` 和 `MessagePort` 允许在同一页面内不同浏览器上下文之间传递消息。

```javascript
const channel = new MessageChannel();

channel.port1.onmessage = (event) => {
  console.log('Received:', event.data);
};

channel.port2.postMessage('Hello from port2');
```

## 9. WebSocket

`WebSocket` 提供了在浏览器和服务器之间创建持久连接的接口，以便双向通信。

```javascript
const socket = new WebSocket('ws://localhost:8080');

socket.onopen = () => {
  console.log('WebSocket connection established');
};

socket.onmessage = (event) => {
  console.log('Received:', event.data);
};

socket.send('Hello, Server');
```

## 10. Server-Sent Events (SSE)

`EventSource` 接口用于从服务器接收推送事件。它是一种服务器向浏览器推送消息的单向通信。

```javascript
const eventSource = new EventSource('/events');

eventSource.onmessage = (event) => {
  console.log('Received:', event.data);
};

eventSource.onerror = (error) => {
  console.error('EventSource failed:', error);
};
```
