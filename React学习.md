# React

## 1 React简介

### 1.1 定义及优势

>
>

- 定义：一个将数据渲染成HTML视图的开源 JavaScript 库

- 传统DOM操作的==痛点==：
  - 原生 JS 操作 DOM 繁琐、效率低（DOM-API 操作 UI)
  - 直接操作 DOM，浏览器会进行大量的重绘重排
  - 原生 JS 没有==组件化==编码方案，代码复用率低

- React的特点
  - 采用==组件化==模式，==声明式编码==，提高开发效率及组件复用率
  - 在 React Native 中可以使用 React 语法进行==移动端开发==（android / iOS 开发)
  - 使用==虚拟DOM== + 优秀的 ==Diff算法==（==最小化页面重绘==），尽量减少与真实DOM的交互

- 示例

```html
<body>
  <div id="app"></div>
  <!-- 引入React核心库 -->
  <script src="./js/react.development.js"></script>
  <!-- 引入react-dom，用于支持React操作DOM -->
  <script src="./js/react-dom.development.js"></script>
  <!-- 引入babel，用于将jsx转换成js -->
  <script src="./js/babel.min.js"></script>
  <script type="text/babel"> /* 此处一定要写babel */
    // 1.创建虚拟DOM
    const vDom = (
      <h1 id="title">
        <span>haha</span>
        <span>Hello React</span>
      </h1>
    )
    // 将虚拟DOM渲染到页面上
    ReactDOM.render(vDom, document.getElementById('app'))
  </script>
</body>
```

---



### 1.2 虚拟DOM

>
>

- 本质：==Object类型的对象==
- 虚拟DOM比较“轻”，真实DOM比较“重”，无需真实DOM上的所有属性
- 虚拟DOM最终会被React渲染成页面上的真实DOM

---



### 1.3 ==JSX语法规则==

>JSX语法

- 定义虚拟DOM时，不要使用引号
- 标签中混入JS表达式（==非js语句==）要使用`{}`
- 样式的类名指定不要用`class`，要用`className`
- 内联样式，要使用`style={{key: value}}`。其中`key`使用==驼峰命名==，`value`为==字符串==形式
- React 会自动添加 ==”px”== 后缀到内联样式为数字的属性后。如需使用 ”px” 以外的单位，请将此值设为数字与所需单位组成的字符串
- 虚拟DOM只能含有==一个根标签==
- 标签必须闭合
- 标签首字母
  - 若小写字母开头，则将该标签转为html中同名元素；若html中无该标签对应的同名元素，则报错
  - 若大写字母开头，react就去渲染对应的组件，若组件没有定义则报错
- `false`、`null`、`undefined`、 `true` 是合法的子元素。但它们并不会被渲染值得注意的是有一些 [“falsy” 值](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)，如数字 `0`，仍然会被 React 渲染；如果你想渲染 `false`、`true`、`null`、`undefined` 等值，你需要先将它们转化为==字符串==

```jsx
const id = 'title'
// 1.创建虚拟DOM
const vDom = (
  <div id={id} className="app">
    <span style={{marginLeft: '50px'}}>haha</span>
    <span>Hello React</span>
    <input type="text" />
  </div>
)
// 将虚拟DOM渲染到页面上
ReactDOM.render(vDom, document.getElementById('app'))
```

- jsx注释写法：`{/*标签*/}`

```jsx
const vDom = (
  <div id={id} className="app">
    {/*<span style={{marginLeft: '50px'}}>haha</span>*/}
  </div>
)
```





- JSX小练习

```jsx
<div id="app"></div>
<script type="text/babel">
  const title = '前端js框架列表'
  const arr = ['Angular', 'React', 'Vue']
  //创建虚拟DOM
  const vDom = (
    <div className="app">
      <h1>{title}</h1>
      <ul style={{padding: '0'}}>
        {
          arr.map((value,index) => <li key={index}>{value}</li>)
        }
      </ul>  
  	</div>
  )
ReactDOM.render(vDom, document.getElementById('app'))
</script>
```

- ==React会自动遍历数组，对象则不能==

---



### 1.4 组件与模块

>
>
>模块

- 理解：向外提供特定功能的js程序，一般就是一个js文件
- 作用：复用js代码，简化js的编写，提高js运行效率

---

>
>
>组件

- 理解：用来实现局部功能效果的代码和资源的集合(html/css/js/image等)
- 作用：复用编码，简化项目编码，提高运行效率

---



## 2 面向组件编程

### 2.1 基本理解和使用

>
>
>函数式组件

- 定义函数时函数名首字母必须==大写==
- 函数中的`this`为`undefined`，因为babel转换jsx时开启了严格模式
- 使用`render`函数渲染组件时，虚拟DOM要使用大写的==组件标签==，这样React就会去解析对应的组件

```jsx
// 创建函数式组件
function Demo() {
  console.log(this) // 此处的this是undefined，因为babel转换jsx时开启了严格模式
  const content = '我是【函数式】组件'
  return <h2>{content}</h2>
}
//渲染组件到页面
ReactDOM.render(<Demo/>, document.getElementById('app'))
```

---

>
>
>类式组件

- 继承`React.Component`类
- 必须有`render()`方法，且必须有==返回值==。`render()`方法放在类的原型对象上，供实例使用
- `render()`方法中的`this`是组件实例
- `render()`调用时机：==初始化渲染==、==状态更新==

```jsx
// 创建类式组件
class Demo extends React.Component {
  render() {
    console.log(this) // Demo组件的实例对象
    return (
      <h1>我是【类式】组件</h1>
    )
  }
}
// 渲染组件到页面
ReactDOM.render(<Demo/>, document.getElementById('app'))
// 调用ReactDOM.reder()后执行的操作
/*
	(1)React解析组件标签，找到了Demo组件
	(2)发现组件是使用类进行定义的，随后new出该类的实例，并通过该实例调用原型上的reder方法
	(3)将render返回的虚拟DOM转换为真实DOM渲染到页面上
*/
```

- 简单组件(函数式组件)和复杂组件的区分：有无状态`state`

---



### 2.2 ==组件实例的三大核心属性 state==

```jsx
class Demo extends React.Component {
  constructor(props) {
    super(props)
    // 初始化状态
    this.state = {
      isHot: true
    }
  }
  render() {
    // 读取状态数据
    const { isHot } = this.state
    return (
      <h1>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
    )
  }
}
ReactDOM.render(<Demo/>, document.getElementById('app'))
```

---



####  2.2.1 React事件绑定

- React 事件的命名采用==小驼峰式（camelCase）==，而不是纯小写。
- 使用 JSX 语法时你需要传入一个函数作为事件处理函数，而不是一个字符串。

