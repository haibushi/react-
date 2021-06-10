# pureComponent的理解

pureComponent是一个纯组件，用来优化react程序 减少render函数渲染的次数，提高性能**

**缺点**

pureComponent进行的是浅比较，也就是说如果是引用数据类型的话只会比较不是同一个地址，而不会去比较这个地址里面的数据是否一致，浅比较会忽略属性或者状态突变情况，其实也就是引用数据指针没有变化，而数据改变的时候render是不会执行的，如果我们需要重新渲染那么就需要重新开辟新的数据空间

**优点**

   当组件发生更新的时候，如果组件的props和state没有发生变化的时候render函数就不会执行，省去虚拟DOM的生成和对比过程，达到提升性能的目的。具体原因是react自动帮我门做了一层浅比较

**使用场景**

1.pureComponent一般会用在一些纯展示组件上，切合props和state不能使用同一个引用

2.在通过pureComponent进行组件的创建的时候不能在写shouldComponentUpdate，会引发警告



# 如何使用React.memo()

react.memo是一个高阶组件，是一个函数组件而不是一个类组件

````
function Child({seconds}){
    console.log('I am rendering');
    return (
        <div>I am update every {seconds} seconds</div>
    )
};
export default React.memo(Child)
````

React.memo()可以接受两个函数，第一个就是纯函数组件，第二个参数就是用于对比props控制是否刷新，与shouldComponentUpdate功能类似