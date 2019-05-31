## React 生命周期以及API

### 初始化阶段

- getDefaultProps
	- 作用于组件类，只调用一次，返回对象用于设置默认的props，对于引用值，会在实例中共享。
	- this.props是只读的
	- return {name:'long'}
	- 其他接口调用：this.props.name
		
- getInitialState
	- 作用于组件的实例，在实例创建时调用一次，用于初始化每个实例的state，此时可以访问this.props。
	- return {name:'long'}
	- 其他接口调用：this.state.name
		
- componentWillMount
	- 此方法会在完成首次渲染之前被调用。这也是在render方法调用前可以修改组件state的最后一次机会。

- componentDidMount
	- 真实的DOM被渲染出来后调用，在该方法中可通过this.getDOMNode()访问到真实的DOM元素。此时已可以使用其他类库来操作这个DOM。
	- 还可以使用 ref 来获取 dom
	
		```
		<audio loop="loop" hidden ref="audio">
			<source src="./rain.mp4" type="audio/mp4" />
		</audio>
		
		// 调用
		this.refs.audio.play();
		```

- componentDidCatch
	- v16 新特性
	- 可处理错误日志，达到错误边界处理的目的，而不会使整个组件树卸载
	- `问题`：在渲染阶段会捕获错误，但是在运行状态下无法捕获错误。

	```
	import React, { Component } from 'react';
	
	export default class ErrorBoundary extends Component {
	  constructor(props) {
	    super(props);
	    this.state = {
	      hasError: false,
	      errorMessage: '',
	    };
	  }

	  // 如果首次未捕获到错误，将不执行此函数
	  componentDidCatch(err) {
	    const { count } = this.state;
	
	    this.setState({
	      hasError: true,
	      errorMessage: err.message || 'Something went wrong!',
	    });
	  }

	  render() {
	    const { hasError, errorMessage } = this.state;
	
	    if (hasError) {
	      return <div style={{ padding: '20px', color: 'red' }}>ERROR: {errorMessage}</div>;
	    }
	    return this.props.children;
	  }
	}
	
	// 使用
	<ErrorBoundary>
		<SubComponts />
	</ErrorBoundary>
	
	// 当 SubComponts 组件发生错误时，会渲染 ErrorBoundary 组件的 render，而不会让整个页面崩溃。此时能在控制台中查看错误内容，并进行定位处理
	```
		
- render
	- 必选的方法，创建虚拟DOM，该方法具有特殊的规则：
		* 只能通过this.props和this.state访问数据
		* 可以返回null、false或任何React组件
		* 只能出现一个顶级组件（不能返回数组）
		* 不能改变组件的状态
		* 不能修改DOM的输出

### 运行中的状态

- componentWillReceiveProps
	- 组件接收到新的props时调用，并将其作为参数nextProps使用，此时可以更改组件props及state。
	
	```
	componentWillReceiveProps: function(nextProps) {
        if (nextProps.bool) {
            this.setState({
                bool: true
            });
        }
    }
	```

- shouldComponentUpdate
	- 组件是否应当渲染新的props或state，返回false表示跳过后续的生命周期方法，通常不需要使用以避免出现bug。在出现应用的瓶颈时，可通过该方法进行适当的优化。
		
- componentWillUpdate
	- 接收到新的props或者state后，进行渲染之前调用，此时不允许更新props或state。
		
- componentDidUpdate
	- 完成渲染新的props或者state后调用，此时可以访问到新的DOM元素。

### 销毁阶段

- componentWillUnmount
	- 组件被移除之前被调用，可以用于做一些清理工作，在componentDidMount方法中添加的所有任务都需要在该方法中撤销，比如创建的定时器或添加的事件监听器。

----

### this.props和this.state的区别

#### this.state

	1. state的作用
		- 它是React组件中的一个对象，React把用户界面当成状态机，当改变state值时，会重新渲染用户界面。
	2. 工作原理
		- 常用的通知React数据变化的方法是调用 setState(data,callback)。这个方法会将data值合并到this.state，然后重新渲染组件。组件渲染结束后，调用回调函数callback。但是一般不写callback。
		- 比如：this.setState({name:xxx})

#### this.props

	1. props的作用
		- 组件中的props是父级向子级传递数据的方式
		- 一般应用于多个组件时的情况
		 
#### 特殊函数使用方法

    1. dangerouslySetInnerHTML={{ __html: xxxxx }}
        - react默认以字符串形式输出，该方法可以将字符串中的标签，转义字符等以正确的方式展示到页面上

### state 的设置和获取

```
state = {
	name: 0,
};
this.setState({
	name: 1,
})
console.log(this.state.name) // 0

// 由于 set 是异步的，所以需要使用回调函数来进行获取
this.setState({
	name: 1
}, () => {
	console.log(this.state.name);
});
// 1
```