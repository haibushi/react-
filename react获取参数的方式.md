react获取参数的方式

1.动态路由

```
路由方式
<Route path='/catrgory/:id' extra component={Catrgory} ></Route>

cacatrgory.jsx
import {useState} from 'react';
import { useHistory, useParams, useLocation } from 'react-router-dom';
import queryString  from 'query-string'
import Test from './test';
function Category(props){
    let history = useHistory();
    let params = useParams();
    let location = useLocation();
	
    console.log('props',props); 
    console.log('props',queryString.parse(props.location.search));
    // 如果URL地址是/catrgory/2?id=3 这里的输出就是 {id:3}
    console.log('history',history);
    // 如果URL地址是/catrgory/2  这里的params={id:2}
    console.log('params',params);
    
    console.log('location',location);
    const [status, setStatus] = useState(false)

    const request = ({ queryKey }) => {
        // 为了模拟实际中 api 的时长随机性，随机 1.5s 或 3s 后得到响应
        const time = Math.random() > .5 ? 3000 : 1500
        return new Promise((resolve, reject) => {
          setTimeout(() => {
            resolve(time)
          }, time)
        })
      }
       return (<>
       <Test />
    </>)
}

export default Category
```



子组件的props里面除了包含父组件传递的参数外，默认是没有history，location的数据的，

子组件Test.jsx

```

如果当前的路由为/catrgory/2?id=3

import {useHistory,useParams,useLocation} from 'react-router-dom';
import queryString from 'query-string';
function Test(props){
    const history = useHistory();
    const params = useParams();
    const location = useLocation();
    console.log('child history.',history);  //包含了路由跳转需要的api
    console.log('child props.',props); 
    console.log('child params.',params); //=>{id:2}
    console.log('child location.',location); 
    console.log('child location.',queryString.parse(location.search)); //=>{id=3}
    const goAsd = ()=>{
        history.push('/asd');
    }
    return <div onClick={goAsd}>Test</div>
}

export default Test;
```

