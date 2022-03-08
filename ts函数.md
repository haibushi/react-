# TypeScript 函数

### 1.定义

```typescript
ES5 函数定义的两种方式
function foo(x,y){
	return x + y;
}
let foo = function(x,y){
	return x + y;
}

ts 函数定义的方式
// 参数和返回值最好都有定义类型
function foo(x:number,y:number):number{
	return x+y;
}
let foo = function(x:number,y:number):number{
	return x+y;
}
```

### 2.参数

参数分为*可选参数*  *默认参数*  *剩余参数*

```typescript
1.可选参数 一般是放在最后的一个参数上面
function foo(x:string,y?:string):void{}
2.默认参数 和可选参数一样，一般是放在最后的一个参数上面
function foo(x:string,y:string = 'xx'):string{}
2.剩余参数  最后一个参数是一个数组
function foo(x:number,...rest:number[]){
	let sum = x;
	for(var i =0;i<rest.length;i++){
		sum += rest;
	}
	return sum
}
```

### 3.重载

同一个函数提供多个函数定义来进行函数重载,编译器会根据这个列表去处理函数的调用

```typescript
function foo(str:string):void;
function foo(num:number):number;
function foo(args:any):any{
  if(typeof args == 'string'){
  	console.log(name);   
  }else if(typeof args == 'number'){
    return args;
  }
}

foo(123);  // 匹配参数为string
foo('abcd');  // 匹配参数为number
foo(true);  // 报错  类型不匹配
```

为了让编译器能选择正确的检查类型，回去检查重载列表，尝试使用第一个重载定义，如果匹配的话就使用这个。

function foo(args:any):any  这个并不是重载列表的一部分，所以这里只有两个重载，一个接收的是字符串，一个接收的是number，如果以其他类型参数来调用就会报错