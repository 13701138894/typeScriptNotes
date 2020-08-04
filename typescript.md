# 1、什么是 TypeScript

> [TypeScript](http://www.typescriptlang.org/) 是 JavaScript 的一个超集，主要提供了**类型系统**和**对 ES6 的支持**

## 优点

> - TypeScript 增加了代码的可读性和可维护性
> - TypeScript 非常包容
> - TypeScript 拥有活跃的社区

## 缺点

> - 有一定的学习成本，需要理解接口（Interfaces）、泛型（Generics）、类（Classes）、枚举类型（Enums）等前端工程师可能不是很熟悉的概念
> - 短期可能会增加一些开发成本，毕竟要多写一些类型的定义，不过对于一个需要长期维护的项目，TypeScript 能够减少其维护成本
> - 集成到构建流程需要一些工作量
> - 可能和一些库结合的不是很完美



# 2、环境准备及运行

安装typescript ： npm install -g typescript

```js
// 检查版本：
tsc -v
// 编译：
tsc xxxx.ts
// 编译某一文件夹下的文件并生成js文件到另一目录 --outDir
tsc --outDir ./dist ./src/xxx.ts
// 指定编译代码版本 --target
tsc --outDir ./dist --target ES6 ./src/xxx.ts
// 监听模式，一次编译，改变自动同步编译 --watch
tsc --outDir ./dist --target ES6 --watch ./src/xxx.ts
```

```json
// 通过配置文件 默认命名tsconfig.json
{
    "complierOptions":{
        "target":"es6",
        "watch":true,
        "outDir": "./dist"
    }
    "include": ["./src/**/*"] // src下的所有子目录里的所有文件
}
// 编译
tsc
// 如果配置文件另有命名 如 c.json 则用tsc -p 编译
tsc -p ./src/c.json

```











# 3、指定变量类型

TypeScript 中，使用 `:` 指定变量的类型，`:` 的前后有没有空格都可以。

**TypeScript 编译的时候即使报错了，还是会生成编译结果**，我们仍然可以使用这个编译之后的文件。

如果要在报错的时候终止 js 文件的生成，可以在 `tsconfig.json` 中配置 `noEmitOnError` 即可

**类型检测检测的是类型而不是数值**

**没有类型标注，可以借助于第三方类型标注系统**：

```ts
// 正确
function sayHello(person:string){
    return 'Hello'+person
}
let user = '[1,2,3]';
console.log(sayHello(user))
// 错误，但是还是生成了 js 文件
function sayHello(person:string){
    return 'Hello'+person
}
let user = [1,2,3];
console.log(sayHello(user))
```

# 4、基础

## 1、原始数据类型

### 布尔值

> 使用 `boolean` 定义布尔值类型

```ts
// 定义布尔值
let isDone:boolean = false;
```

*使用构造函数Boolean创造的对象不是布尔值

```ts
let createdByNewBoolean: boolean = new Boolean(1);
// 事实上 new Boolean() 返回的是一个 Boolean 对象

```

### 数值

> 使用 `number` 定义数值类型

```ts
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010;
// ES6 中的八进制表示法
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```

```js
// 编译结果
var decLiteral = 6;
var hexLiteral = 0xf00d;
// ES6 中的二进制表示法
var binaryLiteral = 10;
// ES6 中的八进制表示法
var octalLiteral = 484;
var notANumber = NaN;
var infinityNumber = Infinity;
```

### 字符串

> 使用 `string` 定义字符串类型

```ts
let myName: string = 'Tom';
let myAge: number = 25;

// 模板字符串
let sentence: string = `Hello, my name is ${myName}.
I'll be ${myAge + 1} years old next month.`;
```

```js
// 编译
var myName = 'Tom';
var myAge = 25;
// 模板字符串
var sentence = "Hello, my name is " + myName + ".\nI'll be " + (myAge + 1) + " years old next month.";
```

### 空值

> JavaScript 没有空值（Void）的概念，在 TypeScript 中，可以用 `void` 表示没有任何返回值的函数
>
> 声明一个 `void` 类型的变量没有什么用，因为你只能将它赋值为 `undefined` 和 `null`

```ts
function alertName(): void {
    alert('My name is Tom');
}
```

```ts
let unusable: void = undefined;
```

### Null 和 Undefined

> 在 TypeScript 中，可以使用 `null` 和 `undefined` 来定义这两个原始数据类型

```ts
let a:undefined = undefined;
let b:null = null;
```

> 与 `void` 的区别是，`undefined` 和 `null` 是所有类型的子类型。也就是说 `undefined` 类型的变量，可以赋值给 `number` 类型的变量

```ts
// 这样不会报错
let num: number = undefined;
// 这样也不会报错
let u: undefined;
let num: number = u;
```

> 而 `void` 类型的变量不能赋值给 `number` 类型的变量

```ts
let u: void;
let num: number = u;

// Type 'void' is not assignable to type 'number'.
```

## 2、任意值

> 任意值（Any）用来表示允许赋值为任意类型

```ts
// 如果是一个普通类型，在赋值过程中改变类型是不被允许的
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

```ts
// 但如果是 any 类型，则允许被赋值为任意类型
let myFavoriteNumber: any = 'seven';
myFavoriteNumber = 7;
```

**任意值的属性和方法**

```ts
// 在任意值上访问任何属性都是允许的
let anyThing: any = 'hello';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName);

// 也允许调用任何方法
let anyThing: any = 'Tom';
anyThing.setName('Jerry');
anyThing.setName('Jerry').sayHello();
anyThing.myName.setFirstName('Cat');
```

**未声明类型的变量**

```ts
// 变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型
let something;
something = 'seven';
something = 7;
something.setName('Tom');

// 等价于
let something: any;
something = 'seven';
something = 7;
something.setName('Tom');

```

## 3、类型推论

> 如果没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。

```ts
// 虽然没有指定类型，但是会在编译的时候报错
let myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.

// 等价于
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;
// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

> TypeScript 会在没有明确的指定类型的时候推测出一个类型，这就是类型推论。
>
> **如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 `any` 类型而完全不被类型检查**

```ts
let myFavoriteNumber;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

## 4、联合类型

> 联合类型（Union Types）表示取值可以为多种类型中的一种。

```ts
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
// 
let myFavoriteNumber: string | number;
myFavoriteNumber = true;
// index.ts(2,1): error TS2322: Type 'boolean' is not assignable to type 'string | number'.
//   Type 'boolean' is not assignable to type 'number'.
```

联合类型使用 `|` 分隔每个类型。

这里的 `let myFavoriteNumber: string | number` 的含义是，允许 `myFavoriteNumber` 的类型是 `string` 或者 `number`，但是不能是其他类型。

**访问联合类型的属性或方法**

当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们**只能访问此联合类型的所有类型里共有的属性或方法**：

```ts
// length 不是 string 和 number 的共有属性，所以会报错
function getLength(something: string | number): number {
    return something.length;
}

// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```

```ts
// 访问 string 和 number 的共有属性是没问题的
function getString(something: string | number): string {
    return something.toString();
}
```

```ts
// 联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
console.log(myFavoriteNumber.length); // 5  myFavoriteNumber 被推断成了 string，访问它的 length 属性不会报错
myFavoriteNumber = 7;
console.log(myFavoriteNumber.length); // 编译时报错  被推断成了 number，访问它的 length 属性时就报错了

// index.ts(5,30): error TS2339: Property 'length' does not exist on type 'number'.
```

## 5、对象的类型--接口

> 在 TypeScript 中，我们使用接口（Interfaces）来定义对象的类型

### 什么是接口

> 在面向对象语言中，接口（Interfaces）是一个很重要的概念，它是对行为的抽象，而具体如何行动需要由类（classes）去实现（implement）。
>
> TypeScript 中的接口是一个非常灵活的概念，除了可用于[对类的一部分行为进行抽象](https://ts.xcatliu.com/advanced/class-and-interfaces.html#类实现接口)以外（见进阶-类与接口），也常用于对「对象的形状（Shape）」进行描述。
>
> **赋值的时候，变量的形状必须和接口的形状保持一致，可选属性除外**

```ts
// 我们定义了一个接口 Person，接着定义了一个变量 tom，它的类型是 Person。这样，我们就约束了 tom 的形状必须和接口 Person 一致。
interface Person {
	name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 19
}
```

```ts
// 定义的变量比接口少了一些属性是不允许的
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom'
}
// index.ts(6,5): error TS2322: Type '{ name: string; }' is not assignable to type 'Person'.
//   Property 'age' is missing in type '{ name: string; }'.
```

```ts
// 多一些属性也是不允许的
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
	name: 'Tom',
    age: 18,
    gender: 'male'
}
//// index.ts(9,5): error TS2322: Type '{ name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Object literal may only specify known properties, and 'gender' does not exist in type 'Person'.
```

### 可选属性

> 有时我们希望不要完全匹配一个形状，那么可以用可选属性
>
> 可选属性的含义是该属性可以不存在

```ts
interface Person {
    name: string;
    age?: number;
}
let tom: Person = {
    name: 'tom'
}

interface Person {
	name: string;
    age?: number
}
let tom: Person = {
    name: 'tom',
    age: 18
}
```

```ts
// 这时仍然不允许添加未定义的属性
interface Person {
    name: string;
    age?: number;
}
let tom: Person = {
    name: tom,
    age: 18,
    gender: 'male'
}
// examples/playground/index.ts(9,5): error TS2322: Type '{ name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Object literal may only specify known properties, and 'gender' does not exist in type 'Person'.
```

### 任意属性

> 有时候我们希望一个接口允许有任意的属性，可以使用`[propName: string]` 定义了任意属性取 `string` 类型的值

```ts
interface Person {
    name: string;
    age?: number;
    [propName: string]: any
}
let tom: Person = {
    name: 'tom',
    gender: 'male'
}
```

```ts
// 需要注意的是，一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集：
// 任意属性的值允许是 string，但是可选属性 age 的值却是 number，number 不是 string 的子属性，所以报错了
interface Person {
    name: string;
    age?: number;
    [propName: string]: string
}
let tom: Person = {
    name: 'tom',
    age: 18,
    gender: 'male'
}
// index.ts(3,5): error TS2411: Property 'age' of type 'number' is not assignable to string index type 'string'.
// index.ts(7,5): error TS2322: Type '{ [x: string]: string | number; name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Index signatures are incompatible.
//     Type 'string | number' is not assignable to type 'string'.
//       Type 'number' is not assignable to type 'string'.
```

```ts
// 一个接口中只能定义一个任意属性。如果接口中有多个类型的属性，则可以在任意属性中使用联合类型
interface Person {
    name: string;
    age?: number;
    [propName: string]: string | number
}
let tom: Person = {
    name: 'tom',
    age: 18,
    gender: 'male'
}
```

### 只读属性

> 有时候我们希望对象中的一些字段只能在创建的时候被赋值，那么可以用 `readonly` 定义只读属性：

```ts
// 使用 readonly 定义的属性 id 初始化后，又被赋值了，所以报错了
interface Person {
    readonly id: number;
    name: string;
    age?: number,
    [propName: string]: string | number
}
let tom: Person = {
    id: 1234,
    name: 'tom',
    age: 18,
    gender: 'male'
}
tom.id = 5555;
// index.ts(14,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.

```

> **只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候**

```ts
// 报错信息有两处，第一处是在对 tom 进行赋值的时候，没有给 id 赋值
// 第二处是在给 tom.id 赋值的时候，由于它是只读属性，所以报错了
interface Person {
    readonly id: number;
    name: string;
    age?: number;
    [propName: string]: string | number
}
let tom: Person = {
    name: 'tom',
    gender: 'male'
}
tom.id = 8888;
// index.ts(8,5): error TS2322: Type '{ name: string; gender: string; }' is not assignable to type 'Person'.
//   Property 'id' is missing in type '{ name: string; gender: string; }'.
// index.ts(13,5): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```

## 6、数组的类型

> **同一类型数据的集合**

### **类型 + 方括号 表示法**

```ts
// 类型 + 方括号 string[]
let arr: number[] = [1,2,3,4,5];
// 不允许出现其他类型
let arr: number[] = [1,2,3,'4',5];
// Argument of type '"8"' is not assignable to parameter of type 'number'.
```

```ts
// 数组的一些方法的参数也会根据数组在定义时约定的类型进行限制
let arr： number[] = [1,2,3,4];
arr.push(5);
arr.push('6'); // 报错
// Argument of type '"8"' is not assignable to parameter of type 'number'.
```

### 数组泛型 表示法

> 可以使用数组泛型（Array Generic） `Array<elemType>` 来表示数组

```ts
let arr: Array<number> = [1,2,4,5];
let arr: Array<string> = ['a','b'];
```

### 用接口表示数组

```ts
interface Arr {
    [index: number]: number
}
let arr: Arr = [1,2,3,4,5];
//
interface Arr1 {
    [index: number]: string
}
let str: Arr1 = ['a','b']
```

### 类数组

类数组（Array-like Object）不是数组类型，比如 `arguments`

```ts
function sum() {
    let args: number[] = arguments;
}
// Type 'IArguments' is missing the following properties from type 'number[]': pop, push, concat, join, and 24 more.
```

`arguments` 实际上是一个类数组，不能用普通的数组的方式来描述，而应该用接口

```ts
function sum() {
    let args: {
        [index: number]: number;
        length: number;
        callee: Function;
    } = arguments;
}
//除了约束当索引的类型是数字时，值的类型必须是数字之外，也约束了它还有 length 和 callee 两个属性。
```

```ts
// 事实上常用的类数组都有自己的接口定义，如 IArguments, NodeList, HTMLCollection 等
function sum () {
	let args: IArguments = arguments;
}
// 其中 IArguments 是 TypeScript 中定义好了的类型,相当于
interface IArguments {
    [index: number]: any;
    length: number;
    callee: Function;
}

```

### any 在数组中的应用

用 `any` 表示数组中允许出现任意类型

```ts
let arr: any[] = ['a','b',1,3,4,true,{a:1,b:2}];
```



## 7、函数类型

> 在 JavaScript 中，有两种常见的定义函数的方式——函数声明（Function Declaration）和函数表达式（Function Expression）

```js
// 函数声明（Function Declaration）
function sum(x, y) {
    return x + y;
}

// 函数表达式（Function Expression）
let mySum = function (x, y) {
    return x + y;
};
```

### 函数声明

> 一个函数有输入和输出，要在 TypeScript 中对其进行约束，需要把输入和输出都考虑到，其中函数声明的类型定义较简单

```ts
function sum(x: number, y: number): number {
    return x + y;
}
```

> **输入多余的（或者少于要求的）参数，是不被允许的**

```ts
function sum(x: number,y: number):number{
    return x + y
}
sum(1,2,3);
// index.ts(4,1): error TS2346: Supplied parameters do not match any signature of call target

function sum(x: number,y: number):number{
    return x + y
}
sum(1);
// index.ts(4,1): error TS2346: Supplied parameters do not match any signature of call target.
```



### 函数表达式

```ts
// 如果要我们现在写一个对函数表达式（Function Expression）的定义，可能会写成这样
let mySum = function (x: number, y: number): number {
    return x + y;
};
// 这是可以通过编译的，不过事实上，上面的代码只对等号右侧的匿名函数进行了类型定义，而等号左边的 mySum，是通过赋值操作进行类型推论而推断出来的
```

> 如果需要我们手动给 `mySum` 添加类型，则应该是这样：

```ts
let mySum(x: number,y: number) => number = function(x: number,y: number): number{
    return x + y;
}
```

**在 TypeScript 的类型定义中，`=>` 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型**

**在 ES6 中，`=>` 叫做箭头函数，应用十分广泛**



### 用接口定义函数的形状

> 我们也可以使用接口的方式来定义一个函数需要符合的形状

```ts
interface SearchFunc {
    (source: string,subString: string): boolean;
}

let mySearch: Search;
mySearch = function(source: string,subString: string) {
    return source.search(subString) !==-1;
}
```

> 采用函数表达式|接口定义函数的方式时，对等号左侧进行类型限制，可以保证以后对函数名赋值时保证参数个数、参数类型、返回值类型不变

### 可选参数

> 前面提到，输入多余的（或者少于要求的）参数，是不允许的，我们用 `?` 表示可选的参数

```ts
function buildName(firstName: string, lastName?: string){
    if(firstName){
        return firstName + '' + lastName;
    }else{
        return lastName;
    }
}
let tomcat = buildName('Tom','Cat');
let tom = buildName('Tom');
```

```ts
// 可选参数后面不允许再出现必需参数了
function buildName(firstName?: string, lastName: string) {
    if (firstName) {
        return firstName + ' ' + lastName;
    } else {
        return lastName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName(undefined, 'Tom');

// index.ts(1,40): error TS1016: A required parameter cannot follow an optional parameter.
```



### 参数默认值

> 在 ES6 中，我们允许给函数的参数添加默认值，**TypeScript 会将添加了默认值的参数识别为可选参数**

```ts
function buildName(firstName: string, lastName: string = 'Cat') {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```



```ts
// 此时就不受「可选参数必须接在必需参数后面」的限制了
function buildName(firstName: string = 'Tom', lastName: string) {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let cat = buildName(undefined, 'Cat');
```

### 剩余参数

> ES6 中，可以使用 `...rest` 的方式获取函数中的剩余参数（rest 参数)

```ts
function push(array, ...items) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a: any[] = [];
push(a, 1, 2, 3);
```

> 事实上，`items` 是一个数组。所以我们可以用数组的类型来定义它

```ts
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```

**注意，rest 参数只能是最后一个参数**

### 重载

> 重载允许一个函数接受不同数量或类型的参数时，作出不同的处理

> 比如，我们需要实现一个函数 `reverse`，输入数字 `123` 的时候，输出反转的数字 `321`，输入字符串 `'hello'` 的时候，输出反转的字符串 `'olleh'`

```ts
// 利用联合类型，我们可以这么实现
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

**然而这样有一个缺点，就是不能够精确的表达，输入为数字的时候，输出也应该为数字，输入为字符串的时候，输出也应该为字符串。**

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
//上例中，我们重复定义了多次函数 reverse，前几次都是函数定义，最后一次是函数实现。在编辑器的代码提示中，可以正确的看到前两个提示。

//注意，TypeScript 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面。
```









# 5、进阶



## 3、元组

不同类型数据的集合

数组合并了相同类型的对象，而元组（Tuple）合并了不同类型的对象。

元组起源于函数编程语言（如 F#），这些语言中会频繁使用元组。

```ts
let arr: [numbre,string] = [1,'a'];
// 当赋值或访问一个已知索引的元素时，会得到正确的类型
let tom : [string,number];
tom[0] = 'Tom';
tim[1] = 25;

tom[0].slice(1);
tom[1].toFixed(2);
```

```ts
// 可以只赋值其中一项
let tom: [string, number];
tom[0] = 'Tom';
```

当 **直接** 对元组类型的变量进行初始化或者赋值的时候，需要提供所有元组类型中指定的项

```ts
let tom: [string, number];
tom = ['Tom',18];

let Tom1: [string, number];
Tom1 = ['tom']; // 报错
// Property '1' is missing in type '[string]' but required in type '[string, number]'.
```

**越界的元素**

```ts
// 当添加越界的元素时，它的类型会被限制为元组中每个类型的联合类型
let tom: [string, number];
tom = ['Tom',18];
tom.push('male');
tom.push(true); // 报错
// Argument of type 'true' is not assignable to parameter of type 'string | number'.
```

定义元组未直接赋值，需要关闭严格检查模式  strictNullChecks：true

```json
{
    "complierOptions" : {
        'target': 'es6',
        'watch': true,
        outDir: './dist',
        // strictNullChecks: true
    }
    include: ['./src/**/*']
}
```



## 4、枚举

> 枚举可以是数字类型枚举，也可以是字符串类型枚举；
>
> 数字类型枚举第一个数字不赋值默认为0；
>
> 后边的数字不赋值默认在前一个数字上加一

```ts
enmu RTCTag {
    RONGCLUDE = 'rongrtc',
    SCREENSHARE = 'screenshare'
}
```





















































