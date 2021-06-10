# js 装饰器@

装饰器@是ES7的一个新语法。他通过***@方法名***可以对一些对象进行包装然后返回一个被包装过的对象，他可以装饰的对象包含：类  ， 属性 ，方法

### 1类的装饰

当装饰器的对象是类的时候我们操作的就是这个类本身，装饰器函数的第一个参数就是要装饰的目标类

```
@test
class MyClass{}
function test(target){
	target.prototype.logger = ()=>{console.log(target.name)}
}

const myClass = new MyClass()
myClass.logger()   //输出的就是  MyClass
```

装饰器可以添加参数

```
@test('xx')
class MyClass{}
//装饰器添加参数的时候  就需要返回一个函数
function test(param){
	return (target)=>{}
    target.prototype.logger = ()=>{console.log(target.name,param)}
  }
}

const myClass = new MyClass()
myClass.logger()   //输出的就是  MyClass
```



### 2属性或者方法的装饰

对于类属性或者类方法的装饰本质是操作器描述符，可以把此时的装饰器理解成是***Object.defineProperty(obj,props,desc)***

```
class MyClass{
	@foo(false)
	method(){
		console.log('abc');
	}
}

function foo(params){
	return (target,key,desc)=>{
		// target ==  MyClass.property
		// key  == method,
		// desc == 原来的对象值
		desc.writable = params
		return desc;
	}
}

const myClass = new MyClass()
myClass.method = ()=>{
	console.log('def');
}
myClass.method() // abc

```

装饰器对类的行为的改标，是代码编译时发生的，而不是运行时，也就是说装饰器是编译时执行的

### 3.react装饰器的使用

在react的常见的操作使用方法(HOC),高阶组件用法

```
import React,{Component} from 'react';
const Root(Component){
	return (props)=>{
		return (<Component {...props} rootAttr="Root属性"></Component>)
	}
}
const Parent(Component){
	return (props)=>{
		return (<Component {...props} rootAttr="Parent属性"></Component>)
	}
}
function Test(props){
	return (
		<div>{this.props.rootAttr}</div>
		<div>{this.props.parentAttr}</div>
	)
}

export default Root(Parent(Component))   //多层次就会嵌套多级调用
```

装饰器使用的时候可以修改为如下方法

```
import React,{Component} from 'react';
const Root(Component){
	return (props)=>{
		return (<Component {...props} rootAttr="Root属性"></Component>)
	}
}
const Parent(Component){
	return (props)=>{
		return (<Component {...props} rootAttr="Parent属性"></Component>)
	}
}

@Root
@Parent
function Test(props){
	return (
		<div>{this.props.rootAttr}</div>
		<div>{this.props.parentAttr}</div>
	)
}

export default Component
```

