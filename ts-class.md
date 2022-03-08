# react class

```typescript
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}

let greeter = new Greeter("world");
```

面向对象编程 常规操作  new操作的时候  执行的就是类的构造函数(constructor)

### 继承

```
class Animal {
    move(distanceInMeters: number = 0) {
        console.log(`Animal moved ${distanceInMeters}m.`);
    }
}
class Dog extends Animal {
  bark() {
  	console.log('Woof! Woof!');
  }
}
```

继承的时候  子类也可以重新父类的方法

### 公共，私有与受保护的修饰符

```
class Animal {
    public name: string;  //到处都是可以调用  this.name
    private age: number  // 只能在本类中使用  this.age
    protected move(distanceInMeters: number) {  // 可以在本类或者派生类中使用  this.move
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}
```

public == 公共属性   如果属性没有修饰符  那么默认就是public

privated == 私有属性  不能在类的外部使用  也就是不能实例对象上面调用

protected == 受保护属性  也不能在类的外部使用  但是可以在子类中使用

### readonly修饰符

```
class Octopus {
    readonly name: string;
    readonly age: number = 8;
    constructor (theName: string) {
        this.name = theName;
    }
}
let dad = new Octopus("Man with the 8 strong legs");
dad.name = "Man with the 3-piece suit"; // 错误! name 是只读的
```

Readonly 将属性设置为只读的  只读属性必须在声明时或者构造函数里面执行

### 静态属性

```
class Grid {
    static origin = {x: 0, y: 0};
    calculateDistanceFromOrigin(point: {x: number; y: number;}) {
        let xDist = (point.x - Grid.origin.x);
        let yDist = (point.y - Grid.origin.y);
        return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
    }
    constructor (public scale: number) { }
}

```

静态属性绑定的是类本身而不是实例对象，要访问的话只能用类名(Ps  :   ***Grid.origin.x***)