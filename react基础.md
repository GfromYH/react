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