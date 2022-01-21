## 基础

---

### TypeScript的含义

TypeScript是JavaScript的一个超集，它主要是提供了类型系统和对ES6的支持，可以编译成纯JavaScript运行在各种浏览器上，TypeScript编译工具可以运行在各种服务器上和系统上，TypeScript是开源的。

### TypeScript的优点

1. 增强可读性和可维护性，看到类型就知道如何使用了，在编译的时候就能发现错误

2. 有包容性，包含了JavaScript，即使编译出错也能解析成JavaScript，兼容第三方库即使不是用ts写的

3. 拥有非常活跃的社区，很多项目都是用ts写的vue3、angular、ant design等，很多第三方库都提供ts定义类型，ts支持ES6语法


### 安装TypeScript

```
npm i -g typescript  //安装
```

```
tsc hello.ts  //编译ts文件，会生成一个同名js文件
```

```
npm i -g ts-node  
ts-node hello.ts   //及时编译
```

### 第一个TypeScript例子

```
function sayHello(name: String) {
    return "hello," + name
}
let user: String = "yangshuang"
console.log(sayHello(user));
```

### 原始数据类型

原始数据类型包括：数值(number)、字符串(string)、布尔值(boolean)、undefined、null以及ES6新增的符号型（Symbol）、BigInt

**注意：**

1. :boolean是一个基本数据类型，

   ```ts
   let createdByNewBoolean: boolean = new Boolean(1); 
   // Type 'Boolean' is not assignable to type 'boolean'.
   ```
2. 用构造函数new Boolean创建的不是一个布尔值，而是一个布尔对象可用:Boolean定义

   let createdByNewBoolean: Boolean = new Boolean(1);
3. 直接调用Boolean是一个布尔值。其他基本类型也一样（除了null和undefined）

   let createdByBoolean: boolean = Boolean(1);
### 空值
在TypeScript中viod代表空值，表示没有任何返回值的函数，定义一个空值的变量只能把它赋值为null和undefined

```ts
function sayHello():void{
  console.log('hello,this is void');  
}
```

与viod的区别是，null和undefined是所有类型的子类型，可以把定义的undefined赋值给任意基本数据类型，而viod不能。

### 任意值

**任意值**（Any）用来表示可以允许赋值为任意类型

1. **任意类型**：如果是一个普通类型，在赋值过程中改变类型是不被允许的，但如果是any类型，允许赋值为任意类型。

let myFavoriteNumber: any = 'seven';
myFavoriteNumber = 7;
2. **在任意值上访问方法和属性都是被允许的**，也就是说一个变量被定义成任意值的时候对他进行的任意操作，返回内容的类型都是任意值。

3. 如果一个**变量在定义的时候未定义类型那他就是任意值**，类型可以任意改变

```ts
let something;
something = 'seven';
something = 7;

something.setName('Tom');
```


### 类型推论

**类型推论：**如果没有明确的指定类型，那么TypeScript会按照类型推论的规则推断出一个类型

1. 当变量在初始化的时候没有指定类型，而只是赋值，那么**值的类型就是变量的类型**，如果下一次改变就会出错。

let myFavoriteNumber = 'seven';  //没有定义类型被推断为string类型
myFavoriteNumber = 7;//把它修改为数值型就会报错
2. 当变量定义的时候**没有赋值那变量的类型就是any任意类型**。

### 联合类型

**联合类型**：表示取值是多种类型中的一种，用|进行分割

