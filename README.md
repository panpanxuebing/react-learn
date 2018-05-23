# react-learn
react学习笔记

## react 脚手架 create-react-app
`新建项目`
```js
npm install -g create-react-app
```
create-react-app [项目名]
`启动项目`
```js
cd [项目名]
npm start
```

## ReactDOM.render()
ReactDOM.render 是 React 的最基本方法，用于将模板转为 HTML 语言，并插入指定的 DOM 节点。
```js
ReactDOM.render(
    <div>Hello React</div>,
    document.getElementById('root')
);
```
## JSX 语法
JSX 语法中 HTML 语言可以直接写在 JavaScript 语言之中，不加任何引号，允许 HTML 与 Javascript 混写
基本规则：遇到 HTML 标签（以 < 开头），就用 HTML 规则解析；遇到代码块（以 { 开头），就用 JavaScript 规则解析
```js
const names = ['Love', 'world', 'Peace'];

ReactDOM.render(
    <div>
        {
            names.map(name => <div key={ name }>hello { name }</div>)
        }
    </div>,
    document.getElementById('root')
);
```
注意: 在 JSX 中不能使用 if else 语句，但可以用 三元运算 来替换

## 组件
组件可以将UI切分成一些的独立的、可复用的部件，这样你就只需专注于构建每一个单独的部件

`使用 javascript 定义组件：`
```js
function Welcome (props) {
    return <h3>hello { props.name }</h3>
}
```

`使用 ES6 class 定义组件：（推荐）`
```js
class Welcome extends React.Component {
    render () {
        return (
            <h3>hello { this.props.name }</h3>
        )
    }
}
```
注意：
1.组件类中只能包含一个顶层标签，否则会报错
2.添加组件属性，class 属性需要写成 className, for 属性需要写成 htmlFor
因为 class, for 是 javascript 的保留字
```
<label className="label" htmlFor="username"></label>
```
3.组件名首字母必须大写

## props
- this.props 对象的属性与组件的属性一一对应
- this.props.children 表示组件的所有的子节点
```
class Welcome extends Component {
    render () {
        return (
            <div>
                { this.props.children.map((child, i) => <li key={ i }>{ child }</li>) }
            </div>
        )
    }
}

ReactDOM.render(
    <Welcome>
        <span>A</span>
        <span>B</span>
    </Welcome>,
    document.getElementById('root')
);
```
- props 的只读性
所有的React组件必须像纯函数那样使用它们的props。
纯函数概念：
```js
function sum(a, b) {
  return a + b;
}
```
类似于上面的这种函数称为“纯函数”，它没有改变它自己的输入值，当传入的值相同时，总是会返回相同的结果

## PropTypes
组件的 PropTypes 属性用来验证别人使用组件时，提供的参数是否符合要求。


注意：用 class 定义的类中 propTypes 要移到外部来
```js
import PropTypes from 'prop-types'
class MyTitle extends Component {
    render () {
        return (
            <h1>{ this.props.title }</h1>
        )
    }
}

MyTitle.propTypes = {
    // 可以声明 prop 为指定的 JS 基本数据类型，默认情况，这些数据是可选的
    optionalArray: React.PropTypes.array,
    optionalBool: React.PropTypes.bool,
    optionalFunc: React.PropTypes.func,
    optionalNumber: React.PropTypes.number,
    optionalObject: React.PropTypes.object,
    optionalString: React.PropTypes.string,
 
    // 可以被渲染的对象 numbers, strings, elements 或 array
    optionalNode: React.PropTypes.node,
 
    //  React 元素
    optionalElement: React.PropTypes.element,
 
    // 用 JS 的 instanceof 操作符声明 prop 为类的实例。
    optionalMessage: React.PropTypes.instanceOf(Message),
 
    // 用 enum 来限制 prop 只接受指定的值。
    optionalEnum: React.PropTypes.oneOf(['News', 'Photos']),
 
    // 可以是多个对象类型中的一个
    optionalUnion: React.PropTypes.oneOfType([
      React.PropTypes.string,
      React.PropTypes.number,
      React.PropTypes.instanceOf(Message)
    ]),
 
    // 指定类型组成的数组
    optionalArrayOf: React.PropTypes.arrayOf(React.PropTypes.number),
 
    // 指定类型的属性构成的对象
    optionalObjectOf: React.PropTypes.objectOf(React.PropTypes.number),
 
    // 特定 shape 参数的对象
    optionalObjectWithShape: React.PropTypes.shape({
      color: React.PropTypes.string,
      fontSize: React.PropTypes.number
    }),
 
    // 任意类型加上 `isRequired` 来使 prop 不可空。
    requiredFunc: React.PropTypes.func.isRequired,
 
    // 不可空的任意类型
    requiredAny: React.PropTypes.any.isRequired,
 
    // 自定义验证器。如果验证失败需要返回一个 Error 对象。不要直接使用 `console.warn` 或抛异常，因为这样 `oneOfType` 会失效。
    customProp: function(props, propName, componentName) {
        if (!/matchme/.test(props[propName])) {
            return new Error('Validation failed!');
        }
    }
}


var data = 123;

ReactDOM.render(
  <MyTitle title={data} />,
  document.body
);
```

## state
组件就是一个状态机，有初始状态，然后用户互动，状态改变，触发重新渲染UI
`初始状态`
```
this.state = {}
```
`设置状态`
```
this.setState({});
```
注意：
- state 和 props 区分：
- this.props 表示那些一旦定义，就不再改变的特性
- this.state 是会随着用户互动而产生变化的特性

## 组件的生命周期
组件的生命周期主要分成三个状态：
- Mounting： 已插入真实DOM
- Updating: 正在被重新渲染
- Unmounting: 已移除真实DOM

Mounting 阶段：
- constructor(props) 该方法在组件加载前调用。当组件作为React.Component的子类实现时需要在其他声明之前先调用super(props)。否则，this.props在构造器中会是undefined，这会导致代码出现bug。
- componentWillMount() 在渲染前调用，在客户端也在服务端
- render() 返回虚拟DOM, 在渲染为真实DOM，不会和浏览器发生交互
- componentDidMount() 在第一次渲染后调用，只在客户端。在该方法内设置状态会导致重渲染。可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异部操作阻塞UI)。

Updating 阶段

- componentWillReceiveProps(nextProps) 在组件接收到一个新的props时调用。在初始化render的时候不会被调用。
- shouldComponentUpdate 返回一个布尔值。在组件接收到新的props或state时调用。在初始化或使用forceUpdate时不会被调用
- componentWillUpdate(object nextProps, object nextState) 在组件接收到新的props或者state时被调用。在初始化时不会被调用
- componentDidUpdate(object prevProps, object prevState) 在组件更新完后立即调用。在初始化时不会被调用

Unmounting 阶段
- componentWillUnmount() 在组件从DOM中移除时被调用

## 获取真实的DOM节点
虚拟 DOM
- 组件只是存在于内存中的一种数据结构，叫做虚拟 DOM，只有插入文档后，才会变成
- 真实的 DOM。所有的 DOM 变动，都先在虚拟 DOM 上发生，然后再将实际发生变动的部分，反映在真实 DOM上，这叫做 DOM diff 算法，极大的提高网页性能。

获取 真实DOM
- 现在虚拟DOM上添加 ref 属性，然后通过 this.refs.[refName] 就会返回真实的 DOM 节点

注意：
- 只有虚拟DOM插入到文档后，才能使用这个属性

## 参照：
- React 入门实例教程：http://www.ruanyifeng.com/blog/2015/03/react.html
- React DOM diff 算法：https://calendar.perfplanet.com/2013/diff/
