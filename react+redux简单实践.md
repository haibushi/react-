# react+redux简单实践

## 1 安装

 npm install --save react-redux redux @type/react-redux @type/redux

## 2 什么是redux

redux 是js应用的可预测状态的容器 可以理解为全局数据状态管理工具 主要用来做组件通信

redux包含了以下终点

​	state  前端中的state就是数据 就是一个对象，redux中的state是不可以直接修改的  只能通过action来修改 				相当于在单例中定义setter方法

​	action  redux将每一个更改动作描述为action,要更改state的内容 你需要发送一个action  一个action就是一个简单的对象  用来描述state发生什么变更

​	reducer  数据state &指示action都有了那么就是实现了  reducer就是根据action来对state进行操作,通过reducer操作后就返回一个新的state   比如下面中根据actionn的type分别对state的num属性进行加减

```typescript
const INCREMENT = 'INCREMENT';
const REDUCE = 'REDUCE';
const calculate = (state, action ) => {
    switch (action.type) {
        case INCREMENT:
            return {num: state.num + action.count}
        case REDUCE:
            return {num: state.num - action.count}
        default:
            return state
    }
}

export default {calculate}
```

​	store    就是整个项目保存数据的地方 并且只能有一个 创建store就是把所有的reducer给他

```jsx
import { createStore, combineReducers } from "redux";
import { calculate } from "./calculate";

// 全局你可以创建多个reducer 在这里统一在一起
const rootReducers = combineReducers({calculate})  如果有多个redux  就都放在这个对象里面
// 全局就管理一个store
export const store = createStore(rootReducers)
```

​	dispatch  store.dispatch()   是组件发出acion的唯一方法

​    		store.dispatch(incrementAction);  通过store 掉用incrementAction   那么就直接把state给修改了

## 3 简单实现

​     创建reducers目录  在该目录下创建两个reducer

​     /reducers/calculate

~~~javascript
const calculate = (state=initData,action) => {
  let item_state = {};
  switch(action.type){
    case INCREMENT:
      item_state = {num : state.num + action.count};
      break;
    case REDUCE:
      item_state = {num : state.num - action.count};
      break;
    default:
      item_state = state;
      break;
  }
  return item_state
}
export default {calculate}
~~~

/reducers/userInfo.js

~~~javascript
const initState = {
  name:'',
} 
export function userInfo(state=initState,action){
  let item_state = {}
  switch(action.type){
    case 'set':
      item_state = { name: action.name }
      break;
    default:
      item_state = state;
      break;
  }
  return item_state;
} 
~~~

/reducer/index.js

~~~javascript
import { calculate } from './calculate.js';
import { userInfo } from './userInfo.js';
import { createStore,combineReducers } from 'redux';
const rootReducer = combineReducers({calculate, setUserInfo});
export default createStore(rootReducer);
~~~

在组件中开始使用redux    home.jsx

~~~jsx
import React, { Component } from 'react';
import {connect} from 'react-redux';
import {withRouter} from 'react-router-dom'
import store from '../reducers/index';
const setUserInfo = (data)=>{
  retcurn {
    type:'set',
    ...data
  }
}
class Home extends Component {
  constructor(props) {
    super(props);
    
    this.state = {
      name:''
    }
  }
  shouldComponentUpdate(nextProps, nextState){
    return true;
  }
  
  clickMe = () => {
    console.log(this.props)
    this.props.setUserInfo(setUserInfo({name:'ween'}));
    console.log(store.getState());
  }
  render(){
    return (
      <div>
        home
        <button onClick={this.clickMe}>点我登入</button>
        {this.props.userInfo.name}
      </div>
    )
  }
}

const mapStateToProps = state => state;
const mapDispatchToProps = dispatch => {
  return {
    setUserInfo : data => {
      dispatch(setUserInfo(data));
    }
  }
};

export default connect(mapStateToProps,mapDispatchToProps)(withRouter(Home));
~~~

可以在debug下面看到store的数据和修改数据的方法   mapStateToProps就是在当前组件加载store的数据state  ， mapDispatchToProps就是在当前组件加载store的修改数据的方法  都装在到了当前组件的props身上了  当组件加载的时候  可以看到详细信息

 /reducers/calculate.js   /reducers/userInfo.js  这两个文件的文件名  就是当前store里面数据state的key(state就是一个对象)  在debug可以看到  当前组件有一个setUserInfo的方法去修改state的值   calculate和userInfo这两个对象就是当前组件可以获取到的store的state的值，他们就是按照reducer的文件名而来的

![image-20201228160448046](/Users/zhangweien/Library/Application Support/typora-user-images/image-20201228160448046.png)