```ts
let myFavoriteNumber: string | number;  //允许类型是string或者number
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

在没有确定类型的时候，**联合类型只能使用共有的方法和属性**

function getLength(something: string | number): number {
    return something.length;  //报错，length 不是 string 和 number 的共有属性
}

联合类型在变量被**赋值之后会被推断出一个类型**，推断之后就只能访问推断那个类型的方法

```ts
let myFavoriteNumber:string|number
myFavoriteNumber='senve'
console.log(myFavoriteNumber.length);
myFavoriteNumber=7
console.log(myFavoriteNumber.length); //报错
```



### 接口

**接口：**在typeScript中使用接口(interfaces)来定义对象的类型。在面向对象语言中接口是对行为的抽象，具体的行为由类来实现。在typescript中除了可以对类的一部分行为进行抽象之外也对对象的形状进行描述。

接口一般首字母大写

```ts
interface Person {
    name: String,
    age: number
}
let tom: Person = {
    name: "tom",
    age: 12
}
console.log(tom);   //{ name: 'tom', age: 12 }
```

**接口的形状和变量的形状必须一致**，不能多也不能少，也不能添加未定义的属性。如果想要不一样可以使用**可选属性**,可选属性表示属性可以不存在，但是仍然不能添加未定义属性

```ts
interface Person {
    name: String,
    age?: number  //可选参数?:
}
let tom: Person = {
    name: "tom"
}
console.log(tom);    //{ name: 'tom'}
```


#### 任意属性

如果有不确定的属性名，可以使用任意属性

```ts
interface Person {
    name: String,
    age?: number,
    [propName: string]: any    //定义任意属性，第一个Strin定义的是字段名（固定）
}
let tom: Person = {
    name: "tom",
    userName: "yangshuang"
}
console.log(tom);  //{ name: 'tom', userName: 'yangshuang'}
```

**注意**：**当定义了一个任意属性，那么可选属性和确定属性都是它的子集**，所有当任意属性后面的类型固定的时候所有属性的类型都要和他一致否者会报错，所以一般定义为类型any或联合类型

```ts
interface Person {
    name: String,
    age?: number,
    [propName: string]: string  //固定类型为string
}
let tom: Person = {
    name: "tom",
    age: 12,
    userName: "yangshuang"
}
console.log(tom);  //报错
```

一个接口只能定义一个任意类型any，如果已经有了一个任意类型，可以使用联合类型解决

```ts
interface Person {
    name: string,
    age?: number,
    [propName: string]: string | number  //联合属性
}
let tom: Person = {
    name: "tom",
    age: 12,
    userName: "yangshuang"
}
console.log(tom);
```


#### 只读属性

表示只能在为对象的某个字段只能在创建的时候赋值，可以用readonly来设置

```ts
interface Person {
    name: String,
    age: number,
    readonly user: string  //只读属性
}
let tom: Person = {
    name: "tom",
    age: 12,
    user: 'yangshuang'
}
console.log(tom);

tom.user = 'ys'   //报错
```

**注意：**只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候

```ts
interface User {
  uName: String,
  age: number,
  readonly user1?: string  //只读属性
}
let ys:User={
  uName:'yang',
  age:18
}
 ys.user1='yang'  //Cannot assign to 'user1' because it is a read-only property.
```


### 数组的类型

在ts中定义数组的方法很多，最为常用的是 **类型+[]**，数组的项中不允许有其他类型并且数组的方法参数也会有限制。

```ts
let numberArr: number[] = [1, 2, 3, 4, 5]
numberArr.push('8')  //报错
```

数组泛型**Array<elemType>**定义

```ts
let numberArr: Array<number> = [1, 2, 3, 4, 5]
```

也可以用接口定义，但是一般不这么用

```ts
interface NumberArr {
    [index: number]: number  //只要索引的类型是数字时，那么值的类型必须是数字
}
let arr: NumberArr = [1, 2, 3]
console.log(arr);
```

如果想要定义不一致类型的数组可以用:any

```ts
let anyArr: any[] = [1, '2', { naem: 'ys' }, 4, false]
console.log(anyArr);
```

**类数组：**不能用普通的数组的方式来描述，而应该用接口。类数组都有自己的接口定义

```ts
interface IArguments {
    [index: number]: any;
    length: number;
    callee: Function;
}
```

### 函数类型

#### 函数声明

一个函数有输入和输出，所以对函数进行类型约束要考虑到输入和输出的类型，也就是形参类型和返回值类型。

```ts
function sum(y: number, x: number): number {  //最后一个:number是返回值类型，也就是输出类型
    return x + y
}
console.log(sum(1, 2));
```

#### 函数表达式

```ts
let sum = function (y: number, x: number): number {
    return x + y
}
console.log(sum(12, 3));
```

函数表达式是通过类型推论确定的变量的类型上面的操作相当于

```tsx
let sum: (y: number, x: number) => number = function (y: number, x: number): number {
    return x + y
}
console.log(sum(12, 3));
```

**注意：**ts中的=>表示函数的定义，左边是输入类型（参数类型），右边是输出类型（返回值类型）。

**当参数多余和少于都是不可以的**，除非用到**可选参数**用？来表示，**可选参数后面不能再定义必需参数**

```ts
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

#### 用接口定义函数