```jsx
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

- ==this指向问题==

```jsx
render() {
  const { isHot } = this.state
  return (
    <h1 onClick={this.click}>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
  )
}
// 处理点击事件
click() {
  console.log(this) // undefined
  // 由于click是作为onClick的回调，所以不是通过实例调用，而是直接调用
  // 类中的方法默认开启了严格模式，所以该处的this为undefined
}
```

- ==解决办法==：让原型中的方法挂载到组件实例this上

```javascript
class Demo extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      isHot: true
    }
    ==================================
    this.click = this.click.bind(this) // 让原型中的click方法挂载到组件实例上
  }
  render() {
    const { isHot } = this.state
    return (
      <h1 onClick={this.click}>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
    )
  }
  // 处理点击事件
  click() {
    console.log(this) // Demo实例 
  }
}
```

---



#### 2.2.2 setState使用

- 产生原因：==直接==更改状态数据，视图并不会发生变化

- ==React中修改状态必须使用`setState`进行更改==，且更新是一种==合并==，不是直接替换

```jsx
const {isHot} = this.state
this.setState({isHot: !isHot})
```

- 构造器只执行==一次==

- `render()`调用次数：`1+n`（1为初始化时调用，n是状态更新的次数）

- 事件处理函数调用次数：触发几次就调用几次

完整写法

```jsx
class Demo extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      isHot: true
    }
    this.click = this.click.bind(this)
  }
  render() {
    const { isHot } = this.state
    return (
      <h1 onClick={this.click}>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
    )
  }
  // 处理点击事件
  click() {
    const isHot = this.state.isHot
    this.setState({isHot: !isHot})
  }
}

ReactDOM.render(<Demo/>, document.getElementById('app'))
```

---



#### 2.2.3 ==state的简写方式==

```jsx
class Demo extends React.Component {
  state = {
    isHot: true
  }
  render() {
    const { isHot } = this.state
    return (
      <h1 onClick={this.click}>今天天气很{isHot ? "炎热" : "凉爽"}</h1>
    )
  }
  // 处理点击事件
  // 自定义方法 —— 使用箭头函数解决this指向问题
  click = () => {
    const isHot = this.state.isHot
    this.setState({ isHot: !isHot })
  }
}

ReactDOM.render(<Demo />, document.getElementById("app"))
```

---



#### 2.2.4 state总结

- 组件中`render()`方法中的`this`指向组件==实例对象==

- 组件自定义方法中的`this`为`undefined`，解决方法：
  - 强制绑定this：使用函数对象的`bind()`方法
  - ==箭头函数==

- 状态数据，不能直接修改，要使用`setState()`方法进行更新，该方法更新状态数据是==合并==操作

---



### 2.3 ==组件实例的三大核心属性 props==

#### 2.3.1 基本使用

```jsx
class Person extends React.Component {
  render() {
    console.log(this) // 组件实例
    const { name, age, gender } = this.props
    return (
      <ul>
        <li>姓名：{name}</li>
        <li>年龄：{age}</li>
        <li>性别：{gender}</li>
      </ul>
		)
	}
}

ReactDOM.render(<Person name="jack" age="18" gender="男" />, document.getElementById('app'))
```

---



#### 2.3.2 ==批量传递props==

==使用展开运算符`...`==

```jsx
const person = { name: 'jack', age: 18, gender: '男' }
ReactDOM.render(<Person {...person} />, document.getElementById('app'))
```

- 在react中，使用展开运算符展开==对象==只能用于==标签属性的传递==
- 你还可以选择只保留当前组件需要接收的 props，并使用展开运算符将==其他 props== 传递下去

```jsx
const Button = props => {
  const { kind, ...other } = props;
  const className = kind === "primary" ? "PrimaryButton" : "SecondaryButton";
  return <button className={className} {...other} />;
};
```

---



#### 2.3.3 ==对props进行限制==

- ==`props`是只读的==
- 引入`prop-types.js文件`
- 限制标签属性：`propTypes`
  - 必要：`isRequired`
  - 函数类型：`func`
- 设置标签属性默认值：`defaultProps`

```jsx
// 对标签属性进行类型、必要性的限制
Person.propTypes = {
  name: PropTypes.string.isRequired,
  gender: PropTypes.string,
  speak: PropTypes.func, // 限制为函数类型
  colors: PropTypes.array // 数组
}
// 指定标签默认属性值
Person.defaultProps = {
  age: 18 // age默认值为18
}

const person = { name: 'jack', age: 18, gender: '男', speak: function() {}, colors: ['red','blue'] }
ReactDOM.render(<Person {...person} />, document.getElementById('app'))
```

---



#### 2.3.4 ==props简写方式==

- ==使用`static定义静态属性`==

```javascript
class Person extends React.Component {
  // 对标签属性进行类型、必要性的限制
  static propTypes = {
    name: PropTypes.string.isRequired,
    gender: PropTypes.string,
    speak: PropTypes.func // 限制为函数类型
  }
  // 指定标签默认属性值
  static defaultProps = {
    age: 18
  }
  render() {
    console.log(this)
    const { name, age, gender } = this.props
    return (
      <ul>
        <li>姓名：{name}</li>
        <li>年龄：{age}</li>
        <li>性别：{gender}</li>
      </ul>
  	)
	}
}
```

---



#### 2.3.5 类构造器和props

在 React 组件挂载之前，会调用它的构造函数。在为 React.Component 子类实现构造函数时，应在其他语句之前前调用 `super(props)`。否则，`this.props` 在构造函数中可能会出现未定义的 bug。

通常，在 React 中，构造函数仅用于以下两种情况：

- 通过给 `this.state` 赋值对象来初始化【内部 state】
- 为【事件处理函数】绑定实例

```jsx
constructor(props) {
  super(props)
  // 如果不使用super(props)，在构造函数中无法获取 this.props
  console.log('constructor', this.props) 
}
```

- 总结：类中的构造器==能不写就不写==

---



#### 2.3.6 函数式组件使用props

- 函数式组件接收了默认参数`props`

```jsx
function Person(props) {
  console.log(props) // {name: "john", age: 20, gender: "女"}
  return (
    <ul>
      <li>姓名：{props.name}</li>
    </ul>
  )
}
ReactDOM.render(<Person name="john" age={20} gender="女" />, document.getElementById('app'))
```

---



#### 2.3.7 ==子传父==

- 通过`props`传递，需要父组件提前给子组件传递一个==函数==，在子组件中调用该函数即可完成子传父通信

```jsx
// 父组件
class Fu extends React.Component {
  receiveState = (id) => {
    console.log(id) // 10
  }
  render() {
    return <div><Zi receiveState={this.receiveState}/></div>
  }
}

