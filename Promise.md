## 异步编程

Javascript异步编程的有直接回调、事件监听、promise、Generator、async函数等

#### 回调函数
- 异步编程最基本的方法。
- 假定有两个函数 f1 和 f2 ，后者等待前者的执行结果。

```
f1();
f2();
```

如果 f1 是一个很耗时的任务，可以考虑改写 f1，把 f2 写成 f1 的回调函数。

```
function f1(callback){
　　setTimeout(function () {
　　　　// f1的任务代码
　　　　callback();
　　}, 0);
}

f1(f2);
```

- 采用这种方式，我们把同步操作变成了异步操作，f1 不会堵塞程序运行，相当于先执行程序的主要逻辑，将耗时的操作推迟执行。
- 优点: 简单、容易理解和部署
- 缺点: 不利于代码的阅读和维护，各个部分之间高度耦合,只能对应一个回调函数，在很多场景中成为一个限制。

#### 事件监听

- 事件驱动模式，任务的执行不取决于代码的顺序，而取决于某个事件是否发生,在 jQuery 等类库中非常常见。

```
const img = document.querySelect(#id);
img.addEventListener('load', () => {
	// 图片加载完成
    ......	
});
img.addEventListener('error', () => {
　　// 出问题了
　　......
});
```

- 优点:是比较容易理解，不再局限于一个回调函数。
- 缺点:是整个程序都要变成事件驱动型，运行流程会变得很不清晰

#### Promise

抽象异步处理对象以及对其进行各种操作的组件，比传统的解决方案‘回调函数和事件’更合理和更强大

- 优点: 解决回调问题，原本的多层级的嵌套代码，变成了链式调用，代码更清晰，减少嵌套数
- 缺点: 无法取消 Promise，一旦新建它就会立即执行，无法中途取消。如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部

#### Promise基本用法

Promise 有两种状态改变的方式，既可以从 Pending 转变为 Fulfilled，也可以从 Pending 转变为 Rejected。一旦状态改变，会一直保持这个状态，不会再发生变化。

- Promise 对象代表一个未完成、但预计将来会完成的操作。它有以下三种状态：
	- Pending: 进行中
	- Resolved: 已完成，又称 Fulfilled
	- Rejected: 已失败

```
const promise = new Promise((resolve, reject) => {
    if (/* 异步操作成功 */) {
        resolve(data);
    } else {
        /* 异步操作失败 */
        reject(error);
    }
});

promise.then((data) => {
  	// do something when success
}, (error) => {
  	// do something when failure
});
```

#### 基本API

- Promise.prototype.then()
	- Promise.prototype.then(onFulfilled, onRejected),对 promise 添加 onFulfilled 和 onRejected 回调，并返回的是一个新的 Promise 实例,因此可以采用链式写法，即 then 方法后面再调用另一个 then 方法

```
getJSON("/posts.json").then((json) => {
  	return json.post;
}).then((post) => {
  	// ...
});
```

then 函数的第一个回调会立即插入 microtask 队列，异步立即执行,第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。


```
setTimeout(()=>{
	console.log(3);
});
Promise.resolve().then(()=>{
	console.log(2);
});
console.log(1);

//1 
//2
//3
```

如果传递一个非函数（比如一个 promise ）值的时候它实际上会解释为 then(null)，具有穿透行为


```
Promise.resolve('foo').then(Promise.resolve('bar')).then( result => {
    console.log(result);
});

//foo
```


- Promise.prototype.catch() 
	- 等同于 promise.then(undefined, onRejected)，如果 Promise 状态已经变成 Resolved，再抛出错误是无效的。
 
```
// bad
promise.then( data => {
    // success
}, (error) => {
    // error
});

// good
promise.then( data => {
    // success
})
  .catch( error => {
    // error
  });

```

第二种写法要好于第一种写法，理由是第二种写法可以捕获前面 then 方法执行中的错误，也更接近同步的写法（try/catch）。因此，建议总是使用 catch 方法。catch 方法返回的还是一个 Promise 对象，因此后面还可以接着调用 then 方法。

- Promise.all()
	- Promise.all方法用于将多个Promise实例，包装成一个新的Promise实例。
	- 在接收到的所有的对象promise都变为 FulFilled 或者 一个变成Rejected 状态之后才会继续进行后面的处理
	
```
const p = Promise.all([p1, p2, p3]);
```

- Promise.race()
	 - 只要有一个promise对象进入 FulFilled 或者 Rejected 状态的话，就会继续进行后面的处理。
	
- Promise.resolve()
	- 现有对象转为 Promise 对象，Promise.resolve 方法就起到这个作用
	
```
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
```

- Promise.resolve 方法的参数分成四种情况
	1. Promise实例: 不做处理，直接返回
	2. thenable对象: 对象转为 Promise 对象，然后就立即执行 thenable 对象的 then 方法
	3. 原始值: 或者对象没有 then 方法，返回一个新的 Promise 对象，状态为 Resolved
	4. 不带有任何参数:返回一个 Resolved 状态的 Promise 对象

- Promise.reject()
	- Promise.reject() 方法的参数，会原封不动地作为 reject 的理由，变成后续方法的参数

## 基础应用

#### Ajax
```
// 组件， getJson.jsx
export default function getJson (url) {
	const req = new XMLHttpRequest();
	req.open('GET', url, true);
	req.onload = () => {
		if (req.status === 200) {
            resolve(req.responseText);
        } else {
            reject(new Error(req.statusText));
        }
	}
	req.onerror = () => {
        reject(new Error(req.statusText));
    };
    req.send();
}
	
// 运行示例
import getJson from './getJson';
	
const URL = "http://httpbin.org/get";
	
getJson(URL).then(function onFulfilled(value){
    console.log(value);
}).catch(function onRejected(error){
    console.error(error);
});
```

#### 队列延迟
```
export default function delay(time) {
  const timerId = `delay-${new Date().getTime()}`;
  // eslint-disable-line
  console.time(timerId);
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve();
      // eslint-disable-line
      console.timeEnd(timerId);
    }, time);
  });
}

// demo
// Generator 中应用
yield delay(1000)

// 普通调用
delay(1000).then(() => console.log('success'));
```

#### fetch 中的超时设置
```
// 弊端：超时后，有可能还是会获取到服务端的 response
function fetchTimeOut(fetch, timeout) {
  return Promise.race([
    fetch,
    new Promise((resolve, reject) => {
      setTimeout(() => reject(new Error('请求超时')), timeout);
    }),
  ]);
}

// demo
fetchTimeOut(fetch(url, ops), time).then().catch()
```




