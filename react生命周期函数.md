## react生命周期钩子

### 第一个是组件初始化(initialization)阶段

​	也就是类组件的构造方法，类组件一般都是继承react.Component这个基类，才能有render(),生命周期等方法可以使用，这也就是说吗为什么函数组件不能使用生命周期方法的原因了。

​	super(props)用来调用基类的构造方法 也是将父组件的props注入给子组件，而constructor方法用来做一下组件的初始化工作，比如定义当前组件需要的属性

### 第二个是组件的挂载(Mounting)阶段

​	1，ComponentWillMount	

​	在组件你挂载到Dom之前调用且只会被调用一次，在这边调用this.setState不会引起组件的重新渲染，当然这里面的内容可以提前到构造方法中 所以项目中很少使用

​	2，render

​	根据组件的props和state(论值是否有变化,都可以引起组件重新弄render),return一个react元素(描述组件，即UI)，不负责组件实际渲染工作,之后有React自身根据此元素渲染出页面DOM,render是一个纯函数,没有副作用，在这个函数里面不能执行this.setState。会有改变组件状态的副作用

​	2，ComponentDidMount

​	组件挂载到Dom调用，且只会执行一次

### 第三个是组件的更新(update)阶段

##### 	1.父组件重新render

​		父组件更新render引起子组件render 的情况有两种

​			a,直接使用，每当父组件重新render导致的重传props,子组件将直接跟着重新渲染，无论props是否有变化！但是也可以通过shouldComponentUpdate方法优化

```
class Child extends Component {
   shouldComponentUpdate(nextProps){ // 应该使用这个方法，否则无论props是否有变化都将会导致组件跟着重新渲染
        if(nextProps.someThings === this.props.someThings){
          return false
        }
    }
    render() {
        return <div>{this.props.someThings}</div>
    }
}
```

​			b,在componentWillReceiveProps方法中将props转化为自己的state数据

```
class Child extends Component {
    constructor(props) {
        super(props);
        this.state = {
            someThings: props.someThings
        };
    }
    componentWillReceiveProps(nextProps) { // 父组件重传props时就会调用这个方法
        this.setState({someThings: nextProps.someThings});
    }
    render() {
        return <div>{this.state.someThings}</div>
    }
}
```

官网描述：在该函数ComponentWillReceiveProps将不会引起组件的第二次渲染

​	是因为componentWillReceiveProps中判断了props是否变化了，若是变化了this.setState将引起state的变化  从而引起render，此时就没有必要在做第二次因为重传props引起的render了，不然做了一次同样的渲染

##### 	2.组件本身调用setState 无论state有没有变化都会引起render函数的执行，也可以通过shouldComponentUpdate方法来优化

```
class Child extends Component {
   constructor(props) {
        super(props);
        this.state = {
          someThings:1
        }
   }
   shouldComponentUpdate(nextStates){ // 应该使用这个方法，否则无论state是否有变化都将会导致组件重新渲染
        if(nextStates.someThings === this.state.someThings){
          return false
        }
    }

   handleClick = () => { // 虽然调用了setState ，但state并无变化
        const preSomeThings = this.state.someThings
         this.setState({
            someThings: preSomeThings
         })
   }

    render() {
        return <div onClick = {this.handleClick}>{this.state.someThings}</div>
    }
}
```

#### 此阶段分为componentWillReceiveProps，shouldComponentUpdate，componentWillUpdate，render，componentDidUpdate

​	ComponentWillReceiveProps(nextProps)

​	此方法只调用于props引起的组件更新过程中，相应props变化之后进行更新的唯一方式。参数nextProps是父组件传给当前组件你的新props,但是父组件的render方法的调用不能保证传给当前组件的props是有变化的。所以此方法中的nextProps和this.props来查明重传的props是否改变，以及如果改变了要执行什么，比如根据心的props调用this.setState触发当前组件的更新render

​	ShouldComponentUpdate(nextProps,nextstate)

​	此方法通过比较nextProps，nextState与当前组件的this.props,this.state，返回true时当前组件将继续执行更新过程，否则当前组件停止更新，该方法可以减少组件的不必要的渲染  优化组件性能

### 卸载阶段