// 子组件
class Zi extends React.Component {
  sendState = () => {
    this.props.receiveState(10)
  }
  render() {
    return <div><button onClick={this.sendState}>子传父</button></div>
  }
}
```

---



#### 2.3.8 ==消息订阅与发布(兄弟组件通信)==

```powershell
npm install pubsub-js -S
```

- A组件（==发布消息==）

```jsx
import React, { Component } from 'react'
import PubSub from 'pubsub-js'
export default class A extends Component {
  // 发布消息
  subscribeMsg = () => {
    PubSub.publish('Msg', 10)
  }
  render() {
    return (
      <div>
        <button onClick={this.subscribeMsg}>发布消息</button>
      </div>
    )
  }
}
```

- B组件（==订阅消息==）

```jsx
import React, { Component } from 'react'
import PubSub from 'pubsub-js'
export default class B extends Component {
  state = {
    data: ''
  }
  // 组件挂载完毕
  componentDidMount() {
    PubSub.subscribe('Msg', (msg, data) => this.setState({data}))
  }
  render() {
    return (
      <div>
        <h1>B组件接收到的消息：{this.state.data}</h1>
      </div>
    )
  }
}
```

- ==取消订阅==

```jsx
 componentDidMount() {
   this.token = PubSub.subscribe('Msg', (msg, data) => this.setState({data}))
 }
