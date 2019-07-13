## 简书项目react

### react中CSS模块  styled-components

<https://www.styled-components.com/>

~~~javascript
cnpm install styled-components --save
//注册全局样式
import React ,{Component,Fragment} from 'react';
import {GlobalStyle} from './styled'
import Header from './common/header/header'

class App extends Component{
    render() {
        return (
            <Fragment>
                <GlobalStyle></GlobalStyle>
                <Header></Header>
            </Fragment>
        );
    }
}

export default App;
//styled.js
import {createGlobalStyle} from 'styled-components'

export const GlobalStyle=createGlobalStyle`
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed, 
figure, figcaption, footer, header, hgroup, 
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
	margin: 0;
	padding: 0;
	border: 0;
	font-size: 100%;
	font: inherit;
	vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure, 
footer, header, hgroup, menu, nav, section {
	display: block;
}
body {
	line-height: 1;
}
ol, ul {
	list-style: none;
}
blockquote, q {
	quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
	content: '';
	content: none;
}
table {
	border-collapse: collapse;
	border-spacing: 0;
}
`;

~~~

~~~javascript
create-react-app 项目名

src目录下
Components //复用组件
Store //redux
	actionCreators
    actionTypes
    index
    reducer
Pages //一级页面
Api //接口
Common //公共部分
Router //路由配置
~~~

~~~javascript
cnpm install redux react-redux react-thunk --save
cnpm install redux-thunk --save
cnpm install react-router-dom --save

//app.js
import React,{Component,Fragment} from 'react'
import {Provider} from 'react-redux'
import store from './Store'
import {BrowserRouter as Router ,Route,Link} from  'react-router-dom'
export default class App extends Component{
    constructor(props){
        super(props);
    }
    render() {
        return(
            <Provider store={store}>
                <Router>
                <div>
                    hello world
                </div>
                </Router>
            </Provider>
        )
    }
}

~~~

------

### React+Antd

#### reset.css路径问题

~~~javascript
//在react项目中要想初始化css最好在public文件夹下迎入reset.css

//将reset.css挂载到index.html上

//1.
    <link rel="stylesheet" href="./css/reset.css">
//通过相对路径写法，如果路由层级少可能不会出错，但是如果有2级或以上则会出错，因为是想读路径
        
//2.合理写法
       <link rel="stylesheet" href="/css/reset.css"> 
~~~



------

#### 动态添加menu菜单

+ reduce++递归

~~~javascript
    //动态添加列表
    //通过reduce+递归，也可以map+递归
    getMenuNodes=(menuList)=>{
       return menuList.reduce((pre,item)=>{
           if (!item.children) {
               pre.push((
                   <Menu.Item key={item.key}>
                       <Link to={item.key}>
                           <Icon type={item.icon} />
                           <span>{item.title}</span>
                       </Link>
                   </Menu.Item>
               ))
           }else{
               pre.push((
                   <SubMenu
                       key={item.key}
                       title={
                           <span>
                                <Icon type={item.icon} />
                                <span>{item.title}</span>
                           </span>
                       }
                   >
                       {this.getMenuNodes(item.children)}
                   </SubMenu>
               ))
           }
           return pre
       },[])
    }
~~~

+ map+递归

~~~javascript
getMenuNodes=(menuList)=>{
       return menuList.map((item)=>{
           if (!item.children) {
              return (
                   <Menu.Item key={item.key}>
                       <Link to={item.key}>
                           <Icon type={item.icon} />
                           <span>{item.title}</span>
                       </Link>
                   </Menu.Item>
               )
           }else{
               return(
                   <SubMenu
                       key={item.key}
                       title={
                           <span>
                                <Icon type={item.icon} />
                                <span>{item.title}</span>
                           </span>
                       }
                   >
                       {this.getMenuNodes(item.children)}
                   </SubMenu>
               )
           }
          
       })
    }
~~~



------

#### 将不是路由的组件变成路由组件

通过react-router-dom中的高阶组件withRouter

withRouter（非路由组件） 

会给非路由组件赋予history/location/match这三个对象

------

#### 关于react脚手架中跨域问题

~~~javascript
cnpm install http-proxy-middleware --save

//在src目录下创建setupProxy.js文件
const proxy = require('http-proxy-middleware')
//后台接口http://localhost:3001/api/XXX/XXX
module.exports = function(app) {
    app.use(proxy('/api', {
        target: 'http://localhost:3001/api/', //目标接口地址
        ws:true,
        changeOrigin:true,
        pathRewrite: {
            "^/api": ""
        }
    }))
}

~~~

