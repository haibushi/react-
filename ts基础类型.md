# TypeScript 基础类型

### 1.布尔值

最基础的数据类型就是简单的true/false值

```typescript
let isDone: boolean = false
```

### 2.数字类型

ts里面所有的数字都是浮点数，这些浮点数的类型都是number

```typescript
let num: number = 1;
```

### 3.字符串

使用string表示文本数据类型，可以使用("")或者('')来表述字符串

```
let name: string = "xiaoming"
// 也可以使用es6模版字符串来表示
let str: string = `xiaoming`
```

### 4.数组

ts有两种方式来定义数组

```
let arr1: number[] = [1,2,3]
let arr2: Array<number> = [1,2,3]
```

### 5.枚举

enum是ts对js标准数据类型的一个补充

```
enum Color {Red,Green,Black}
let c: Color = Color.Red  //0  
// 默认情况之下从0开始为数据编号，当然也可以手动修改数据的编号
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green;  // 2
// 也可以全部手动赋值
enum Color {Red = 1, Green = 2, Blue = 3}
let c: Color = Color.Green;  // 2
```

### 5.Any

在编程阶段还不清楚的数据类型  就可以指定为Any，这些值有可能来源于动态的内容

```
let notSure: any = 4
notSure = 'a'; //在没有指定类型的情况之下可以赋值为字符串
notSure = false  //在没有指定类型的情况之下可以赋值为布尔值
```

### 6.Void

在某种程度上来说 void和any是刚好相反的，void表示没有任何类型，当一个函数没有返回值的时候，通常设置的返回值类型就是void

```
function pring(name: string): void{
	console.log(name);
}
```

声明一个void类型的变量时没有什么意义  因为变量的值只能赋值为undefined  | null

```
let unusable: void = undefined;
```

### 7.null 和 undefined

在ts中undefined和null两个各自有自己的类型叫`undefined`和`null`

### 8.元组 Tuple

元组类型允许表示一个已知元素数量和类型的数组，各个元素的类型不必相同

```
let x:[string,number]
x = ['xx',20]; // success
x = [20,'xx'] // error
//当访问一个已知索引的元素时，可以等到正确的类型
x[0].substr(1) // success
x[1].substr(1) // error, 'number' does not have 'substr'
//当访问一个越界的元素时，会使用联合类型替代
x[3] = 'world' // success 字符串可以赋值给(number | string)类型
x[5].toString() // success  number | string 都有toString方法
x[6] = false //error 布尔类型不是(number | string)类型
```

### 9.never

never表示哪些永远不存在的类型

### 10.Object

object表示非原始类型  ，也就是除了number, string,boolean,null,undefined,symbol之外的类型

```
function create(obj: object | null):void;
create({x:1}) // success
create(null) // success
create(2) // error
create('xx') // error
```

### 11.ps 类型断言

ts类型断言好比类型转换，他没有运行时的影响，只是在编译阶段起作用，ts会假设你在写程序的时候已经进行了必须的检查

类型断言有两种方式

1.其一是“尖括号”语法

```
let someValue: any = 'this is a string';
let someLen: number = (<string>someValue).length
```

21.其一是“as”语法

```
let simeVakue: any = 'this is a string';
let someLen: number = (someValue as string).length;
```

两种形式时一样的 ，只是在ts使用jsx语法的时候  只有as断言语法时被允许的，所以推荐使用as语法