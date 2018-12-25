## window.history

- History 对象包含用户（在浏览器窗口中）访问过的 URL。


### API

#### length

- 声明了浏览器历史列表中的元素数量。

```
console.log(history.length);
==> 1
```

#### back()

- 可加载历史列表中的前一个 URL（如果存在）。
- 应用场景：可用于 h5 应用的【返回】按钮，当其他应用进入 h5 页面时，可返回到之前的应用

```
urlA -> urlB
// 当前在 urlB 时执行 history.back() 时，会加载 urlA 页面。
// 当前在 urlA 时执行，则返回 undefined
```

#### forward()

- 可加载历史列表中的下一个 URL（如果存在）。

```
urlA -> urlB
// 当前在 urlA 时执行 history.forward() 时，会加载 urlB 页面。
// 当前在 urlB 时执行，则返回 undefined
```

#### go(number|URL)

- 可加载历史列表中的某个具体的页面。当前显示页面为
- number: 要访问的 URL 在 History 的 URL 列表中的相对位置。
- URL: 要访问的 url 地址。
	- 非特性属性，目前所知浏览器都不支持。
	- 该方法未能实现需要的效果，如：

	```
	// 浏览器并不会跳转至该页面，只会刷新当前页面
	history.go('http://www.baidu.com');

	// 替代方案
	window.location.href = 'http://www.baidu.com';
	```

----
----

### HTML5 新特性

#### replaceState(state, title, url)
- 替换浏览器中的 url，但是不会将改变后的 url 记录到 history 中，所以点击浏览器后退按钮后，会回退到浏览器的上一个页面，也不会发送任何 request，只是单纯的改变 url。

	```
	// demo
	先加载 www.baidu.com 页面（作者总是喜欢用“百度”做测试，ping 的时候也一样。。。）
	原地址：www.baidu.com
	console：history.replaceState(null, null, 'hello');
	改变后：www.baidu.com/hello
	再次 console后：
	改变后：www.baidu.com/hello/hello

	// demo2
	原地址：www.baidu.com/aaaa/bbb
	console：history.replaceState(null, null, '/hello');
	改变后：www.baidu.com/hello
	```

#### pushState(state, title, url)

- 向浏览器历史添加一个状态。该状态可被浏览器监听并读取。
- 参数：
	- state: 状态对象是一个由 pushState()方法创建的、与历史纪录相关的JS对象。当用户定向到一个新的状态时，会触发 popstate 事件。事件的 state 属性包含了历史纪录对象。state 对象可以是任何可以序列化的东西。
	- title: 现代浏览器都不支持这个参数了，但是考虑到兼容性问题，可以将其设置为 null。
	- url: 向 history 中添加一个历史记录，但是并不会去加载该 url。

	```
	// demo
	原地址：www.baidu.com
	console：history.pushState(null, null, 'hello');
			  history.replaceState(null, null, 'hello');
	改变后：www.baidu.com/hello
	点击浏览器回退按钮：www.baidu.com

	// demo2
	原地址：www.baidu.com
	console：history.pushState(null, null, 'https://www.baidu.com');
	报错：传递的 url 地址和当前地址并非同源，属于跨域行为，被浏览器安全策略拦截。
	```

#### popstate

- 浏览器的【回退】【前进】会触发该事件

```
history.pushState({ demo: 'hello' }, null, 'hello');
window.addEventListener('popstate', function(e) {
	console.log(e.state);
}

// console
{ demo: 'hello' }
```