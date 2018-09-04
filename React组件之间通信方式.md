

## React组件之间通信方式

由于React单向数据流动的特点，对于刚入门学习React的同学或许会感到很疑惑，React组件之间到底是怎么进行通信的。以下总结了组件之间需要通信的几种情况：

- 父组件向子组件通信
- 子组件向父组件通信
- 兄弟组件之间通信
- 跨级组件通信

### 1.父组件向子组件通信

在React中的所有通信关系中，父组件向子组件通信是最常见。父组件通过`props` 向子组件传递需要的信息。

Child.jsx

```ruby
import React from 'react';
import PropTypes from 'prop-types';
export default class Child extends React.Component {
    render() {
        return <h1>Hello,{name}</h1>
    }	
}
Child.propTypes = {
	name: PropTypes.string.isRequired
}
```

Parent.jsx

```ruby
import React from 'react';
import Child from './Child';
class Parent extends React.Component {
	render() {
        return(
        	<div>
                <Child name='Pipi' />
            </div>    
        );
	}
}
```

### 2.子组件向父组件通信
* #####  回调函数

* ##### 自定义事件机制


##### 回调函数

实现在子组件中点击隐藏按钮可以将自身隐藏的功能

List.jsx

```ruby
import React from 'react';
import PropTypes from 'prop-types';
export default class List extends React.Component {
	static propTyps = {
        hideComponent: PropTypes.func.isRequired
	}
    render() {
    	return (
        	<div>
                我是list
            	<button onClick={this.props.hideComponent}>隐藏List组件</button>
            </div>    
        )    
    }
}
```

App.jsx

```ruby
import React from 'react';
import List from './List';
export default class App extends React.Component {
    constructor(props) {
     	super(props);
        this.state = {
        	isShowList: false    
        }
    }
    showComponent = () => {
    	this.setState({ isShowList: true })    
    }
    hideComponent = () => {
    	this.setState({ isShowList: false })    
    }
    render() {
    	return (
            <div>
                <button onClick={this.showComponent}>显示组件List</button>
                { this.state.isShowList ? 
                   	<List hideComponent={this.hideComponent} /> 
                  : null
                }
            </div>
        );   
    }  
}
```

由上例子可以看出React中的回调函数和传统回调函数的实现方法一致，一般`setState` 会与回调函数成对出现，用来转换内部函数的状态。

##### 自定义事件机制

在componentDidMount事件中,如果组件挂载完成,再订阅事件;在组件卸载的时候,在componentWillUnmount事件中取消事件的订阅;
以常用的发布/订阅模式举例,借用Node.js Events模块的浏览器版实现。

###### 使用自定义事件的方式

下面例子中的组件关系: List1和List2没有任何嵌套关系,App是他们的父组件;

实现这样一个功能: 点击List2中的一个按钮,改变List1中的信息显示

首先，需要在项目中安装events包：

```
npm install events --save
```

在src下创建一个util目录里面键一个events.js

```
import { EventEmitter } from 'events';
export default new EventEmitter();
```

List1.js

```ruby
import React from 'react';
import emitter from '../util/events';
export default class List1 extends React.Component {
	constructor(props) {
        super(props);
        this.state = {
        	message: 'List1'    
        }
	}
    componentDidMount() {
        //组件装载完成以后声明一个自定义事件
        this.eventEmitter = emitter.addListener('changeMessage',(message) => {
        	this.setState({ message });    
        })
	}
    componentWillUnmount() {
    	emitter.removeListener(this.eventEmitter);    
    }
    render() {
    	return (
            <div>
                {this.state.message}
            </div>
        )    
    }
}
```

List2.js

```ruby
import React from 'react';
import emitter from '../util/events';
export default class List2 extends React.Component {
    handleClick = (message) => {
        emitter.emit('changeMessage',message)
	}
    render() {
    	return (
            <div>
                <button onClick={this.handleClick.bind(this,'List2')}>
                	点击我改变List1组件中的显示信息				
            	</button>
            </div>
        )    
    }
}
```

App.js

```ruby
import React from 'react';
import List1 from './components/List1';
import List2 from './components/List2';
export default class App extends React.Component {
	render() {
		return (
            <div>
                <List1 />
                <List2 />
            </div>
        )
    }
}
```

自定义事件是典型的发布订阅模式，通过向事件对象上添加监听器和触发事件来实现组件之间的通信。

### 3.兄弟组件通信

当两个组件不是父子关系，但有相同的父组件时，将这两个组件成为兄弟组件。兄弟组件不能直接相互传递数据，此时可以子组件通过回调函数的方法将需要传递数据挂载在父组件中，由两个组件共享；如果组件需要数据渲染，则由父组件通过props传递给该组件；如果组件需要改变数据，则父组件传递一个改变数据的回调函数给该组件，并在对应事件中调用。这其实就是一种`状态提升` 的做法。

如下图所示的组件关系：ComponentInput中输入的信息，需要放在ComponentList中进行展示。

![state](D:\Markdown\images\state.png)

父组件`CommentApp` 通过`props` 给子组件`CommentInput` 传入一个回调函数。当用户点击发布按钮的时候，`ComponentInput` 调用`props` 中的回调函数并且将`state` 传入该函数即可。

CommentInput.js

