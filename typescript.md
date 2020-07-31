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

