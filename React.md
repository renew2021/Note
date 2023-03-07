## React的特性
- 声明式设计——React采用声明方式，可以轻松描述应用
- 高效——React通过对DOM的模拟(虚拟DOM)，最大限度地减少与DOM的交互
- 灵活——React可以与已知的库或框架很好的配合
- JSX——JSX是JavaScript语法的扩展
- 组件——通过React构建组件，使代码更加容易得到复用，能够很好的应用在大型项目的开发中
- 单向响应的数据流——React实现了单向响应的数据流，从而减少了重复代码，这也是它为什么比传统数据绑定更加简单
### 虚拟DOM
- 传统DOM更新
	- 真实页面对应一个DOM树。在传统页面的开发模式中，每次需要更新页面时，都要手动操作DOM来进行更新
- 虚拟DOM
	- 在开发中，性能消耗最大的就是DOM操作，而且这部分代码会让整体项目的代码变得难以维护。React把真实DOM树转换成JavaScript对象树，也就是virtual DOM

### JSX语法
JSX将HTML语法直接加入到JavaScript代码中，再通过翻译器转换到纯JavaScript后由浏览器执行。在实际开发中，JSX在产品打包阶段就已经翻译成纯JavaScript，不会带来任何副作用，反而会让代码更加直观并易于维护。编译过程由Babel的JSX编译器实现。
https://reactjs.org/docs/hello-world.html

**所谓的JSX其实就是JavaScript对象，所以使用React和JSX的时候一定要经过编译的过程**
>JSX一使用React构造组件，babel进行编译 --> JavaScript对象 --> ReactDOM.render() --> DOM元素 --> 插入页面

代码示例：
```jsx
//JSX语法
ReactDOM.render(<div id="aaa" class="bbb">1111</div>,document.getElementById("root"))

//经过babel编译后变成下面的代码

ReactDOM.render(React.createElement("div",{
    id:"aaa",
    className:"bbb"
},"1111"),document.getElementById("root"))
```
## React应用
### 搭建React环境
```shell
# 全局安装
$ npm install -g create-react-app
# 创建React应用
$ create-react-app appname

# 如果不想全局安装，可以直接使用npx
$ npx create-react-app myapp
```
***react在创建应用时会安装三个文件：***
- react：react的顶级库
- react-dom：因为react有很多的运行环境，比如app端的react-native，我们要在web上运行就使用react-dom
- react-scripts：包含运行和打包react应用程序的所有脚本及配置