// 取消订阅
PubSub.unsubscribe(this.token)
```

- 该方法可以适用==任何组件之间进行通信==

---



#### 2.3.8 标签体收集

- 使用`props`里的`children`属性获取。同时`children`也是一个==标签属性==

- 例如`<A children="Home">` <=> `<A>Home</A>` 等价

```jsx
export default class MyNavLink extends Component {
  render() {
    return (
      <NavLink className="list-group-item" activeClassName="link" {...this.props} />
    )
  }
}
```

---



### 2.4 ==组件实例的三大核心属性 refs==

#### 2.4.1 字符串形式的ref（不推荐使用）

- 给标签添加`ref`属性，用于收集DOM节点
- 在组件实例中使用`this.refs`获取DOM节点

```jsx
class Demo extends React.Component {
  render() {
    return (
      <div className="app">
        <input type="text" ref="input1" placeholder="点击右侧按钮提示输入内容" />
        <button style={{margin: '0 20px'}} onClick={this.clickBtn}>点击</button>
        <input type="text" ref="input2" onBlur={this.blurInput}  placeholder="失去焦点提示输入内容" />
      </div>
    )
  }
  clickBtn = () => {
    const { input1 } = this.refs
    alert(input1.value)
  }
  blurInput = () => {
    const { input2 } = this.refs
    alert(input2.value)
  }
}
```

- ==React官方推荐不再使用该API，因为存在**效率不高**问题==

---



#### 2.4.2 回调函数形式的ref

- 将函数赋值给`ref`，React执行时会自动调用该回调函数
- 回调函数接收一个参数，参数为==该DOM节点==。将该节点挂载在组件实例`this`上供后续使用

```jsx
class Demo extends React.Component {
  render() {
    return (
      <div>
      	<input type="text" ref={node => this.input2 = node} onBlur={this.blurInput} />
      </div>
    )
  }
  blurInput = () => {
    alert(this.input2.value)
  }
}
```

- 回调ref中调用次数的问题

  如果 `ref` 回调函数是以内联函数的方式定义的，在==更新==过程中它会被执行==两次==，第一次传入参数 `null`，然后第二次会传入参数 DOM 元素。这是因为在每次渲染时会创建一个新的函数实例，所以 React 清空旧的 ref 并且设置新的。通过将 ref 的回调函数定义成 class 的绑定函数的方式可以避免上述问题，但是大多数情况下它是无关紧要的。

```jsx
class Demo extends React.Component {
  render() {
    return (
      <div>
      	<input type="text" ref={this.saveInput} onBlur={this.blurInput} />
      </div>
    )
  }
  blurInput = () => {
    alert(this.input2.value)
  }
  // class绑定函数的方式，该方式不会频繁创建回调函数实例
  saveInput = node => this.input2 = node
}
```

---



#### 2.4.3 createRef 使用

- `React.createRef()`调用后返回一个==容器==，该容器可以存储被`ref`标识的==节点==
- ==注意==：该返回的==容器==是”==专人专用==“，只能存储一个`ref`。如果有多个`ref`则需使用`React.createRef()`创建多个`ref`容器

```jsx
class Demo extends React.Component {
  myRef = React.createRef()
  render() {
    return (
      <div className="app">
        <input type="text" ref={this.myRef} placeholder="点击右侧按钮提示输入内容" />
        <button style={{margin: '0 20px'}} onClick={this.clickBtn}>点击</button>
      </div>
    )
  }
  // 展示左侧输入框的数据
  clickBtn = () => {
    console.log(this.myRef) // {current: input}
    alert(this.myRef.current.value)
  }
}
```

---



### 2.5 React 事件处理

- 通过onXxx属性指定事件处理函数（==驼峰命名==）
  - React使用的是==自定义(合成)==事件，而不是使用原生DOM事件——为了更好的==兼容性==
  - React中的事件是通过==事件委托==方式处理的（委托给组件最外层的元素）——为了==高效==
- 通过`event.target`得到发生事件的DOM元素对象——不要过度使用`ref`

---



### 2.6 ==高阶函数柯里化==

- 使用高阶函数柯里化

```jsx
<input type="text" name="username" onChange={this.saveFormData('username')} />
saveFormData = (dataType) => {
  return e => this.setState({[dataType]: e.target.value})
}
```

- 不使用柯里化

```jsx
<input type="text" name="username" onChange={e => this.saveFormData('username', e)} />
saveFormData = (dataType, e) => {
  this.setState({[dataType]: e.target.value})
}
```

---



### 2.7 ==**组件生命周期**==

#### 2.7.1 生命周期（旧）

<img src="D:\JAVA\学习资料\Web前端入门教程视频\React\react全家桶资料\02_原理图\react生命周期(旧).png" alt="react生命周期(旧)" style="zoom:80%;" />

---

- ==组件挂载阶段==：`constructor`(构造函数) -> `componentwillMount`(组件即将挂载) -> `render`(组件渲染) ->  `componentDidMount`(组件渲染完成) -> `componentWillUnmount`(组件即将被销毁)
- 使用`setState()`更新组件阶段
  - `shouldComponentUpdate`：==控制组件是否允许更新的“阀门”==。该钩子接收的返回值为布尔类型；该钩子如果不写，底层==会自动调用并且返回`true`==允许更新；如果写该钩子并且返回`false`，则后面的阶段不再执行
  - `shouldComponentUpdate` -> `componentWillUpdate`(组件即将更新) -> `render`(重新渲染) -> `componentDidUpdate`(组件完成更新) -> `componentWillUnmount`(组件即将销毁)

- 使用`forceUpdate()`更新组件阶段
  - `forceUpdate()`会==强制更新状态，但不会更改组件状态==
  - `componentWillUpdate`(组件即将更新) -> `render`(重新渲染) -> `componentDidUpdate`(组件完成更新) -> `componentWillUnmount`(组件即将销毁)

- 父组件`render`更新阶段
  - `componentWillReceiveProps`钩子只在==子组件接收到新**props**时才会调用==。该钩子函数可以收集到一个参数`props`，是从父组件传递过来的数据
  - `componentWillReceiveProps`(接收到父组件的==新==props) -> `shouldComponentUpdate`(控制组件是否更新) -> `componentWillUpdate`(组件即将更新) -> `render`(组件重新渲染) -> `componentDidUpdate`(组件完成更新) -> `componentWillUnmount`(组件即将销毁)

- 卸载组件：使用`ReactDOM.unmountComponentAtNode('要渲染的容器节点')`

- ==常用的生命周期钩子==

  - `componentDidMount`：在该钩子中做一些初始化，例如 开启定时器、发送网络请求、订阅消息等
  - `componentWillUnmount`：做一些收尾的事情，比如关闭定时器、取消订阅消息

  - `render`

---



#### 2.7.2 生命周期（新）

<img src="D:\JAVA\学习资料\Web前端入门教程视频\React\react全家桶资料\02_原理图\react生命周期(新).png" alt="react生命周期(新)" style="zoom: 75%;" />

---

相比**React16**的变化——即将废弃3个钩子，增加2个新钩子

- 在使用`componentWillMount`、`componentWillReceiveProps`、`componentWillUpdate`时建议加上`UNSAFE_`前缀
- `static getDerivedStateFromProps(props, state)`：若`state`的值在任何时候都取决了`props`时，使用该钩子
- `getSnapshotBeforeUpdate`：在更新之前获取==快照==

---



### ==**2.8 Diff 算法及 key的使用**==

#### 2.8.1 虚拟DOM中key的作用

- 概括：`key`是虚拟DOM对象的==标识==，在更新显示时`key`起着极其重要的作用

- 详细：当==状态==中的数据发生变化时，React会根据==新数据==生成==新的虚拟DOM==，之后React会进行==新虚拟DOM==与==旧虚拟DOM==的**diff**比较

  - 旧虚拟DOM中找到了与新虚拟DOM**相同**的`key`

    - 若虚拟DOM中==内容没变==，直接复用之前的**真实**DOM

    - 若虚拟DOM中==内容变了==，则生成新的**真实DOM**，随后==替换==掉页面中之前的真实DOM

  - 旧虚拟DOM中没有找到与新虚拟DOM相同的`key`

    - 创建新的**真实DOM**，随后渲染到页面中

---



#### 2.8.2 使用index作为key的注意

- 若对数据进行：==逆序添加==、==逆序删除==等**破坏顺序**操作——会产生没有必要的真实DOM的更新 => 界面效果没问题，但==效率低==
- 如果结构中包括**输入类的DOM**——会产生错误DOM更新 => 界面有问题
- 如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，可以使用`index`作为`key`
- ==总结==：最好使用每条数据的==唯一标识==作为`key`

---







## 3 React应用(脚手架)

>
>
>样式模块化

- 将 xxx.css 文件 改名 ==xxx.module.css==
- 在组件中引入

```jsx
import demo from './demo.module.css'
...
render() {
  return <h1 className={demo.title}></h1> // title为类名
}
```

---

>
>
>组件化编码流程

1. 拆分组件：拆分界面，抽取组件
2. 实现静态组件
3. 实现动态组件：数据类型 -> 数据名称 -> 数据放入哪个组件 -> 交互（从绑定事件开始）

---





## 4 React ajax

### 4.1 脚手架配置代理

>
>
>方法一

在`package.json`中添加如下字段：

> 在package.json中追加如下配置

```json
"proxy":"http://localhost:5000"
```

说明：

1. 优点：配置简单，前端请求资源时可以不加任何前缀。
2. 缺点：不能配置多个代理。
3. 工作方式：上述方式配置代理，当请求了3000不存在的资源时，那么该请求会转发给5000 （优先匹配前端资源）

---

>
>
>方法二

1. 第一步：创建代理配置文件

   ```
   在src下创建配置文件：src/setupProxy.js
   ```

2. 编写setupProxy.js配置具体代理规则：

   ```js
   const proxy = require('http-proxy-middleware')
   
   module.exports = function(app) {
     app.use(
       proxy('/api1', {  //api1是需要转发的请求(所有带有/api1前缀的请求都会转发给5000)
         target: 'http://localhost:5000', //配置转发目标地址(能返回数据的服务器地址)
         changeOrigin: true, //控制服务器接收到的请求头中host字段的值
         /*
         	changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
         	changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:3000
         	changeOrigin默认值为false，但我们一般将changeOrigin值设为true
         */
         pathRewrite: {'^/api1': ''} //去除请求前缀，保证交给后台服务器的是正常请求地址(必须配置)
       }),
       proxy('/api2', { 
         target: 'http://localhost:5001',
         changeOrigin: true,
         pathRewrite: {'^/api2': ''}
       })
     )
   }
   
   // 发送请求url http://localhost:3000/api2/students
   ```

说明：

1. 优点：可以配置多个代理，可以灵活的控制请求是否走代理。
2. 缺点：配置繁琐，前端请求资源时必须加前缀。

---





## 5 React 路由

### 5.1 路由的理解

>
>
>SPA的理解

- 单页Web页面
- 整个应用只有==一个完整的页面==
- 点击页面中的链接==不会刷新==页面，只会做页面的==局部刷新==
- 数据都需要通过ajax获取，并在前端异步展示

---



>
>
>路由定义

- 一个路由就是一个==映射==关系（key-value）
- `key`为路径，`value`为`function`或`component`

---



>
>
>路由分类

- 前端路由：
  - 浏览器路由，`value`是组件，用于展示页面内容
  - 注册路由：`<Route path="/test" component={Test}>`
  - 工作过程：当浏览器的`path`变为`/test`时，前端路由组件就会变成`Test`组件
- 后端路由
  - `value`是函数，用于处理客户端提交的请求
  - 注册路由：`router.get(path, function(req, res))`
  - 工作过程：当node服务器接收到一个请求时，根据请求路径找到匹配的路由，调整路由中的函数来处理请求，返回响应数据

---





### 5.2 ==react-router-dom使用==

- 使用`Link`组件来代替传统`a`标签

```jsx
import { Link } from 'react-router-dom'
<Link to="/home"> Home </Link>
```

- `NavLink`组件：点击链接后==默认==追加一个`active`，也可以使用`activeClassName`标签属性指定点击链接后追加的类名

```jsx
import { NavLink } from 'react-router-dom'
<NavLink to="/home" activeClassName="link"> Home </NavLink>
```

- 使用`Route`组件进行路由—组件匹配

```jsx
import { Route } from 'react-router-dom'
import Home from './components/Home'
<Route path="/home" component={Home} />
```

- 使用`Switch`组件==提高路由匹配效率== — 匹配到路径对应的==第一个路由==后，不再往下匹配

```jsx
import { Route, Switch } from 'react-router-dom'
import Home from './components/Home'
import Test from './components/Test'
<Switch>
  <Route path="/home" component={Home} />
	<Route path="/home" component={Test} /> // 该组件不会匹配渲染
