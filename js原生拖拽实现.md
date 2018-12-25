### JS 拖拽

> 拖拽功能大致由 mousedown、mouseup、mousemove 三种事件来完成。目标元素必须为 absolute 样式，根据监听的鼠标移动坐标，来改变目标元素的坐标值。一般拖拽功能多实现在 Modal 上。以下是 demo，仅供参考！

#### 原生 demo

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Draggable</title>
  <style>
    #drag {
      width: 100px;
      height: 100px;
      background: #ccc;
      border: 1px solid red;
      cursor: move;
      position: absolute;
    }
  </style>
</head>
<body>
  <div id="drag" ></div>
</body>
<script>
  var dragDom = document.getElementById('drag'); // 目标元素，必须为 absolute。
  var dragging; // 是否激活拖拽状态
  var tLeft, tTop; // 鼠标按下时相对于选中元素的位移

  // 目标元素鼠标摁下后
  dragDom.onmousedown = function (e) {
    e.preventDefault();
    dragging = true; // 激活拖拽状态

    /*
    * getBoundingClientRect: 返回一个 DomRect 对象
    *   包含该元素的 top、right、bottom、left 值，对应的是到屏幕上方和左边的距离，单位 px
    * */
    var dragDomRect = dragDom.getBoundingClientRect();
    /*
    * e.clientX、e.clientY
    *   获取鼠标的坐标位置
    * */
    tLeft = e.clientX - dragDomRect.left; // 鼠标按下时和选中元素的坐标偏移:x坐标
    tTop = e.clientY - dragDomRect.top; // 鼠标按下时和选中元素的坐标偏移:y坐标
  };

  //监听鼠标移动事件
  document.addEventListener('mousemove', function(e) {
    e.preventDefault();
    // 当目标元素处于移动激活状态
    if (dragging) {
      var moveX = e.clientX - tLeft,
          moveY = e.clientY - tTop;
      var bodyWidth = window.innerWidth, // 获取浏览器内容宽度
          bodyHeight = window.innerHeight; // 获取浏览器内容高度
      /*
      * 防止元素移动超出左边和上边
      * */
      moveX < 0 ? moveX = 0 : null;
      moveY < 0 ? moveY = 0 : null;
      /*
      * 防止元素移动超出右边和下边
      * dragDom.offsetWidth：获取元素宽度
      * dragDom.offsetHeight：获取元素高度
      * */
      (moveX + dragDom.offsetWidth) > bodyWidth ? moveX = bodyWidth - 100 : null;
      (moveY + dragDom.offsetHeight) > bodyHeight ? moveY = bodyHeight - 100 : null;

      dragDom.style.left = moveX + 'px';
      dragDom.style.top = moveY + 'px';
    }
  });

  // 目标元素鼠标放开事件
  dragDom.onmouseup = function (e) {
    e.preventDefault();
    dragging = false; // 停止移动状态
  };
</script>
</html>
```

#### 封装成独立插件

```
class Drag {
  constructor() {
    this.dragging = false;
    this.tLeft = 0;
    this.tTop = 0;
  }

  // 判断当前传入的 node 是否为一个 html 元素
  create (node) {
    if (node instanceof HTMLElement) {
      return Drag.onMouseDown(node);
    }

    throw `${node} 不是一个 HTML 元素`;
  }

  // 目标元素鼠标摁下后
  static onMouseDown (node) {
    var _this = this;

    node.onmousedown = function (e) {
      e.preventDefault();
      /*
      * getBoundingClientRect: 返回一个 DomRect 对象
      *   包含该元素的 top、right、bottom、left 值，对应的是到屏幕上方和左边的距离，单位 px
      * */
      var dragDomRect = node.getBoundingClientRect();
      /*
      * e.clientX、e.clientY
      *   获取鼠标的坐标位置
      * */
      _this.dragging = true; // 激活拖拽状态
      _this.tLeft = e.clientX - dragDomRect.left; // 鼠标按下时和选中元素的坐标偏移:x坐标
      _this.tTop = e.clientY - dragDomRect.top; // 鼠标按下时和选中元素的坐标偏移:y坐标

      _this.onMouseMove(node);
      _this.onMouseUp(node);
    };
  }

