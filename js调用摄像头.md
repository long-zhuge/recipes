## js 调用设备摄像头

> 移动设备的 WebRTC，即网页即使通信<br/>
> 本文以 chrome(74.0.3729.169) 版本为测试浏览器。

1. 核心 API：navigator.mediaDevices.getUserMedia
2. 插件兼容：Adapter.js
	- [传送门](https://www.npmjs.com/package/adapterjs)
	- 解决浏览器之间的差异，并输出统一的接口。

### HTML

```
<video id="video" style="width: 480px; height: 320px"></video>
<button id="open">打开</button>
<button id="close">关闭</button>
```

### JS

```
let videoStream = null; // 用于存储 media 的 stream 对象
const videoNode = document.getElementById('video');
const openBtn = document.getElementById('open');
const closeBtn = document.getElementById('close');

const startMedia = () => {
	const constraints = {
	  audio: false, // 是否打开语音
	  video: true, // 是否打开视频
	};
	let userMedia = null; // media 对象
	if (navigator.mediaDevices.getUserMedia) {
	  // 最新标准 API
	  userMedia = navigator.mediaDevices.getUserMedia(constraints);
	} else if (navigator.webkitGetUserMedia) {
	  // webkit 内核浏览器
	  userMedia = navigator.webkitGetUserMedia(constraints);
	} else if (navigator.mozGetUserMedia) {
	  // Firefox 浏览器
	  userMedia = navigator.mozGetUserMedia(constraints);
	} else if (navigator.getUserMedia) {
	  // 旧版 API
	  userMedia = navigator.getUserMedia(constraints);
	}
	// 此时 userMedia 是一个 promise 对象
	userMedia.then((stream) => {
	  // 成功后返回的业务逻辑
	  videoNode.srcObject = stream;
	  videoNode.play();
	  videoStream = stream;
	}).catch((error) => {
	  console.error('navigator.getUserMedia error: ', error);
	});
};

// 打开的按钮
openBtn.onclick = () => {
	startMedia();
};

// 关闭的按钮
closeBtn.onclick = () => {
   // tracks 是一个数组，可能包含 audio、video 对象
	const tracks = videoStream.getTracks();
	tracks[0].stop();
};
```

## Adapter.js

> 解决浏览器之间的差异，并输出统一的接口

- [传送门](https://www.npmjs.com/package/adapterjs)
- [cdn](http://cdn.temasys.io/adapterjs/0.15.x/adapter.min.js)

### JS

```
const videoNode = document.getElementById('video');
let videoStream = null;
AdapterJS.webRTCReady(function(isUsingPlugin) {
  // WebRTC API 已准备就绪
  //isUsingPlugin: 使用WebRTC插件时为true，否则为false
  getUserMedia({
    audio: false,
    video: true,
  }, (stream) => {
    videoNode.srcObject = stream;
    videoNode.play();
    videoStream = stream;
  }, (error) => {
    console.error('navigator.getUserMedia error: ', error);
  });
});
```