</Switch>
```

- `<App>`组件最外侧包裹一个`<BrowserRouter>`(HIstory路由) 或 `HashRouter`(Hash路由)，用于管理整个应用的路由

```jsx
import { BrowserRouter } from 'react-router-dom'
import App from './App'

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById('root')
)
```

- `Redirect`：重定向路由——放在所有路由注册的最后，所有路由匹配不到时会使用`to`属性对应的路由

```jsx
import { Route, Switch, Redirect } from 'react-router-dom'
<Route path="/home" component={Home} />
<Route path="/about" component={About} />
<Redirect to="/home"/>
```

---





### 5.3 一般组件与路由组件区别

- 写法不同

  - 一般组件：`<Demo/>`
  - 路由组件：`<Route path="/demo" component={Demo} />`

- 文件夹结构不同

  - 一般组件：components/
  - 路由组件：pages/

- ==接收到的**props**不同==

  - 一般组件：写组件标签时，传递的是什么就可以接收到什么
  - 路由组件：接收到3个固定参数

  ```javascript
  {
    history: {
      action: "PUSH",
      go: ƒ go(n),
      goBack: ƒ goBack(),
      goForward: ƒ goForward(),
      push: ƒ push(path, state),
      replace: ƒ replace(path, state)
    }
    
    location: {
      pathname: "/home",
      search: "",
      state: undefined
    }
    
    match: {
      params: {},
      path: "/home",
      url: "/home"
    }
  }
  ```

---



### 5.4 路由的模糊匹配与严格匹配

- 模糊匹配（默认）

```jsx
<NavLink to="/home/a/b">Home</NavLink>
// 可以匹配成功
<Route path="/home" component={Home} />

<NavLink to="/a/home/b">About</NavLink>
// 无法成功匹配路由组件
<Route path="/home" component={About} />
```

- 严格匹配：在`<Route>`标签加上`exact`属性
- 严格模式需要用时再开启，有些时候开启可能导致==嵌套路由无法生效==

```jsx
<NavLink to="/home/a/b">About</NavLink>
// 无法成功匹配路由组件
<Route exact path="/home" component={Home} />
```

---



### 5.5 路由嵌套

```jsx
import { Switch, Route, Redirect } from 'react-router-dom'

<MyNavLink to="/home/news">News</MyNavLink>
<MyNavLink to="/home/message">Message</MyNavLink>
<Switch>
  <Route path="/home/news" component={News} />
  <Route path="/home/message" component={Message} />
  <Redirect to="/home/news"/>
</Switch>
```

---



### 5.6 ==路由传参==

#### 5.6.1 params参数

- 向路由组件传递`params`参数

```jsx
<Link to={`/home/message/detail/tom/18`}>详情</Link>
```

- 声明接收`params`参数：`/:参数`

```jsx
<Route path="/home/message/detail/:name/:age" component={Test}></Route>
```

- 在路由组件中取出参数：从`this.props.match.params`中取

```jsx
// const {match: {params: {name, age}}} = this.props
const {{params: {id, content}}} = this.props.match
console.log(name, age) // 'tom' 18
```

---



#### 5.6.2 search参数

- 向路由组件传递`search`参数

```jsx
<Link to={'/home/message/detail/?name=tom&age=18'}>详情</Link>
```

- `search`参数无需声明接收，正常注册路由即可：

```jsx
<Route path="/home/message/detail" component={Test}></Route>
```

- 在路由组件中取出参数：从`this.props.location.search`取，取出来是一个`urlencoded`字符串

- 使用`querystring`库处理该字符串

  - querystring库的使用

  ```javascript
  import qs from 'querystring'
  // 对象转urlencoded字符串
  qs.stringify(str)
  // urlencoded字符串转对象
  qs.parse(str) // 第一个参数带问号?
  ```

  - 处理路由参数

  ```jsx
  import qs from 'querystring'
  const { location: {search} } = this.props
  const { name, age } = qs.parse(search.substr(1))
  console.log(name, age) // 'tom' 18
  ```

---



#### 5.6.3 state参数(地址栏上隐藏)

- 缺点：刷新页面参数也不会丢失，==清除缓存后数据丢失==

- 向路由组件传递`state`参数

```jsx
<Link to={{pathname: '/home/message/detail', state: {name: 'tom', age: 18}}}>详情</Link>
```

- `state`参数无需声明接收，正常注册路由即可：

```jsx
<Route path="/home/message/detail" component={Test}></Route>
```

- 在路由组件中取出参数：从`this.props.location.state`中取

```jsx
const {state: {id, content}} = this.props.location
console.log(name, age) // 'tom' 18
```

---



#### 5.6.4 路由跳转模式

- 默认为`push`模式
- 指定`replace`模式：添加`replace`属性

```jsx
<Link replace to={{pathname: '/home/message/detail', state: {name: 'tom', age: 18}}}>详情</Link>
```

---





### 5.7 ==编程式路由导航==

- `params`参数形式

```jsx
push = (id, content) => {
  const { history: { push } } = this.props
  push(`/home/message/detail/${id}/${content}`)
}
replace = (id, content) => {
  const { history: { replace } } = this.props
  replace(`/home/message/detail/${id}/${content}`)
}
```

- `search`参数形式

```jsx
push = (id, content) => {
  const { history: { push } } = this.props
  push(`/home/message/detail/?id=${id}&content=${content}`)
}
replace = (id, content) => {
  const { history: { replace } } = this.props
  replace(`/home/message/detail/?id=${id}&content=${content}`)
}
```

- `state`参数形式

```jsx
push = (id, content) => {
  const { history: { push } } = this.props
  push('/home/message/detail', {id,content})
}
replace = (id, content) => {
  const { history: { replace } } = this.props
  replace('/home/message/detail', {id,content})
}
```

---



### 5.8 ==withRouter的使用==

- 使用场景：加工组件，给==非路由组件==添加==路由组件==特有的方法
- `withRouter()`的返回值是一个新组件

```jsx
import { withRouter } from 'react-router-dom'
class Header extends Component {
  componentDidMount() {
    setTimeout(() => this.props.history.push('/home/message'), 2000)
  }
  
