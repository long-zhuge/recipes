## H5 clipborad

#### 前言

- 远古时期，人们如果需要在浏览器里实现用户点击操作后复制内容，则需要在代码中引入功能插件，而且用户浏览器必须安装 flash 才能正常使用。这种方式，在那个年代确实很好用。而现在，由于各浏览器都抛弃了 flash (已死)，导致原先的复制操作方式不太能用，而且效率极低(你要还是用这种方式，就跟大街上有人拿着大哥大打电话是一个样的)。但是别担心，现代浏览器内置了很多很好用的原生 API 可以解决，请看下面的栗子。。。

----

#### clipboardData 对象

- 注意：
	- 该对象目前只在 ie 下原生支持(ie 还是有贡献的，然而。。。。)
	- Chrome/Firefox 可以通过 event 获取该对象
		- window.clipboardData // undefined
		- 需要监听事件来完成，后面会有 demo

- 对象属性

|方法|描述|
|:--|:--|
|clearData|通过 dataTransfer 或 clipboardData 对象从剪贴板删除一种或多种数据格式|
|getData|通过 dataTransfer 或 clipboardData 对象从剪贴板获取指定格式的数据|
|setData|以指定格式给 dataTransfer 或 clipboardData 对象赋予数据|

- demo

```
// 该事件需要用户执行 "ctrl+c" 或者 "command+c" 的操作
// 监听 copy 事件，【copy, paste, cut】
document.addEventListener('copy', function(e){
  // Hello, world! 为动态设置的复制内容
  e.clipboardData.setData('text/plain', 'Hello, world!');
  e.preventDefault();
});

// ctrl+v 或者 command+v
Hello, world!
```

- demo2: 我们也可以把复制操作添加到按钮事件中, 这样就可以自定义交互

```
/*
** html
********/ 
<input id="copy_text" type="text" />
<button id="copy">复制</button>

/*
** js
********/
const inputDom = document.getElementById('copy_text');
const copyDom = document.getElementById('copy');

// 点击事件
copyDom.onclick = function() {
  document.addEventListener('copy', copyEvent);
  // 执行 copy 命令
  document.execCommand('copy');
  // 移除绑定的 copyEvent 事件，如果不移除，则用户无法再使用传统的选中复制功能
  document.removeEventListener('copy', copyEvent);
};

const copyEvent = function(e) {
  // 获取 input 值
  const text = inputDom.value;
  // 将值设置到剪贴板中
  e.clipboardData.setData('text/plain', text);
  e.preventDefault();
}
```
----

#### document.execCommand

- `官方定义`：当一个HTML文档切换到设计模式 designMode时，文档对象暴露 execCommand 方法，该方法允许运行命令来操纵可编辑区域的内容。大多数命令影响文档的选择（粗体，斜体等），而其他命令插入新元素（添加链接）或影响整行（缩进）。当使用contentEditable时，调用 execCommand() 将影响当前活动的可编辑元素。

- `人话`：就是这个方法只能在 input／textarea 里使用
	- 因为只有 input／textarea 有 select() 方法

- demo

```
/*
** html
********/
<button id="copy">复制</button>
<textarea name="" id="copy_textarea" cols="30" rows="10"></textarea>

/*
** js
********/
const copyDom = document.getElementById('copy');
const textareaDom = document.getElementById('copy_textarea');
copyDom.onclick = () => {
  textareaDom.select();

  try {
    const msg = document.execCommand('copy') ? 'success' : 'failed';
    console.log(msg)
  } catch (err) {
    console.log('unable to copy', err)
  }
}

// 当然，你也可以把 textarea 隐藏了或者动态创建的方式来完成复制功能。
```