```ts
interface SearchFunc {
    (source: string, subStr: string): boolean
}
let mySearch: SearchFunc = function (source: string, subStr: string) {
    return source.search(subStr) !== -1
}
console.log(mySearch('yangshuang', 'ys'));  //false
```

#### 参数默认值

默认值就相当于可选参数，而且不受可选参数要在必需参数的后面的限制

```ts
function sum(y: number = 12, x: number): number {
    return x + y
}
console.log(sum(undefined, 1));  //13
```

#### 剩余参数

ES6 中，可以使用 `...rest` 的方式获取函数中的剩余参数（rest 参数）,注意，**rest 参数只能是最后一个参数**

```tsx
function pushFn(array:any[],...items:any[]){
  items.forEach(function(item){
    array.push(item)
  })
  return console.log(array)
}
pushFn([0],1,2,3,4)
```

#### 函数重载

重载允许**一个函数接收不同类型或不同数量的参数做不同的操作，先函数定义再函数实现，多个函数定义要把精确定义写在前面**

```ts
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
console.log(reverse('hello')); //olleh
```

### 类型断言

表示手动地指定一个值的类型，有`值 as 类型`和`<类型>值`通常用第一种(tsx必须使用第一种)，第二种可能和泛型混淆。

#### 类型断言的用途

1. 把联合类型断言为一个类型

   ```ts
   interface Cat {
       name: string;
       run(): void;
   }
   interface Fish {
       name: string;
       swim(): void;
   }
   
   function swim(animal: Cat | Fish) {
       (animal as Fish).swim();  //断言
   }
   
   const tom1: Cat = {
       name: 'Tom',
       run() { console.log('run') }
   };
   swim(tom1);  // animal.swim is not a function
   ```

   **注意：**`swim` 函数接受的参数是 `Cat | Fish`，一旦传入的参数是 `Cat` 类型的变量，由于 `Cat` 上没有 `swim` 方法，就会导致运行时错误了

   

   2.父类断言一个更加具体的子类

   ```ts
   interface ApiError extends Error {
       code: number;
   }
   interface HttpError extends Error {
       statusCode: number;
   }
   
   function isApiError(error: Error) {
       if (typeof (error as ApiError).code === 'number') {
           return true;
       }
       return false;
   }
   ```
   
   注意：由于父类 `Error` 中没有 `code` 属性，故直接获取 `error.code` 会报错，需要使用类型断言获取 `(error as ApiError).code`。
   
   
   
   3.将任何一个类型断言为any
   
   ```ts
   (window as any).foo = 1;
   ```

4. 将any断言为一个具体的类型

   ```ts
   function getCacheData(key: string): any {
       return (window as any).cache[key];
   }
   
   interface Cat {
       name: string;
       run(): void;
   }
   
   const tom = getCacheData('tom') as Cat;
   tom.run();
   ```

   **特点**：

   - 联合类型可以被断言为其中一个类型
   - 父类可以被断言为子类
   - 任何类型都可以被断言为 any
   - any 可以被断言为任何类型
   - 要使得 `A` 能够被断言为 `B`，只需要 `A` 兼容 `B` 或 `B` 兼容 `A` 即可

   其实前四种情况都是最后一个的特例。

####  类型断言和类型转换

**类型断言只会影响 TypeScript 编译时的类型**，类型断言语句在编译结果中会被删除；所以类型断言不是类型转换，它**不会真的影响到变量的类型**。

```ts
function toBoolean(something: any): boolean {
    return something as boolean;  //类型断言
}

toBoolean(1);  // 返回值为 1
```

```ts
function toBoolean(something: any): boolean { 
    return Boolean(something);  //类型转换
}

toBoolean(1);  // 返回值为 true
```

#### 类型断言和类型声明

- `animal` 断言为 `Cat`，只需要满足 `Animal` 兼容 `Cat` 或 `Cat` 兼容 `Animal` 即可
- `animal` 赋值给 `tom`，需要满足 `Cat` 兼容 `Animal` 才行
- 也就是类型声明是比类型断言更加严格的。

```ts
interface Animal {
    name: string;
}
interface Cat {
    name: string;
    run(): void;
}

const animal: Animal = {
    name: 'tom'
};

let tom = animal as Cat  //断言  

let tom: Cat = animal;  //声明  报错因为Cat不兼容animal
```

#### 泛型

通过给 `getCacheData` 函数添加了一个泛型 `<T>`，我们可以更加规范的实现对 `getCacheData` 返回值的约束，这也同时去除掉了代码中的 `any`，是最优的一个解决方案。