  render() {
    console.log(this.props)
    return (
      <h2>111</h2>
    )
  }
}

export default withRouter(Header)
```

---



### 5.9 BrowserRouter与HashRouter区别

- ==底层实现原理不同==
  - `BrowserRouter`使用的是H5的**history API**，不兼容IE9及以下
  - `HashRouter`使用的是URL的==哈希值==，`#`后面的哈希值不会发送给服务器
- ==path表现形式不同==
  - `BrowserRouter`的路径中没有`#`，如：http://localhost:3000/demo
  - `HashRouter`的路径中带有`#`，如：http://localhost:3000/#/demo

- ==**刷新后对state参数的影响**==
  - `BrowserRouter`没有任何影响，因为`state`会保持在`history对象中`
  - `HashRouter`刷新后会导致路由`state`参数==丢失==
- `HashRouter`可以解决一些==路径错误问题==

---





## 6 Ant-Design使用

>
>
>按需引入及定制主题

```tex
1.安装依赖：yarn add react-app-rewired customize-cra babel-plugin-import less less-loader

2.修改package.json
	....
  "scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-scripts eject"
	}
	....
	
3.根目录下创建config-overrides.js
	//配置具体的修改规则
  const { override, fixBabelImports,addLessLoader} = require('customize-cra');
  module.exports = override(
  	fixBabelImports('import', {
  		libraryName: 'antd',
  		libraryDirectory: 'es',
  		style: true
  	}),
  	addLessLoader({
  		lessOptions:{
  			javascriptEnabled: true,
  			modifyVars: { '@primary-color': 'green' } // 配置主题颜色
  		}
  	})
  )
  
4.备注：不用在组件里亲自引入样式了，即：import 'antd/dist/antd.css'应该删掉
```

---



## 7 Redux

### 7.1 理解Redux

>
>
>作用

- redux是一个专门用于做==状态管理==的JS库（不是React插件库）
- 作用：集中式管理React应用中多个组件==共享==的状态

---

>
>
>使用场景

- 某个组件的状态，需要让其他组件可以随时拿到（共享）
- 一个组件需要改变另一个组件的状态（通信）
- 总体原则：能不用就不用, 如果不用比较吃力才考虑使用

---

>
>
>Redux工作流程图

<img src="D:\JAVA\学习资料\Web前端入门教程视频\React\react全家桶资料\02_原理图\redux原理图.png" alt="redux原理图" style="zoom: 65%;" />

- **Reducers**可以==初始化==和==加工==状态数据
- 初始化时，**action**对象的`type`为默认的`@@redux_init/随机字符`，`data`数据==可以没有==；`previousState`为`undefined`

---



### 7.2 Redux的三个核心概念

#### 7.2.1 action

- 定义：动作的对象

- 包含2个属性：

  - type：标识属性, 值为字符串, 唯一, 必要属性
  - data：数据属性, 值类型任意, 可选属性
  
```jsx
{ type: 'ADD_STUDENT',data:{name: 'tom',age:18} }
```

- 异步action

```jsx
// 异步action定义为一个函数
export const addActionAsync = (data, time) => {
  return (dispatch) => {
    setTimeout(() => {
      dispatch(addAction(data))
    }, time)
  }
}

// 组件
store.dispatch(addActionAsync(value * 1, 1000))

// store.js
// 引入createStore用于创建store对象，applyMiddleware作为中间件
import { createStore, applyMiddleware } from 'redux'
// 引入reducers用于加工数据
import counterReducer from './counter_reducer'
// 引入redux-thunk，用于支持异步action
import thunk from 'redux-thunk'
// 创建并导出store对象
export default createStore(counterReducer, applyMiddleware(thunk))
```

---



#### 7.2.2 reducer

- 用于初始化状态、加工状态
- 加工时，根据旧的state和action， 产生新的state的==纯函数==

```jsx
// Reducer函数会接收到两个参数：之前的状态值；动作对象
const initState = 100 // 初始化状态
export default function counterReducers(previousState = initState, action) {
  const { type, data } = action
  switch (type) {
    case INCREMENT:
      return previousState + data
    case DECREMENT:
      return previousState - data
    default:
      return previousState
  }
}
```

---



#### 7.2.2 ==store==

- 将state、action、reducer联系在一起的对象
- 创建该对象

```jsx
import {createStore} from 'redux'
import reducer from './reducers'
const store = createStore(reducer)
```

- 该对象作用
  - `getState()`：得到state
  - `dispatch(action)`：分发action, 触发reducer调用, 产生新的state
  - `subscribe(listener)`：注册监听, 当产生了新的state时, 自动调用

```jsx
store.subscribe(() => ReactDOM.render(<App />,document.getElementById('root'))
```

---



### 7.3 react-redux

- 原理图

<img src="D:\JAVA\学习资料\Web前端入门教程视频\React\react全家桶资料\02_原理图\react-redux模型图.png" alt="react-redux模型图" style="zoom:70%;" />

---



#### 7.3.1 一般组件与容器组件

>

- UI组件

  - 只负责 UI 的呈现，不带有任何业务逻辑

  - 通过props接收数据(一般数据和函数)

  - 不使用任何 Redux 的 API

  - 一般保存在components文件夹下

- 容器组件

  - 负责管理数据和业务逻辑，不负责UI的呈现

  - 使用 Redux 的 API

  - 一般保存在containers文件夹下

---



#### 7.3.2 相关API

>

- `Provider`：让所有组件都可以得到state数据

```jsx
<Provider store={store}>
	<App />
</Provider>
```

- `connect`：用于包装 UI 组件生成容器组件
- `mapStateToProps`：将外部的数据（即state对象）转换为UI组件的标签属性
- `mapDispatchToProps`：将分发action的函数转换为UI组件的标签属性

---



#### 7.3.3 具体使用

>

- 明确两个概念：
  - UI组件:不能使用任何redux的api，只负责页面的呈现、交互等。
  - 容器组件：负责和redux通信，将结果交给UI组件。

- 如何创建一个容器组件————靠react-redux 的 connect函数
  - `connect(mapStateToProps,mapDispatchToProps)(UI组件)`
  - `mapStateToProps`：映射状态，返回值是一个对象
  - `mapDispatchToProps`：映射操作状态的方法，返回值是一个对象
- 备注1：容器组件中的store是靠props传进去的，而不是在容器组件中直接引入
- 备注2：mapDispatchToProps，也可以是一个==对象==

```jsx
// 容器组件
import { connect } from 'react-redux'

// 该函数的返回值作为状态传递给了UI组件
const mapStateToProps = state => ({count: state})

// 定义修改状态的方法
const mapDispatchToProps = dispatch => (
  {
    add: data => dispatch(addAction(data)),
    dec: data => dispatch(decAction(data)),
    addAsync : (data, time) => dispatch(addActionAsync(data, time))
  }
)

// 创建容器组件并导出
export default connect( mapStateToProps, mapDispatchToProps)(CounterUI)
```

