## fetch 封装

> 让 fetch 像 jquery 一样被调用。

- 可以直接调用
- 配合 async 进行使用

### API

|参数|说明|类型|默认值|
|:--|:--|:--|:--|
|url|请求路径|string|无|
|method|请求方式|string|GET|
|headers|请求头信息，非必须|object|无|
|body|入参，如果是 POST 请求等，需要使用|object|无|
|onStart|请求发生前触发|fun|无|
|onSuccess|请求成功后触发|fun|无|
|onError|请求失败后触发|fun|无|

### request.js

```
function checkStatus(response) {
  if (response.status >= 200 && response.status < 300) {
    return response;
  }
  const error = new Error(response.statusText);
  error.response = response;

  throw error;
}

export default function request({ url, method = 'GET', headers, body, onStart, onSuccess, onError }) {
  const newOptions = { credentials: 'include', method, body, headers };
  const methodArr = ['POST', 'PUT'];
  if (methodArr.indexOf(method) > -1) {
    newOptions.headers = {
      Accept: 'application/json',
      'Content-Type': 'application/json; charset=utf-8',
      ...newOptions.headers,
    };
    newOptions.body = JSON.stringify(newOptions.body);
  }

  // 发送请求前触发
  onStart && onStart();

  // 发送请求
  return fetch(url, newOptions)
    .then(checkStatus)
    .then(response => response.json())
    .then((data) => {
      // 当存在回调函数时，表示是独立组件在调用
      if (onStart || onSuccess || onError) {
      	 // todo：修改为自己业务返回成功的方法
        if (data.success) return onSuccess && onSuccess(data);
        // todo: 必要的错误提示信息，如 alert(data.errorMsg)
        return onError && onError(data);
      }
      // 都不存在，适合 services 的请求方式
      if (data && !data.success) {
        // todo: 不要的错误提示信息，如 alert(data.errorMsg)
      }
      return data;
    })
    .catch((error) => {
      if ('stack' in error && 'message' in error) {
        // todo：可以将下面的提示信息替换为自己的方法
        console.error(error.message || `请求错误: ${url}`);
      }

      return error;
    });
}
```

### 直接调用

```
request({
  url: 'submit.json',
  method: 'POST',
  body: { id: 1 },
  onStart() {},
  onSuccess(data) {
    console.log(data);
  },
  onError(data) {
    console.log(data);
  },
});
```

### 框架形式调用（如 dva）

server.js

```
import request from './request';

export async function submit(params) {
  return request({
    url: 'submit.json',
    method: 'POST',
    body: params,
  });
}
```

models.js

```
import { submit } from './server';

{
  ...
  effects: {
    *fetchSubmit({ payload }, { call, put }) {
      const res = yield call(submit, payload);
      if (res && res.success) {
        console.log(res);
      }
    },
  }
}
```