```ts
function getCacheData<T>(key: string): T {
    return (window as any).cache[key];
}

interface Cat {
    name: string;
    run(): void;
}

const tom = getCacheData<Cat>('tom');
tom.run();
```

### 声明文件

当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。声明文件必需以 `.d.ts` 为后缀。推荐的是使用 `@types` 统一管理第三方库的声明文件。如`npm install @types/jquery --save-dev`

#### 书写声明文件（未看）

- [`declare var`](https://ts.xcatliu.com/basics/declaration-files.html#declare-var) 声明全局变量
- [`declare function`](https://ts.xcatliu.com/basics/declaration-files.html#declare-function) 声明全局方法
- [`declare class`](https://ts.xcatliu.com/basics/declaration-files.html#declare-class) 声明全局类
- [`declare enum`](https://ts.xcatliu.com/basics/declaration-files.html#declare-enum) 声明全局枚举类型
- [`declare namespace`](https://ts.xcatliu.com/basics/declaration-files.html#declare-namespace) 声明（含有子属性的）全局对象
- [`interface` 和 `type`](https://ts.xcatliu.com/basics/declaration-files.html#interface-和-type) 声明全局类型
- [`export`](https://ts.xcatliu.com/basics/declaration-files.html#export) 导出变量
- [`export namespace`](https://ts.xcatliu.com/basics/declaration-files.html#export-namespace) 导出（含有子属性的）对象
- [`export default`](https://ts.xcatliu.com/basics/declaration-files.html#export-default) ES6 默认导出
- [`export =`](https://ts.xcatliu.com/basics/declaration-files.html#export-1) commonjs 导出模块
- [`export as namespace`](https://ts.xcatliu.com/basics/declaration-files.html#export-as-namespace) UMD 库声明全局变量
- [`declare global`](https://ts.xcatliu.com/basics/declaration-files.html#declare-global) 扩展全局变量
- [`declare module`](https://ts.xcatliu.com/basics/declaration-files.html#declare-module) 扩展模块
- [`/// `](https://ts.xcatliu.com/basics/declaration-files.html#san-xie-xian-zhi-ling) 三斜线指令



## 进阶

---

### 类型别名

用于给类型取一个别名，用**type**实现，常用于联合类型

```ts
type Name = string;
type NameResolver = () => string; 
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    } else {
        return n();
    }
}
console.log(getName(getName('888'))); //888
```

### 字符串字面量类型

用来**约束取值只能是某几个字符串的一个**，**用type创建，`|`分割**

```ts
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
    // do something
}

handleEvent(document.getElementById('hello'), 'scroll');  // 没问题
handleEvent(document.getElementById('world'), 'dblclick'); // 报错，event 不能为 'dblclick'
```

### 元组

数组用来合并相同类型的对象，**元组用来合并不同类型的对象**。可以通过已知的索引来访问或赋值指定的项。

```ts
let tom: [string, number] = ['Tom', 25];
```

### 枚举

**用于取值在限定范围内的场景（可以把所有值都列出来）**，如一周有七天，用enum来创建。枚举成员会被赋值为从 `0` 开始递增的数字，同时也会对枚举值到枚举名进行反向映射.

```ts
enum Days {Sun=7, Mon=1.5, Tue, Wed, Thu, Fri, Sat=<any>'s'}
```

### 类

- 类（Class）：定义了一件事物的抽象特点，包含它的属性和方法
- 对象（Object）：类的实例，通过 `new` 生成
- 面向对象（OOP）的三大特性：封装、继承、多态
- 封装（Encapsulation）：将对数据的操作细节隐藏起来，只暴露对外的接口。外界调用端不需要（也不可能）知道细节，就能通过对外提供的接口来访问该对象，同时也保证了外界无法任意更改对象内部的数据
- 继承（Inheritance）：子类继承父类，子类除了拥有父类的所有特性外，还有一些更具体的特性
- **多态**（Polymorphism）：由继承而产生了相关的不同的类，对同一个方法可以有不同的响应。比如 `Cat` 和 `Dog` 都继承自 `Animal`，但是分别实现了自己的 `eat` 方法。此时针对某一个实例，我们无需了解它是 `Cat` 还是 `Dog`，就可以直接调用 `eat` 方法，程序会自动判断出来应该如何执行 `eat`
- **存取器**（getter & setter）：用以改变属性的读取和赋值行为
- 修饰符（Modifiers）：修饰符是一些关键字，用于限定成员或类型的性质。比如 `public` 表示公有属性或方法
- **抽象类**（Abstract Class）：抽象类是供其他类继承的基类，抽象类不允许被实例化。抽象类中的抽象方法必须在子类中被实现
- 接口（Interfaces）：不同类之间公有的属性或方法，可以抽象成一个接口。接口可以被类实现（implements）。一个类只能继承自另一个类，但是可以实现多个接口

#### 属性和方法

class定义类，constructor定义构造函数，new实例化对象

```ts
class Animal {
    public name;
    constructor(name) {
        this.name = name
    }
    sayHi() {
        return `my name is ${this.name}`
    }
}
let a = new Animal('java')
console.log(a.sayHi()); //my name is java
```

#### 类的继承

extends继承父类，super调用父类的方法和属性

```ts
class Dog extends Animal {
    constructor(name) {
        super(name)
        console.log(this.name)
    }
    sayHi() {
        return `hello,` + super.sayHi()
    }
}
let c = new Dog('js')  //js
console.log(c.sayHi())  //hello,my name is js
```

#### 存取器

使用 getter 和 setter 可以改变属性的赋值和读取行为

```ts
class Animal {
    constructor(name) {
        this.name = name;
    }
    get name() {
        return 'Jack';
    }
    set name(value) {
        console.log('setter: ' + value);
    }
}

let a = new Animal('Kitty'); // setter: Kitty
a.name = 'Tom'; // setter: Tom
console.log(a.name); // Jack
```

#### 静态方法

用static定义静态方法，它们不需要实例化，而是直接通过类来调用

```ts
class Animal {
    static isAnimal(a) {
        return a instanceof Animal;
    }
}

let a = new Animal();
Animal.isAnimal(a); // true
a.isAnimal(a); // TypeError: a.isAnimal is not a function
```

**实例属性**：ES6 中实例的属性只能通过构造函数中的 `this.xxx` 来定义，ES7 提案中可以直接在类里面定义` name = 'Jack';`

**静态属性**：ES7 提案中，可以使用 `static` 定义一个静态属性，`static num = 42`

**三种修饰符**：

- `public` 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 `public` 的
- `private` 修饰的属性或方法是私有的，不能在声明它的类的外部访问。当构造函数修饰为 `private` 时，该类不允许被继承或者实例化
- `protected` 修饰的属性或方法是受保护的，它和 `private` 类似，区别是它在子类中也是允许被访问的。当构造函数修饰为 `protected` 时，该类只允许被继承

**抽象类**：`abstract` 用于定义抽象类和其中的抽象方法。抽象类是不允许被实例化的，抽象类中的抽象方法必须被子类实现

```ts
abstract class Animal {
  public name;
  public constructor(name) {
    this.name = name;
  }
  public abstract sayHi();
}

class Cat extends Animal {
  public sayHi() {
    console.log(`Meow, My name is ${this.name}`);
  }
}

let cat = new Cat('Tom');
```

#### 类的类型

```ts
class Animal {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  sayHi(): string {
    return `My name is ${this.name}`;
  }
}

let a: Animal = new Animal('Jack');
console.log(a.sayHi()); // My name is Jack
```

### 类与接口

#### 类实现接口

一个类只能继承于另一个类，有时候不同的类之间有可以有一些相同的特性，可以把这些特性提升到接口中，用implement关键字来实现。这种特性大大提高了面向对象的灵活性

- 类可以实现接口
- 一个类可以实现多个接口
- 接口可以继承接口
- 接口可以继承类

```ts
interface Alarm{
  alert():void
}

interface LightableAlarm extends Alarm{//接口继承接口，接口也可继承类
  lightOn():void
  lightOff():void
}

class Door{}
class DoorTwo extends Door implements Alarm{
  alert(){
    console.log('DoorTwo is alert!');
  }
}
class Car implements Alarm,LightableAlarm{ //类实现多个接口
  alert(){
    console.log('Car is alert');
  }
  lightOn(){
    console.log('car light on');    
  }
  lightOff(){
    console.log('car light off');    
  }
}
let car = new Car()
car.alert()
car.lightOff()
car.lightOn()
```

### 泛型

泛型：是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

### 声明合并

声明合并：定义了两个相同名字的函数、接口或类，那么他们会被合并成一个类型

函数的合并可以使用重载

接口的合并，合并属性的类型必须是唯一的