---



#### 7.3.4 ==优化使用==

- 简写`mapDispatchToProps`：写成一个对象

```jsx
// 容器组件
import { connect } from 'react-redux'

// 该函数的返回值作为状态传递给了UI组件
const mapStateToProps = state => ({count: state}) // 映射状态

// mapDispatchToProps的精简写法
// react-redux接收到action后会自动分发(dispatch) // 映射操作状态的方法
const mapDispatchToProps = {
  add: addAction,
  dec: decAction,
  addAsync: addActionAsync
}

// 创建容器组件并导出
export default connect( mapStateToProps, mapDispatchToProps)(CounterUI)
```

- 使用`Provider`，让==所有组件==都可以得到`store`对象

```jsx
import { Provider } from 'react-redux'
import store from './redux/store'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

---



>
>
>总结

- 容器组件和UI组件整合一个文件

- 无需自己给容器组件传递store，给`<App/>`包裹一个`<Provider store={store}>`即可

- ==使用了react-redux后也不用再自己检测redux中状态的改变了，容器组件可以自动完成这个工作==

- `mapDispatchToProps`也可以简单的写成一个对象。返回的对象中的key就作为传递给UI组件props的key,value就作为传递给UI组件props的value

- ==组件使用redux的步骤==

  - 定义好UI组件---不导出
  - 引入connect生成一个容器组件，并导出，写法如下：

  ```jsx
  export default connect(
    state => ({key:value}), //映射状态
    {key:xxxxxAction} //映射操作状态的方法
  )(UI组件)
  ```

  - 在UI组件中通过``this.props.xxxxxxx`读取和操作状态

---



#### 7.3.5 ==数据共享==

- 如果在`redux`中需要存储多个状态，需要使用`combineReducers`合并多个`Reducers`
- ==合并后的总状态是一个对象！！！==

```jsx
// 引入createStore用于创建store对象
import { createStore, applyMiddleware, combineReducers } from 'redux'
// 引入redux-thunk，用于支持异步action
import thunk from 'redux-thunk'
// 引入为Counter组件服务的reducers
import counterReducer from './reducers/counter'
// 引入为Person组件服务的reducers
import personReducer from './reducers/person'
// 合并reducer为总reducers
const allReducers  = combineReducers({
  counter: counterReducer,
  person: personReducer
})
// 创建并导出store对象
export default createStore(allReducers, applyMiddleware(thunk))
```

- 在组件中取出Redux中的状态，则使用key --- value的形式去取出数据

```jsx
import { addPerson } from '../../redux/actions/person'
export default connect(
  state => ({persons: state.person, count: state.counter}),
  { addPerson }
)(Person)

// 组件中取数据
<h2>{this.props.count}</h2>
```

---



#### 7.3.6 纯函数

- 一类特别的函数: 只要是==同样==的输入(实参)，必定得到==同样==的输出(返回)

- 必须遵守以下一些约束 

  - 不得改写参数数据

  - 不会产生任何副作用，例如网络请求，输入和输出设备

  - 不能调用Date.now()或者Math.random()等不纯的方法 

- ==redux的reducer函数必须是一个纯函数==

---



#### 7.3.7 Redux开发者工具

- 安装

```bash
npm install --save-dev redux-devtools-extension
```

- 在==store.js==中使用

```jsx
import {composeWithDevTools} from 'redux-devtools-extension'
export default createStore(allReducer,composeWithDevTools(applyMiddleware(thunk)))
```

---





## 8 React扩展

### 8.1 ==setState两种写法==

- 对象式：`setState(stateChange, [callback])`
  - stateChange为状态改变对象(该对象可以体现出状态的更改)
  - callback是可选的回调函数, 它在状态更新完毕、界面也更新后(render调用后)才被调用
- 函数式：`setState(updater, [callback])`
  - `updater`为返回stateChange对象的函数
  - `updater`可以接收到==state==和==props==
  - callback是可选的回调函数, 它在状态更新、界面也更新后(render调用后)才被调用。

```jsx
const { count } = this.state
this.setState({count: count + 1}, () => console.log(this.state.count))
this.setState(state => ({ count: state.count + 1 }))
```

- 总结
  - 对象式的setState是函数式的setState的简写方式(语法糖)
  - 使用原则
    - 如果新状态不依赖于原状态 => ==使用对象方式==
    - ==如果新状态依赖于原状态 ===> 使用函数方式
    - 如果需要在setState()执行后获取最新的状态数据, 要在第二个callback函数中读取

---



### 8.2 ==layLoad==

- 路由组件的==懒加载==

```jsx
import { Lazy, Suspense } from 'react'
//1.通过React的lazy函数配合import()函数动态加载路由组件 ===> 路由组件代码会被分开打包
const Login = lazy(()=>import('@/pages/Login'))

//2.通过<Suspense>指定在加载得到路由打包文件前显示一个自定义loading界面
<Suspense fallback={<h1>loading.....</h1>}>
  <Switch>
    <Route path="/xxx" component={Xxxx}/>
    <Redirect to="/login"/>
  </Switch>
</Suspense>
```

- Loading组件==不使用懒加载==

---



### 8.3 ==Hooks==

#### 8.3.1 State Hook

- State Hook让函数组件也可以有state状态, 并进行状态数据的读写操作
- 语法: `const [xxx, setXxx] = React.useState(initValue)` 
- `useState()`说明:
  - 参数: 第一次初始化指定的值在内部作缓存
  - 返回值: 包含2个元素的数组, 第1个为内部当前状态值, 第2个为更新状态值的函数
- `setXxx()`2种写法:
  - setXxx(newValue): 参数为非函数值, 直接指定新的状态值, 内部用其覆盖原来的状态值
  - setXxx(value => newValue): 参数为函数, ==接收原本的状态值==, 返回新的状态值, 内部用其覆盖原来的状态值

```jsx
export default function Demo() {
  const [count, setCount] = React.useState(0)
  const [name, setName] = React.useState('jack')
  const add = () => {
    setCount(count + 1)
  }
  const changeName = () => {
    setName(name => name + '=')
  }
  return (
    <div>
      <h2>当前求和为：{count}</h2>
      <h2>当前姓名为：{name}</h2>
      <button onClick={add}>加1</button>
      <button onClick={changeName}>改名</button>
    </div>
  )
}
```

---



#### 8.3.2 Effect Hook

- Effect Hook 可以让你在函数组件中执行副作用操作(用于模拟类组件中的生命周期钩子)
- React中的副作用操作:
  - 发ajax请求数据获取
  - 设置订阅 / 启动定时器
  - 手动更改真实DOM
