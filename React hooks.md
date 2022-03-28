# React hooks

### React函数式组件的特性123  123  123 234

hello

1.不需要声明类，可以避免大量的constructor，extends之类的代码

2.不需要显示声明this关键字

3.更佳的性能表现(不需要进行生命周期的管理和状态的管理)

...

### 函数式组件hooks

hooks是React16.8的新增特性，可以在不编写class组件的情况之下只有state以及react其他特性

那么什么时候会用到hooks？如果你在编写函数式组件的时候需要向当前组件添加一些state的时候，以前的做法就是需要将当前组件变成一个class组件，那么现在就可以直接在函数式组件中使用hooks

#### 一.state hooks

```react
import React, { useState } from 'react';

function Example() {
  // 声明一个叫 "count" 的 state 变量 默认值为0
  // count 就是state的一个属性  setCount就是用来修改count的一个方法
  const [count, setCount] = useState(0);  

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
// 如果在函数组件中需要使用多个state状态的话  可以调用多次useState函数来创建
function Example() {
  const [count, setCount] = useState(0);  
	const [name, setName] = useState('xiaoming');  
  return (
    <></>
  );
}
```

在函数式组件中  我们没有this，所以没有办法读取this.state,所以可以直接在函数组件中使用useState

#### 二.Effect Hook

​	useEffect可以在你的函数组件中使用执行副作用操作

```
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);
  //组件挂载成功或者更新之后就会自动调用useEffect
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
// 下面这样的方式  会产生一直调用Effect函数
// 在组件挂载完成了之后  调用了useEffect函数，那么函数执行 更新了list
// list更新了之后会导致组件UI的更新  这个时候就会在一次调用useEffect函数。。。
function Example() {
	const [list, setList] = useState([]);
  useEffect(() => {
    axios.get('http://xxx.com').then(res=>{
    	setList(res.dataList)
    })
  });
  return (
    <div>
    	{list.map(res=>{
    		return <p>{res.id}</p>
    	})}
    </div>
  );
}
```

Effect默认状态之下是每次组件挂载完成或者更新之后都会去执行，但是这样也会导致性能问题，在class组件中我们判断组件更新之后是否执行某个操作可以按照componentDidUpdate这个函数来比较，那么在使用Effect来操作的话我们只需要给Effect传递数组作为`useEffect` 的第二个可选参数即可：

```
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // 仅在 count 更改时更新  传空数组就表示只是执行一次该函数  这里有一点vue的watcher影子
```

#### 三.useMemo 

useMemo 用于性能优化，通过记忆值来避免在每个渲染上执⾏高开销的计算。

返回一个 memoized 值。

- callback是一个函数用于处理逻辑
- array 控制useMemo重新执⾏行的数组，array改变时才会 重新执行useMemo

​    第二个参数不穿数组的话  每次都会重新计算

​	第二个参数为空数组的话   只会执行一次

​    第二个参数传递对应的属性，当值发生变化的时候，才会重新计算

```typescript
function App () {
  const [ count, setCount ] = useState(0)
  const add = useMemo(() => count + 1, [count])
  return (
    <div>
      点击次数: { count }
      <br/>
      次数加一: { add }
      <button onClick={() => { setCount(count + 1)}}>点我</button>
    </div>
    )
}
```

