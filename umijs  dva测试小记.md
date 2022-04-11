### umijs  dva测试小记

数据的改变发生通常是通过用户交互行为或者浏览器行为(如路由跳转)触发的，当此类行为会改变数据的时候是可以通过dispatch发起一个action，如果是同步会直接通过Reducers改变State，如果是异步则会先触发Effect然后流向Reducers最终改变state，所以在dva的数据流当中，数据流向非常清晰简明，

Dva默认的都是装在一个models的目录下，它有以下几个元素

### State

State是Model的状态数据，通常是一个js对象，操作的时候每次都要当作不可变数据来对待，保证每次都是全新的对象，没有引用关系，才能保证state的独立性

### Action

Action是一个普通的js对象，他是改变state的唯一途径，交互的过程中都是通过dispatch函数来调用一个Action，从而改变页面数据。Action必须包含一个type属性指明具体的行为，其他的字段就可以自定义了

### Dispatch

dispatch是一个用于触发action的函数

Reducers必须是一个纯函数，同样的输入必然得到同样的输出，他不应该产生任何的副作用，每次操作都是返回一个全新的数据

### Effect

Effect被称为一个副作用函数，最常见的就是异步操作

### Reducers



### Subscription

下面是一个umijs测试的案例

```javascript
在/src/models 创建一个system.ts文件
import {getUser} from '../api/index';  // 是一个异步的接口返回一个promise
export default {
    namespace:'system',
    state:{
        dataNum:1
    },
    reducers: {
        save(state:any,{payload}){
            return {...state,...payload}
        }
    },
    effects:{
        *addAsync({payload},{call,put}){
            // payload 接口传递的参数
            // call 用来执行异步的方法
            // put 用来执行同步的方法
            const res = yield call(getUser,payload);
            if(res){
                yield put({
                    type: 'save',
                    payload: {dataNum:res}
                })

            }
        }
    }
}

//下面是一个react组建
import { connect } from 'dva';
import {useSelector,useDispatch} from 'react-redux';
function IndexPage(props) {
  const dispatch = useDispatch();
  const system_state = useSelector(state=>state.system);
  const handles = ()=>{
    dispatch({
      type: 'system/save',
      payload:{
        dataNum:123
      }
    })
  }
  const handleBtn = ()=>{
    dispatch({
      type: 'system/addAsync',
    })
  }
  return (
    <div>
      <h1 className={styles.title} onClick={handles}>Page index{system_state.dataNum}</h1>
      <button onClick={handleBtn}>change</button>
    </div>
  );
}

export default connect(state => state)(IndexPage);
```