### 组件
#### class组件
test.js
```jsx
import React from "react"

class App extends React.Component{
    render(){
        return <div>Hello App Component</div>
    }
}

export default App
```
index.js
```jsx
import React from "react";
import ReactDOM  from "react-dom";
import App from "./test";

ReactDOM.render(<App/>,document.getElementById("root"))
```
#### 函数组件
test.js
```jsx
// 函数组件
// 函数组件在16.8版本之前是无状态组件
// 16.8之后与react hooks结合

function App(){
    return (
        <div>Hello App Component</div>
    )
}

export default App
```
#### 组件嵌套
```jsx
import React, { Component } from 'react'

class Child extends Component {
    render(){
        return (
            <div>Child</div>
        )
    }
}

class Two extends Component {
    render(){
        return (
            <div>
                two
                <Child></Child>
            </div>
        )
    }
}

function Three() {
    return (
        <div>three</div>
    )
}

const Four = ()=><div>Four</div>

export default class One extends Component {
  render() {
    return (
        <div>
            <Two></Two>
            <Three></Three>
            <Four></Four>
        </div>
    )
  }
}
```
#### 组件样式
##### 内部设置
```jsx
import React from "react"

class App extends React.Component{
    render(){
        const cs = {
            backgroundColor: "yellow", //background-color
            fontSize: "30px" //font-size
        }
        return <div style={cs}>Hello App Component</div>
    }
}
export default App
```
##### 外部引用
index.css
```css
.active{
    background-color: blue;
}

#myapp{
    background-color: green;
}
```
one.js
```jsx
import React, { Component } from 'react'
import "./index.css"

export default class one extends Component {
  render() {
    return (
      <div>
        <div className='active'>11111</div>
        <div id='myapp'>22222</div>
  
        <label htmlFor='username'>用户名：</label>
        <input type="text" id='username' />
      </div>
    )
  }
}
```
### 事件绑定
- 第一种、第三种和第四种均不会产生this指针的问题
- 第二种会产生this指针的问题。若需要处理点击[[this指针问题#修改this的指向]]
```jsx
import React, { Component } from 'react'

export default class one extends Component {
  render() {
    return (
        <div>
            <button onClick={
                ()=>{
                    console.log("click")
                }
            }>add1</button>
            <button onClick={ this.handleClick2 }>add2</button>
            <button onClick={ this.handleClick3 }>add3</button>
            <button onClick={ ()=>this.handleClick3() }>add4</button>
        </div>
    )
  }

  handleClick2(){
    console.log("click2");
  }
  
  handleClick3 = ()=>{
    console.log("click3");
  }

  handleClick4(){
    console.log("click4");
  }
}
```
- ***React并不会真正的绑定事件到每一个具体的元素上，而是采用事件代理的模式（挂载到根节点上）***
- 和普通浏览器一样，事件handler会被自动传入一个event对象，这个对象和普通的浏览器event对象所包含的方法和属性都基本一致。不同的是React中的event对象并不是浏览器提供的，而是它自己内部所构建的。它同样具有`event.stopPropagation`、`event.preventDefault`这种常用的方法
### ref的使用
```jsx
import React, { Component } from 'react'

export default class one extends Component {

    myref = React.createRef()

    render() {
        return (
            <div>
                <input ref={this.myref} />
                <button onClick={
                    ()=>{
                        console.log("click",this.myref.current.value);
                    }
                }>
                获取输入框的文本内容</button>
            </div>
        )
    }
}
```
### 状态state
```jsx
import React, { Component } from 'react'

export default class one extends Component {
	//第一种设置state
    constructor(){
	    super()
	    this.state = {
		    text: "未改变"
	    }
    }
    
    //第二种定义状态
    state = {
        text: "未改变"
    }

    render() {
        return (
            <div>
                <button onClick={
                    ()=>{
                        this.setState({
                            text:"改变"
                        })
                    }
                }>{this.state.text}</button>
            </div>
        )
    }
}
```

### 列表渲染
#### 示例代码
```jsx
import React, { Component } from 'react'

export default class one extends Component {
    state = {
        list: [{
            id: 1,
            text: "1111"
        },
        {
            id: 2,
            text: "2222"
        },
        {
            id: 3,
            text: "3333"
        }
        ]
    }
    render() {
        return (
            <div>
                <ul>
                    {
	                    //第一种以item.id为key
                        this.state.list.map(item=>{
                            <li key = {item.id}>{item.text}</li>
                        })
                        //第二种以index为key
                        this.state.list.map((item,index)=>{
                            <li key = {index}>{item.text}</li>
                        })
                    }
                </ul>
            </div>
        )
    }
}

```
#### 为什么要设置key
- 为了列表的复用和重排，提高性能
	- React的高效依赖于所谓的Virtual DOM，尽量不碰DOM。对于列表元素来说会有一个问题：元素可能会在一个列表中改变位置。要实现这个操作，只需要交换一下DOM位置就行了，但是React并不知道其实我们只是改变了元素的位置，所以它会重新渲染后面两个元素（再执行Virtual DOM），这样会大大增加DOM操作。但如果给每个元素加上唯一的标识，React就可以知道这两个元素只是交换了位置，这个标识就是key，这个key必须是每个元素唯一的标识。
- 理想的key值为`item.id`，如果不涉及到列表的增加删除、重拍，设置成索引（index）没有问题
### 富文本展示
```jsx
<span dangerouslySetInnerHTML = {
	{
		__html:this.myhtml
	}
}>
</span>
```

