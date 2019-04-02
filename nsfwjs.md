## nsfwjs for React（客户端图片鉴黄）

> 一个简单的javascript库，帮助您快速识别不合适的图像。所有的这些都是在客户端上进行。本秘籍将以 react 为框架来搭建使用。如果你还不熟悉 react，请自行学习，本秘籍不将讲述 react，或者您也可以直接看代码进行学习并在自己的框架上进行尝试。

- [github](https://github.com/infinitered/nsfwjs)
- [npm](https://www.npmjs.com/package/nsfwjs)

### 图像等级

- Drawing - 安全的
- Hentai - 色情的
- Neutral - 中性的
- Porn - 色情图片，性图片
- Sexy - 色情图片，但不是色情作品

### demo 搭建

1. create-react-app
	- 确保本机全局已安装了 react-app
	- create-react-app myApp
2. 将 github 项目上的 example/public/model 目录复制到本地 react 项目的 public 文件夹下。
	- model：这些文件是客户端图片鉴黄的基础文件和配置
3. npm install --save @tensorflow/tfjs nsfwjs
5. 修改 App.js 和 App.css

	App.js
	
	```
	/*
	* 图片鉴黄
	* 逻辑：
	*   0. 初始化时读取 public/model 中的 AI 数据
	*   1. 先将图片转化为 base64 码，在渲染出来之后，再进行下一步
	*   2. 将 img 节点导入到 nsfw model 中进行检测
	* */
	
	import React from 'react';
	import * as nsfwjs from 'nsfwjs';
	import './index.css';
	
	class App extends React.PureComponent {
	  state = {
	    nsfwModel: null,
	    base64: null,
	    predictions: null,
	    message: 'Ready to Classify',
	  };
	
	  componentDidMount() {
	    nsfwjs.load('/model/').then((model) => {
	      this.setState({
	        nsfwModel: model,
	      });
	    });
	  }
	
	  getPredictions = () => {
	    // 使用定时器获取 img DOM，此时获取到的 dom 为拥有 base64 的图片节点
	    setTimeout(() => {
	      const { nsfwModel } = this.state;
	      const img = document.getElementById('img');
	
	      nsfwModel.classify(img).then((predictions) => {
	        this.setState({
	          predictions,
	          message: `Identified as ${predictions[0].className}`,
	        });
	      });
	    }, 60);
	  };
	
	  onChange = (e) => {
	    this.reset();
	    const that = this;
	    const file = e.currentTarget.files[0];
	    const reader = new FileReader();
	
	    // eslint-disable-next-line
	    reader.onload = (file) => {
	      that.setState({
	        base64: file.target.result,
	      }, this.getPredictions);
	    };
	    reader.readAsDataURL(file);
	  };
	
	  reset = () => {
	    this.setState({
	      base64: null,
	      predictions: null,
	    });
	  };
	
	  render() {
	    const { base64, predictions, message } = this.state;
	
	    return (
	      <React.Fragment>
	        <div className="fileBox">
	          <img id="img" className="img" src={base64} alt="" />
	          <label className="fileBtn" htmlFor="file" />
	        </div>
	        <input
	          id="file"
	          type="file"
	          className="hidden"
	          onChange={this.onChange}
	          accept="image/gif, image/jpeg, image/png,"
	        />
	        <div className="content">
	          <div className="content_title">{message}</div>
	          {predictions && predictions.map(item => (
	            <div className="item" key={item.className}>
	              {`${item.className} - ${(item.probability * 100).toFixed(2)}%`}
	            </div>
	          ))}
	        </div>
	      </React.Fragment>
	    );
	  }
	}
	
	export default App;
	```
	
	App.css 添加样式
	
	```
	.hidden {
	  display: none;
	}
	
	.fileBox {
	  position: relative;
	  margin: 0 auto;
	  width: 400px;
	  min-height: 400px;
	  border: 5px dashed #000;
	}
	
	.fileBtn {
	  top: 0;
	  left: 0;
	  position: absolute;
	  width: 100%;
	  height: 100%;
	  cursor: pointer;
	  z-index: 9;
	}
	
	.img {
	  width: 100%;
	}
	
	.content {
	  width: 350px;
	  margin: 0 auto;
	  padding-top: 30px;
	  text-align: center;
	}
	
	.content_title {
	  border-bottom: 2px solid #000;
	  font-size: 30px;
	  font-weight: bold;
	}
	
	.item {
	  color: #e79f23;
	  font-size: 25px;
	}
	```
5. npm start
	- 此时如果浏览器报错：tf.loadModel is not function 时，请将依赖 @tensorflow/tfjs 的版本调整至 0.15.1。

### 注意

> 请在 nsfwjs.load('/model/') 完全读取完成后才能进行图片分析，否则将会报错。所以建议在加载期间进行 loading 处理。本秘籍并没有做此功能，请自行添加。