```ruby
 export default class CommentInput extends React.Component {
    constructor() {
        super()
        this.state = {
            username: '',
            content: ''
		}
	}
    handleUernameChange(e) {
        this.setState({ username: e.target.value })
	}
    handleContentChange(e) {
         this.setState({ content: e.target.value })
    }
    hanleSubmit() {
        if (this.props.onSubmit) {
        	const {username,content} = this.state
            this.props.onSubmit({username,content})
        }   
        this.setState({content: ''})
    }
	render() {
        return (
            <div>
                <div>
                    <span>用户名：</span>
                    <input 
            			value={this.state.username} 
            			onChange={this.handleUsernameChange.bind(this)}
            		/>
                <div>
                <div>
                    <span>评论内容：</span>
                    <textarea 
            			value={this.state.content} 
            			onChange={this.handleContentChange.bind(this)}
            		/>
                <div>
                <div>
                	<button onClick={this.handleSubmit.bind(this)}>发布</button>
                </div>
            <div>
        )
	}    
}
```

CommentApp.js

```ruby
class CommentApp extends React.Component {
    handleSubmitComment(comment) {
        console.log(comment)
	}
	render() {
        <div>
            <CommentInput onSubmit={this.handleSubmitComment.bind(this)} />
            <CommentList />
        </div>
	} 
}
```

在 `CommentApp` 中给 `CommentInput` 传入一个 `onSubmit` 属性，这个属性值是 `CommentApp` 自己的一个方法 `handleSubmitComment`。这样 `CommentInput` 就可以调用 `this.props.onSubmit(…)` 把数据传给 `CommenApp`。

CommentList.js

```ruby
import React, { Component } from 'react'
import Comment from './Comment'

class CommentList extends Component {
  render() {
    return (
      <div>
        {this.props.comments.map((comment, i) => <Comment comment={comment} key={i} />)}
      </div>
    )
  }
}

export default CommentList
```

就这样就实现了兄弟组件之间的通信。

### 4.跨级组件通信

如果组件之间的层级关系嵌套太深，通过`Lifting State Up` 还是太繁琐的话，这时候就要用到`context` 的用法了。在对应的场景中，`context` 可以缩短父组件到子组件属性传递路径。如下例子：

Container.jsx

```ruby
import Parent from './Parent';
import ChildOne from '../components/ChildOne';
import ChildTwo from '../components/ChildTwo';
export default class Container extends React.Component {
    constructor(props) {
    	super(props);
        this.state = {
        	value: ''    
        }
    }  
    changeValue = value => {
    	this.setState({ value })    
    }
    getChildContext() {
        return {
        	value: this.state.value,
            changeValue: this.changeValue
        }    
    }
    render() {
    	return(
            <div>
                <Parent>
                	<ChildOne />
                </Parent>
                <Parent>
                	<ChildTwo />
                </Parent>
            </div>
        )    
    }
}
Container.childContextTypes = {
	value: PropTypes.string,
    changeValue: PropTypes.func
}
```

Parent.jsx

```ruby
import React from "react"
const Parent = (props) => (
    <div {...props} />
)
export default Parent
```

ChildOne.jsx

```ruby
export default class ChildOne extends React.Component {
    handleChange = (e) => {
        const {changeValue} = this.context
        changeValue(e.target.value)
	}
    render() {
    	return(
            <div>
                子组件1
            	<input onChange={this.hanleChange} />
            </div>
        )    
    }
}
ChildOne.contextTypes = {
	changeValue: PropTypes.func      
}
```

ChildTwo.jsx

```ruby
export default class ChildTwo extends React.Component {
	render() {
        return (   
            <div>
                子组件2
            	<p>this.context.value</p>
            </div>
       	)
	}
}
ChildTwo.contextTypes = {
   value: PropTypes.string
}
```

在 **Container.childContextTypes** 中进行接口的声明，通过 `getChildContext` 返回更新后的state，在 **Child.contextTypes** 中声明要获取的接口，这样在子组件内部就能通过 `this.context` 获取到。通过 Context 这样一个中间对象，ChildOne 和 ChildTwo 就可以相互通信了。

注意，React Context 也有一些**局限性**：

- 在组件树中，如果中间某一个组件 **ShouldComponentUpdate returning false** 了，会阻碍 context 的正常传值，导致子组件无法获取更新。

- 组件本身 **extends React.PureComponent** 也会阻碍 context 的更新。

- `context` 打破了组件和组件之间通过 `props` 传递数据的规范，极大地增强了组件之间的耦合性。而且，就如全局变量一样，**`context` 里面的数据能被随意接触就能被随意修改**，每个组件都能够改 `context` 里面的内容会导致程序的运行不可预料。

  5.父组件调子组件的方法

  ```
  import React, {Component} from 'react';
  export default class Parent extends Component {
      render() {
          return(
              <div>
                  <Child onRef={this.onRef} />
                  <button onClick={this.click} >click</button>
              </div>
          )
      }
      onRef = (ref) => {
          this.child = ref
      }
      click = (e) => {
          this.child.myName()
      }
  }
  class Child extends Component {    componentDidMount(){
          //必须在这里声明，所以 ref 回调可以引用它
          this.props.onRef(this)
      }
      myName = () => alert('我的名字')
      render() {
          return ('lalalala')
      }
  }
  ```

