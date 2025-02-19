在边缘函数中，使用 cookies 进行 A/B 测试控制。

## 示例代码

```typescript
// cookie 名称
const cookieName = 'ABTest';
// cookie 值
const valueA = 'value-a';
const valueB = 'value-b';
// 页面路径
const pathA = '/path-a';
const pathB = '/path-b';

async function handleRequest(request) {
  const urlInfo = new URL(request.url);

  // 请求已经已进入AB测试，则直接返回相应内容
  if (urlInfo.pathname.startsWith(pathA) || urlInfo.pathname.startsWith(pathB)) {
      return fetch(request);
  }

  // 获取当前请求的 cookie
  const cookies = request.getCookies();
  const cookie = cookies.get(cookieName);
  const cookieValue = cookie && cookie.value;

  // 如果 cookie 值为A测试，返回相应内容
  if (cookieValue === valueA) {
      urlInfo.pathname = pathA + urlInfo.pathname;
      return fetch(urlInfo.toString());
  }
  
  // 如果 cookie 值为B测试，返回相应内容
  if (cookieValue === valueB) {
      urlInfo.pathname = pathB + urlInfo.pathname;
      return fetch(urlInfo.toString());
  }

  // 随机分配当前请求走A或B测试
  const testValue = Math.random() < 0.5 ? valueA : valueB;
  const path = testValue === valueA ? pathA : pathB;
  urlInfo.pathname = path + urlInfo.pathname;

  const response = await fetch(urlInfo.toString());

  cookies.set(cookieName, testValue, { path: '/' });

  response.setCookies(cookies);
  return response;
}

addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request));
});
```

## 示例预览

在浏览器地址栏中输入边缘函数触发规则，即可预览到示例效果。

<img src="https://user-images.githubusercontent.com/117053395/207602883-96d3f71f-6087-4fa7-bbdd-c461f1d84776.png" width=609px>

## 相关参考
- [Runtime APIs: Cookies](https://cloud.tencent.com/document/product/1552/83932)
- [Runtime APIs: Response](https://cloud.tencent.com/document/product/1552/81917)
