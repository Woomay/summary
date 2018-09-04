## React中的state和props的区别

### props

在React中核心的思想是组件化，页面被切分成一些独立的，可复用的组件。组件从概念上看就像一个函数，可以接受一个参数作为输入值，而这个参数就是`props`,所以可以把`props` 理解为从组件外部传入组件内部的数据，由于React是单向数据流，所以`props` 基本上也就是父组件向子组件传递的数据。实际上，`props` 就是“properities”的缩写。

##### Example

- 在父组件`Board` 中

```ruby
import Square from './Square'
class Board extends React.Component {
	renderSquare(i) {
    	return <Square value={i} />
	}
}
```
- 在子组件`Square` 中

``` ruby
class Square extends React.Component {
	render() {
        <button>{this.props.value}</button>
	}
}
```

在子组件内部是通过`this.props` 来获取传递到该组建的所有数据，它是一个对象，包含了所有你对这个组件的配置。

##### props的只读性

无论是使用函数或是类来声明一个组件，都不能修改它自己的`props` 。当一个组件被实例化之后，它的`props` 是只读的，不可改变的。只有通过父组件重新渲染的方式才可以吧新的`props` 传入组件中。

##### 默认参数

在组件中，我们最好为`props` 中的参数设置一个`defaultProps` ,并且制定它的类型。比如这样：

``` ruby
Square.defaultProps = {
    value: 'hello world'
};
Square.propTypes = {
    value: PropTypes.string
}
```

##### 总结

`props` 是一个从外部传进组件的参数或者也可以是来自组件内部`defaultProps`，主要作用就是从父组件向自组建传递数据，它具有可读性和不变性，只能通过外部组件主动传入新的`props` 来重新渲染子组件，否则自组建的`props` 以及展现形式不会改变。

### state

一个组件的显示形态可以由数据状态和外部参数所决定，外部参数也就是`props` ,而数据状态就是`state` 。一般情况下，组件是没有`state` 的，当组件需要跟踪渲染信息的时候，就要用到`state` 。`state` 由组件本身创造，更新和使用。

##### Example

``` ruby
export default class ItemList extends React.Component {
	constructor() {
        super()
	}
    this.state = {
        itemList: '一些数据'
	}
    render() {
		return (
            {this.state.itemList}
        )
    }
}
```

在组件初始化的时候，通过`this.state` 给组件设定一个初始的`state` ，在第一次组件`render` 的时候就会用这个数据来渲染组件。

##### setState

``` ruby
setState(updater,[callback])
这样写其实不规范：
this.setState({count: this.state.count + 1})
推荐下列写法：
this.setState((prevState,props) => {
    return {count: prevState.count + 1}
}
```

`state` 不同于`props` 的一点是，`state` 可以被改变。但是必须通过`this.setState()` 方法才能修改`state` 。

比如，我们经常会通过异步操作来获取数据，我们需要在`componentDidMount` 阶段来执行异步操作：

``` ruby
componentDidMount() {
    fetch('url')
    	.then(res => res.res.json())
    .then((data) => {
        this.setState({ itemList: item });
        })
}
```

当数据获取完成后，通过`this.setState()` 来修改数据状态。当我们在调用`this.setState` 方法时，React会更新组件的数据状态`state` ,并且重新调用`render` 方法，对组件重新渲染。

**注意：通过`this.state =` 来初始化`state` ,使用`this.setState` 来修改 `state`**

`setState` 接收一个对象或者函数作为第一个参数，只需要传入需要更新的部分即可，不需要传入整个对象。比如：

``` ruby
export default class ItemList extends React.Component {
	constructor() {
        super();
        this.state = {
            name: 'axuebin',
            age: 25
        }
	}
    componentDidMount() {
    	this.setState({age: 18})    
    }
}
```

在执行完`setState` 之后的`state` 应该是`{name: 'axuebin',age:18}` 。

`setState` 还可以接受第二个参数，它是一个函数，会在`setState` 调用完成并且组件开始重新渲染时被调用，可以用来监听渲染是否完成：

``` ruby
this.setState({ name: 'loyi' },() => console.log('setState finished'))
```

##### 总结

`state` 的主要作用是用于组件保存、控制以及修改自己的状态，它只能在`constructor` 中初始化，它算是组件的私有属性，不能通过外部访问和修改，只能通过组件内部的`this.setState` 来修改，修改`state` 属性会导致组件的重新渲染。

### 区别

1. `state` 是组件自己创造，更新和使用，用来管理数据，控制自己的状态，可以改变；
2. `props` 是外部传入的数据参数，不可以改变；
3. 没有`state` 的组件叫做无状态组件，有`state` 的叫做有状态组件；多用`props` ,即多写无状态组件。

​