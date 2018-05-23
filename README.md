# react-learn
react学习笔记

## react 脚手架 create-react-app
`新建项目`

npm install -g create-react-app  
create-react-app [项目名]  

`启动项目`

cd [项目名]  
npm start  

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
- 1.组件类中只能包含一个顶层标签，否则会报错
- 2.添加组件属性，class 属性需要写成 className, for 属性需要写成 htmlFor
因为 class, for 是 javascript 的保留字
```js
<label className="label" htmlFor="username"></label>
```
3.组件名首字母必须大写

## props
- this.props 对象的属性与组件的属性一一对应
- this.props.children 表示组件的所有的子节点
```js
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
- this.props.children 可以被传入任务数据，包括回调函数
```js
class Repeat extends Component {
    render () {
        let items = [];
        for (let i = 0; i < this.props.numtimes; i++) {
            items.push(this.props.children(i));
        }
        return <ul>{ items }</ul>
    }
}

class ListOfTenThings extends Component {
    render () {
        return (
            <Repeat numtimes={ 10 }>
                { (index) => <li key={ index }>this is item { index } in the list</li> }
            </Repeat>
        )
    }
}
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

注意：
- 用 class 定义的类中 propTypes 要移到外部来
- 从 React v15.5 开始 ，React.PropTypes 助手函数已被弃用，我们建议使用 prop-types 库 来定义contextTypes。
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
    name: PropTypes.string
}
var data = 123;

ReactDOM.render(
  <MyTitle title={data} />,
  document.body
);
```

`默认的prop值`
propTypes 的类型检测是在defaultProps 解析之后发生的，因此也会对默认属性 defaultProps 进行类型检测。
```js
MyTitle.defaultProps = {
    name: 'Stranger'
};
```

`不同的验证器：`
```js
import PropTypes from 'prop-types';

MyComponent.propTypes = {
  // 你可以声明一个 prop 是一个特定的 JS 原始类型。 
  // 默认情况下，这些都是可选的。
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // 任何东西都可以被渲染:numbers, strings, elements,或者是包含这些类型的数组(或者是片段)。
  optionalNode: PropTypes.node,

  // 一个 React 元素。
  optionalElement: PropTypes.element,

  // 你也可以声明一个 prop 是类的一个实例。 
  // 使用 JS 的 instanceof 运算符。
  optionalMessage: PropTypes.instanceOf(Message),

  // 你可以声明 prop 是特定的值，类似于枚举
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // 一个对象可以是多种类型其中之一
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // 一个某种类型的数组
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // 属性值为某种类型的对象
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // 一个特定形式的对象
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),

  // 你可以使用 `isRequired' 链接上述任何一个，以确保在没有提供 prop 的情况下显示警告。
  requiredFunc: PropTypes.func.isRequired,

  // 任何数据类型的值
  requiredAny: PropTypes.any.isRequired,

  // 你也可以声明自定义的验证器。如果验证失败返回 Error 对象。不要使用 `console.warn` 或者 throw ，
  // 因为这不会在 `oneOfType` 类型的验证器中起作用。
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // 也可以声明`arrayOf`和`objectOf`类型的验证器，如果验证失败需要返回Error对象。
  // 会在数组或者对象的每一个元素上调用验证器。验证器的前两个参数分别是数组或者对象本身，
  // 以及当前元素的键值。
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};
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
`虚拟 DOM`
- 组件只是存在于内存中的一种数据结构，叫做虚拟 DOM，只有插入文档后，才会变成
- 真实的 DOM。所有的 DOM 变动，都先在虚拟 DOM 上发生，然后再将实际发生变动的部分，反映在真实 DOM上，这叫做 DOM diff 算法，极大的提高网页性能。

`获取 真实DOM - Ref`
在虚拟DOM上添加 ref 属性，有两种方法可以获取到真实DOM
- this.refs.[refName] 就会返回真实的 DOM 节点
```js
class MyInput extends Component {

    handleClick = () => {
        this.refs.textInput.focus();
    }

    render () {
        return (
            <div>
                <input ref="textInput" />
                <button onClick={ () => this.handleClick() }>Submit</button>
            </div>
            
        )
    }
}
```
- ref 属性接受回调函数，ref 回调接受底层的DOM元素作为参数，组件 装载(mounted) 或者 卸载(unmounted) 之后，回调函数会立即执行，DOM元素作为的组件属性来获取。
```js
<input ref={ (input) => { this.textInput = input } } />
```

注意：
- 只有虚拟DOM插入到文档后，才能使用这个属性
- 不能在函数式组件上使用 ref 属性，因为它们没有实例
```js
function MyFunctionalComponent() {
  return <input />;
}

class Parent extends React.Component {
  render() {
    // 这里 *不会* 执行！
    return (
      <MyFunctionalComponent
        ref={(input) => { this.textInput = input; }} />
    );
  }
}
```

### [React创建组件的三种方式及其区别](http://www.cnblogs.com/wonyun/p/5930333.html)

### 参照：
- React 入门实例教程：http://www.ruanyifeng.com/blog/2015/03/react.html
- React DOM diff 算法：https://calendar.perfplanet.com/2013/diff/