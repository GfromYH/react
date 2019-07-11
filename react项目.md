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

