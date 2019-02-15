# 原生 JS 实现路由功能

> 现在很多 web 单页应用，只需要在第一次网页加载后，就可以自由的切换浏览器 url，并达到切换页面而不重新刷新页面。这相比之前由服务端渲染返回路由的情况，能省事许多。下面我们来看看如何实现！

## hash

- hash 表示 URL 中 “#” 后的字符，通常情况下，我们会将此功能当做网页的锚点进行使用，能使浏览器定位到某个元素位置。
- hash 的改变不会刷新页面，也不会向服务端发送信息。
- window.location.hash 可以获取到值

## onhashchange

- 浏览器提供的当 URL 的 hash 发生变化时的事件。
- window.onhashchange
- 可以使用 window.addEventListener('hashchange', callback) 进行监听

## demo

style

```
<style>
	#content {
	  padding-top: 20px;
	  color: red;
	}
</style>
```

html

```
<div id="container">
  <a href="#/js">js</a>
  <a href="#/css">css</a>
  <a href="#/react">react</a>
  <a href="#/vue">vue</a>
  <div id="content">我是xxx页面</div>
</div>
```

js

```
<script>
  const Router = {
    pages: { // 配置页面路由信息
      '/': '我是主页面',
      '/js': '我是 js 页面',
      '/css': '我是 css 页面',
      '/react': '我是 react 页面',
      '/vue': '我是 vue 页面',
    },
    hashUrl: '',
    // 初始化监听事件
    init: function () {
      const _this = this;
      window.addEventListener('hashchange', function (e) {
        // e.oldURL(改变前的 url) e.newURL(改变后的 url)
        // 获取改变后的 hash 值，且去掉 #
        _this.hashUrl = window.location.hash.substring(1) || '/';
        _this.onLoad();
      })
    },
    // 每次监听事件的回调函数会触发重新渲染
    onLoad: function () {
      const contentDom = document.getElementById('content');
      contentDom.innerHTML = this.pages[this.hashUrl];
    },
  };
  Router.init();
</script>
```