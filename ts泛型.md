#  TypeScript泛型

Typescript 的泛型可以为我们创建可以重用的组件，一个组件支持多种类型的数据，为代码添加额外的抽象层和可重用行，泛型可以用于函数 , 接口 ,类

### 1泛型函数

```
function Foo<T>(params:T):T{
	return params;
}

Foo('abc');
//Foo<string>('abc');
Foo(123);
//Foo<number>(123);
```

### 2泛型类

```
// 添加用户 只有用户名称和年龄
interFace User{
	name:string,
	age:number
}
class UserList{
	list:any[]
	add(data:User){
		this.list.push(data)
		console.log(data)
	}
}

// 添加用户  姓名 年龄 性别
interFace User1{
	name:string,
	age:number,
	sex:number
}
class UserList{
	list:any[]
	add(data:User1){
		this.list.push(data)
		console.log(data)
	}
}



class UserList<T>{
	list:any[]
	add(data:T){
		this.list.push(data)
		console.log(data)
	}
}
//只有用户名称和年龄
interFace User{
	name:string,
	age:number
}
let userList = new UserList<User>()
UserList.add({name:'xx',age:12})

//只有用户名称和年龄,性别
interFace User{
	name:string,
	age:number
}
let userList = new UserList<User>()
UserList.add({name:'xx', age:12, sex:1})

```

