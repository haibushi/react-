# TypeScript 接口类型

### 预览接口实现

```typescript
interface LabelledValue {
  label: string;
}

function printLabel(labelledObj: LabelledValue) {
  console.log(labelledObj.label);
}

let myObj = {size: 10, label: "Size 10 Object"};
printLabel(myObj);
```

interface 接口关键字，接口LabelledValue代表了有一个label属性且类型为string的属性

ts只会关注接口中值的外形，只要传入的对象是符合对象的必要条件，那就是被ts允许的，类型检查器不会检查属性的顺序，只要相应的属性存在也类型也符合就是对的

### 可选属性

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
}
function createSquare(config: SquareConfig): {color: string; area: number} {
  let newSquare = {color: "white", area: 100};
  if (config.color) { //判断属性是否存在
    newSquare.color = config.color;
  }
  if (config.width) {
    newSquare.area = config.width * config.width;
  }
  return newSquare;
}

let mySquare = createSquare({color: "black"});
```

属性名字定义的后面加*?*就是表述接口的可选属性，顾名思义就是这个属性可有可无

### 只读属性

```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}
let p1: Point = { x: 10, y: 20 };
p1.x = 5; // error! 只读属性不允许更改
```

只读属性只允许对象在刚创建的时候给其定义值，类似于定义常量，只要定义了就不允许更改了

### 函数类型

```
interface SearchFunc {
  (source: string, subString: string): boolean;
}
let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}
```

对于函数类型的检查来说，函数的参数名不需要与接口里面定义的名称相同

```
let mySearch: SearchFunc;
mySearch = function(src: string, sub: string): boolean {
  let result = src.search(sub);
  return result > -1;
}
```

函数的参数会逐个进行检查。要求对应位置的参数类型是兼容的，