- 语法和说明: 

```jsx
useEffect(() => { 
  // 在此可以执行任何带副作用操作
  return () => { // 在组件卸载前执行
    // 在此做一些收尾工作, 比如清除定时器/取消订阅等
  }
}, [stateValue]) // 如果指定的是[], 回调函数只会在第一次render()后执行
// stateValue相当于一个监听器，如果指定某个state状态，则该状态发生更新时也会调用render
```

- 可以把 useEffect Hook 看做如下三个函数的组合：
  - componentDidMount()
  - componentDidUpdate()
  - componentWillUnmount() 

```jsx
React.useEffect(() => {
  const timer = setInterval(() => setCount(count => count + 1), 1000)
  return () => {
    clearInterval(timer)
  }
}, [])
```

---



#### 8.3.3 Ref hook

- Ref Hook可以在函数组件中存储/查找组件内的标签或任意其它数据
- 语法：`const refContainer = useRef()`
- 作用：保存标签对象,功能与``React.createRef()``一样

```jsx
const myRef = React.useRef()
const show = () => {
  console.log(myRef.current.value)
}

<input type="text" ref={myRef} />
<button onClick={show}>显示输入数据</button>
```

---



#### 8.3.4 自定义 hook

- 自定义 Hook 是一个函数，其名称以 “`use`” 开头，函数内部可以调用其他的 Hook
- 自定义 Hook 必须以 “`use`” 开头。这个约定非常重要。不遵循的话，由于无法判断某个函数是否包含对其内部 Hook 的调用，React 将无法自动检查你的 Hook 是否违反了 Hook 的规则
- 在两个组件中使用相同的 Hook ==不会共享 state== 。自定义 Hook 是一种重用*状态逻辑*的机制(例如设置为订阅并存储当前值)，所以每次使用自定义 Hook 时，其中的所有 state 和副作用都是完全隔离的。

---





### 8.4 Fragment

- 用于JSX语法规则，虚拟DOM必须有一个根标签，使用Fragment，该结构会在React解析时被==丢弃==

- 该组件只能有`key`和`children`属性
- 也可以使用空标签`<></>`，但是==空标签不能用任何标签属性==

```jsx
return (
  <Fragment key={1}>
    <input type="text" ref={myRef} />
  </Fragment>
)
// 空标签使用
return (
  <>
    <input type="text" ref={myRef} />
  </t>
)
```

---



### 8.5 context

- 理解：一种组件间通信方式，常用于==【祖组件】==与==【后代组件】==之间的通信
- 使用

1. 创建Context容器对象：

```jsx
const MyContext = React.createContext()
```

2. 渲染子组时，外面包裹`MyContext.Provider`, 通过`value`属性给后代组件传递数据：

```jsx
<MyContext.Provider value={数据}>
		子组件
</MyContext.Provider>
```

3. 后代组件读取数据：

```jsx
//第一种方式:仅适用于类组件 
static contextType = xxxContext  // 声明接收context
this.context // 读取context中的value数据
  
//第二种方式: 函数组件与类组件都可以
<xxxContext.Consumer>
  {
    value => ( // value就是context中的value数据
    要显示的内容
    )
  }
</xxxContext.Consumer>
```
- 示例

```jsx
const MyContext = React.createContext()
<MyContext.Provider value={{count: this.state.count}}>
  <B/>
</MyContext.Provider>
  
class C extends Component {
  static contextType = MyContext
  render() {
    return (
      <div>
        <h2>我是C组件</h2>
        <h3>我从A组件接收到的数据是：{this.context.count}</h3>
      </div>
    )
  }
}

function C () {
  return (
    <div>
      <h2>我是C组件</h2>
      <MyContext.Consumer>
        {
          value => <h3>我从A组件接收到的数据是：{value.count}</h3>
        }
      </MyContext.Consumer>
    </div>
  )
}
```

---



### 8.6 组件优化

- `PureComponent`：重写了``shouldComponentUpdate()``, 只有`state`或`props`数据有变化才返回`true`
- 注意：
  - 只是进行state和props数据的浅比较, 如果只是数据对象内部数据变了, 返回false
  - ==不要直接修改state数据, 而是要产生**新数据**==
- 项目中一般使用PureComponent来优化

---



### 8.7 ==render props(插槽)==

- 作用：向组件内部==动态传入==带内容的结构(标签)——==插槽==

- 使用 children props

```jsx
<A>
  <B>xxxx</B>
</A>
{this.props.children} // 用于渲染B组件
问题: 如果B组件需要A组件内的数据, ==> 做不到 
```

- 使用 render props

```jsx
<A render={(data) => <B data={data}/>}></A>
A组件: {this.props.render(内部state数据)}
B组件: 读取A组件传入的数据显示 {this.props.data} 
```

```jsx
export default class Demo extends Component {
  render() {
    return (
      <div>
        <A render={count => <B count={count} />} />
      </div>
    )
  }
}

class A extends Component {
  state = {
    count: 10
  }
  render() {
    return (
      <div>
        <h2>我是父组件</h2>
        // 类似插槽
        {this.props.render(this.state.count)}
      </div>
    )
  }
}

class B extends Component {
  render() {
    return (
      <div>
        <h3>我是子组件</h3>
        <h3>父组件传递的值：{this.props.count}</h3> // 10
      </div>
    )
  }
}
```

---



### 8.8 错误边界

- 错误边界(Error boundary)：用来捕获后代组件错误，渲染出备用页面
- 只能捕获后代==组件生命周期==产生的错误，不能捕获自己组件产生的错误和其他组件在合成事件、定时器中产生的错误
- **自 React 16 起，任何未被错误边界捕获的错误将会导致整个 React 组件树被卸载**
- 使用方式：`getDerivedStateFromError`配合`componentDidCatch`

```jsx
// 生命周期函数，一旦后台组件报错，就会触发
static getDerivedStateFromError(error) {
    console.log(error);
    // 在render之前触发
    // 返回新的state
    return {
        hasError: true
    }
}

componentDidCatch(error, info) {
    // 统计页面的错误。发送请求发送到后台去
    console.log(error, info);
}
```

---



### 8.9 ==组件间通信方式总结==

- 组件间的关系
  - 父子组件
  - 兄弟组件（非嵌套组件）
  - 祖孙组件（跨级组件）

- 几种通信方式：

		1.props：
			(1).children props
			(2).render props
		2.消息订阅-发布：
			pubs-sub、event等等
		3.集中式管理：
			redux、dva等等
		4.conText:
			生产者-消费者模式

- 比较好的搭配方式：

		父子组件：props
		兄弟组件：消息订阅-发布、集中式管理
		祖孙组件(跨级组件)：消息订阅-发布、集中式管理、conText(开发用的少，封装插件用的多)

