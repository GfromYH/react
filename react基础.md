## React Fiber(React)

React Fiber指的是react 版本16之后，因为加入了**事件优先级**，所以称版本为16之后的为**React Fiber**

安装react脚手架工具create-react-app

~~~javascript
cnpm install -g create-react-app //首先安装全局的

create-react-app my-app

npx create-react-app my-app
cd my-app
cnpm start

两种写法
Fragment相当于一个占位符，既满足return需要一个跟节点，又满足在浏览器上不显示标签
import React,{Fragment，Component} from 'react';

class App extends Component {
    render(){
      return (
        <Fragment>
          <input type="text"/> <button>提交</button>
          <ul>
            <li>学英语</li>
            <li>养小狗</li>
          </ul>
        </Fragment>
  		);
    }
}

export default App;

2.
import React,{Fragment} from 'react';

function App() {
  return (
    <Fragment>
      <input type="text"/> <button>提交</button>
      <ul>
        <li>学英语</li>
        <li>养小狗</li>
      </ul>
    </Fragment>
  );
}

export default App;
~~~

react中有jsx语法例如html标签引入，一般使用jsx语法需要在对应的文件下**import React from ‘react’**

+ jsx

1. jsx是在js文件中写html标签

2. jsx如果要是用自己定义的html标签，首字母必须大写

3. jsx在js中写标签，不需要单引号或双引号，可以直接写标签

4. jsx如果要写js表达式外面需要套个{}

5. 注释{/* XXX */}

6. label中的for要用htmlFor来表示，class要用className来表示，如果input内容不需要转义需要用

   ~~~javascript
   dangerouslySetInnerHTML={{__html:item}} //item自己定义内容一般都是用来显示循环的li标签
   
   ~~~




------

### 事件响应式以及事件绑定

+ 在Vue中通过数据劫持的方法，给每个属性添加getter与setter方法来监听数据改变，一旦数据发生改变则通知watcher，然后watcher调用update（）方法来更新界面

先通过代码来观察变化

~~~javascript
import React,{Fragment,Component} from 'react';

class App extends Component{
    constructor(props){
        super(props);
        this.state={   
            inputValue:'',
            list:[]
        }
    }
      render() {
        return (
            <Fragment>
              <input
                  value={this.state.inputValue}
                  onChange={this.handleInputChange}
                  type="text"/>
              <button>提交</button>
              <ul>
                <li>学英语</li>
                <li>养小狗</li>
              </ul>
            </Fragment>
        );
    }
    handleInputChange=(e)=>{
        this.setState({
            inputValue:e.target.value
        })
    }

}

export default App;

~~~

1. 首先如果在jsx中绑定事件舰艇函数，必须是驼峰原则

2. 如果要初始化数据，需要先继承父类Component，存放数据是在state中的

3. ~~~javascript
       handleInputChange=(e)=>{ //通过箭头函数防止this指向改变变成undefined
           this.setState({ //当要修改state中的数据时不能直接this.state.xxx而是需要在this.setState中改变，如果不是修改的话可以直接调用this.state.xxx来读取内容
               inputValue:e.target.value
           })
       }
   ~~~



------

### 事件传参

~~~javascript
jsx中的循环
render() {
    return (
            <ul>
                {
                    this.state.list.map((item,index)=>{
                        return(
                            <li
                                key={index}
                                onClick={()=>this.handleItemDelete(index)}
                            >
                                {item}
                            </li>
                        )
                    })
                }
            </ul>
		)
	}
    handleItemDelete=(index)=>{
        // console.log(index)
        const list = [...this.state.list]
        list.splice(index,1)
        // console.log(list)
        this.setState({
            list:list
        })
       //优化1
        this.setState(()=>({
            list
        }))
        //优化2
        this.setState(()=>{
            return{
                list
            }
        })
    }
~~~

当事件需要传参的时候，如果处理不当，会立即执行

传参的三种方式

~~~javascript
1.
<button onClick={this.handleBtnClick.bind(this,"abc")}></button>
定义handleBtnClick方法
handleBtnClick(name){
    ....
}
2.
<button onClick={this.handleBtnClick("abc")}></button>
定义handleBtnClick方法
handleBtnClick=(name)=>{
    return ()=>{
        ...
    }
}
   
3.
    <button onClick={()=>this.handleBtnClick("abc")}></button>
    定义handleBtnClick方法
    handleBtnClick=(name)=>{
		...
    }
~~~

------



### 组件之间的传值

在Vue中父子组件的传值

通过父组件向子组件绑定值（：xxx=“xxx”）,绑定方法(@xxx="xxx")

子组件通过props来接受数据props:['xxx'],或者详细写法props:{xxx:{type:String,required:true}}

子组件触发父组件的方法，通过this.$emit('xxx',值)

=======================================================================================

在react中通过**this.props**来获取父组件向子组件传递的值

~~~javascript
父组件
import TodoItem from './TodoItem'
...
<div>
    <TodoItem 
        item={item} 
        index={index} 
        handleItemDelete={()=>this.handleItemDelete(index)}>
    </TodoItem>
</div>
子组件
import React,{Component} from 'react'
import PropTypes from 'prop-types'
class TodoItem extends Component{
    constructor(props){
        super(props)
    }
    render(){
        return (
            <div onClick={this.handleDelete}>
                {this.props.item}
            </div>
        )

    }
    handleDelete=()=>{
        // console.log(this.props.index)
        this.props.handleItemDelete(this.props.index)
    }

}
//强定义类型
TodoItem.propTypes={
    test:PropTypes.string.isRequired,
    item:PropTypes.string,
    handleItemDelete:PropTypes.func
}
TodoItem.defaultProps={
    test:"hello world"
}
export default TodoItem

~~~

#### props state render()关系

props或者state值改变了，render()函数就执行一次，所以就会有数据改变了，视图也跟着改变



------

### 虚拟DOM与diff算法

+ 虚拟DOM就是个js对象

  在react中实现虚拟DOM的步骤

1. state数据
2. JSX模板，然后通过React.createElement("标签名",{id=“xxx”...},"标签值")来生成JS对象，也就是虚拟DOM
3. 生成虚拟DOM(虚拟DOM就是个JS对象，用它来描述真是DOIM) (损耗了性能)
4. 数据 + 模板 生成真实的DOM，并显示
5. state数据改变
6. 数据 + 模板 生成新的虚拟DOM(通过生成新的JS对象，极大的提升性能，因为没有虚拟DOM话，是要生成真实DOM)
7. 比较原始虚拟DOM与新的虚拟DOM的差别，通过diff算法找到区别，并修改
8. 直接 操作DOM，显示新的DOM

+ diff算法