  // 目标元素鼠标放开事件
  static onMouseUp (node) {
    var _this = this;

    node.onmouseup = function (e) {
      e.preventDefault();
      _this.dragging = false; // 停止移动状态
      document.onmousemove = null; // 停止鼠标移动事件
    };
  }

  // 监听鼠标移动事件
  static onMouseMove (node) {
    var _this = this;

    document.onmousemove = function (e) {
      e.preventDefault();
      // 当目标元素处于移动激活状态
      if (_this.dragging) {
        var moveX = e.clientX - _this.tLeft,
          moveY = e.clientY - _this.tTop;
        var bodyWidth = window.innerWidth, // 获取浏览器内容宽度
          bodyHeight = window.innerHeight; // 获取浏览器内容高度
        /*
        * 防止元素移动超出左边和上边
        * */
        moveX < 0 ? moveX = 0 : null;
        moveY < 0 ? moveY = 0 : null;
        /*
        * 防止元素移动超出右边和下边
        * dragDom.offsetWidth：获取元素宽度
        * dragDom.offsetHeight：获取元素高度
        * */
        var offsetWidth = node.offsetWidth,
            offsetHeight = node.offsetHeight;
        (moveX + offsetWidth) > bodyWidth ? moveX = bodyWidth - offsetWidth : null;
        (moveY + offsetHeight) > bodyHeight ? moveY = bodyHeight - offsetHeight : null;

        node.style.left = moveX + 'px';
        node.style.top = moveY + 'px';
      }
    }
  }
}

// 调用
var dom = document.getElementById('drag');
var draggble = new Drag();
draggble.creage(dom);
```

#### React 版 demo

```
import React from 'react';

const style = {
  width: '100px',
  height: '100px',
  background: '#ccc',
  border: '1px solid red',
  cursor: 'move',
  position: 'absolute',
  zIndex: 999,
};

class Drag extends React.Component {
  constructor(props) {
    super(props);

    this.dragDom = null; // 目标元素
    this.dragging = false;
    this.tLeft = 0;
    this.tTop = 0;
  }

  componentDidMount() {
    document.addEventListener('mousemove', (e) => {
      e.preventDefault();
      // 当目标元素处于移动激活状态
      if (this.dragging) {
        const { innerWidth, innerHeight }  = window; // 获取浏览器内容宽度和宽度
        let moveX = e.clientX - this.tLeft;
        let moveY = e.clientY - this.tTop;
        /*
        * 防止元素移动超出左边和上边
        * */
        moveX < 0 ? (moveX = 0) : null;
        moveY < 0 ? moveY = 0 : null;
        /*
        * 防止元素移动超出右边和下边
        * dragDom.offsetWidth：获取元素宽度
        * dragDom.offsetHeight：获取元素高度
        * */
        (moveX + this.dragDom.offsetWidth) > innerWidth ? (moveX = innerWidth - 100) : null;
        (moveY + this.dragDom.offsetHeight) > innerHeight ? (moveY = innerHeight - 100) : null;

        this.dragDom.style.left = moveX + 'px';
        this.dragDom.style.top = moveY + 'px';
      }
    });
  }

  onMouseDown = (e) => {
    e.preventDefault();
    this.dragging = true; // 激活拖拽状态
    this.dragDom = e.target; // 保存目标元素

    /*
    * getBoundingClientRect: 返回一个 DomRect 对象
    *   包含该元素的 top、right、bottom、left 值，对应的是到屏幕上方和左边的距离，单位 px
    * */
    const dragDomRect = e.target.getBoundingClientRect();
    /*
    * e.clientX、e.clientY
    *   获取鼠标的坐标位置
    * */
    this.tLeft = e.clientX - dragDomRect.left; // 鼠标按下时和选中元素的坐标偏移:x坐标
    this.tTop = e.clientY - dragDomRect.top; // 鼠标按下时和选中元素的坐标偏移:y坐标
  };

  onMouseUp = (e) => {
    e.preventDefault();
    this.dragging = false; // 停止移动状态
  };

  render() {
    return <div style={style} onMouseDown={this.onMouseDown} onMouseUp={this.onMouseUp} />;
  }
}

export default Drag;
```