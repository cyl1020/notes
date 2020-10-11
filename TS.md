### 一.编译

#### 1.Typescript安装 编译

```js
//安装
npm install -g typescript
```

```js
//编译
tsc helloworld.ts
```

#### 2.vscode配置自动编译

- 第一步： tsc --init 生成tsconfig.json  改"outDir": "./js"
- 第二步：终端  --> 运行任务 --> typescript --> tsc:监视 - tsconfig.json

### 二.数据类型

> typescript中为了使编写的代码更规范，更有利于维护，增加了类型校验（写ts代码必须指定类型），在typescript中主要给我们提供了以下的数据类型

#### 1.布尔类型（boolean）

```js
//布尔类型
let flag: boolean = true;
```

#### 2.数字类型（number）

```js
//数字类型
let num: number = 123;
```

#### 3.字符串类型（string）

```js
//字符串类型
let str: string = "cba";
```

#### 4.数组类型（array）

```js
//数组类型
//第一种定义数组的方式
	let arr1: number[] = [11, 22, 33]; //这里数组里的元素必须全是数字

//第二种定义数据的方式
let arr2: Array<number> = [11, 22, 33]; //这里数组里的元素必须全是数字

//第三种定义数据的方式
let arr3: any[] = [132, "abc", true];
```

#### 5.元组类型（tuple）

```js
//元组类型: 属于数组的一种
let arr4: [number, string] = [123, "cba"];
```

#### 6.枚举类型（enum）

```js
//枚举类型
/* 
  enum 枚举名 {
    标识符 [=整型常数],
    标识符 [=整型常数],
    ...
    标识符 [=整型常数],
  }
*/
enum Flag {
  success = 1,
  error = 2,
}

let f: Flag = Flag.success; //f的值为1

enum Color {
  blue,
  red,
  "pink",
}
let c: Color = Color.red; // 如果标识符没有赋值，他的值就是下标(这里c为1)
```

#### 7.任意类型（any）

```js
//任意类型
let num1: any = 123;
num1 = "str";
num1 = true;
//任意类型的用处：
let box: any = document.getElementById("box");
box.style.color = "red";
```

#### 8.null 和 undefined

```js
//null 和 undefined 其他（never类型） 数据类型的子类型
let num2: number;
// console.log(num2); //输出：undefined  报错

let num3: undefined;
console.log(num3); //输出：undefined  正确

//定义了没有赋值就是undefined
let num4: number | undefined;

let num5: null;

//一个元素可能是number类型 可能是null类型 可能是undefined类型
let num6: number | null | undefined;
```

#### 9.void类型

```js
//void类型：typescript中的void类型表示没有任何类型，一般用于定义方法的时候方法没有返回值
function run(): void { //表示方法没有返回任何类型
  console.log("run");
}

function run1(): number {
  return 123;
}
```

#### 10.never类型

```js
//never类型：代表从不会出现的值
//这意味着声明never的变量只能被never类型所赋值
let a: never;
a = (() => {
  throw new Error("错误");
})();
```

### 三.函数

#### 1.定义函数的方法

- 声明函数法

  ```js
  function run2(): string {
    return "run";
  }
  ```

- 匿名函数

  ```js
  const run3 = function (): string {
    return "run";
  };
  ```

#### 2.ts中定义方法传参

```js
function getInfo1(name: string, age: number): string {
  return `${name} --- ${age}`;
}
console.log(getInfo1("zhangsan", 18));

const getInfo2 = function (name: string, age: number): string {
  return `${name} --- ${age}`;
};
console.log(getInfo2("zhangsan", 18));

//没有返回值的方法
function run4(): void {
  console.log("err");
}
run4();
```

#### 3.方法可选参数

```js
//es5里面方法的实参和形参可以不一样，但是ts中必须一样，如果不一样就需要配置可选参数
//某个参数加了?号就表示该参数可传可不传
const getInfo3 = function (name: string, age?: number): string {
  if (age) {
    return `${name} --- ${age}`;
  } else {
    return `${name} --- 年龄保密`;
  }
};
console.log(getInfo3("zhangsan"));
//注意：可选参数必须配置到参数的最后面
```

#### 4.默认参数

```js
const getInfo4 = function (name: string, age: number = 20): string {
  if (age) {
    return `${name} --- ${age}`;
  } else {
    return `${name} --- 年龄保密`;
  }
};
console.log(getInfo4("zhangsan"));
```

#### 5.剩余参数

```js
function sum1(a: number, b: number, c: number, d: number): number {
  return a + b + c + d;
}
console.log(sum1(1, 2, 3, 4));

//三点运算符：接受形参传过来的值
function sum2(...props: number[]): number {
  let sum = 0;
  for (let i = 0; i < props.length; i++) {
    sum += props[i];
  }
  return sum;
}
console.log(sum2(1, 2, 3, 4));

//三点运算符的另一种写法
function sum3(a: number, b: number, ...props: number[]): number {
  let sum = a + b;
  for (let i = 0; i < props.length; i++) {
    sum += props[i];
  }
  return sum;
}
console.log(sum3(1, 2, 3, 4, 5, 3, 6, 6, 6));
```

#### 6.函数重载

```js
//typescript中的重载，通过为同一个函数提供多个函数类型定义来实现多种功能的目的
//es5中出现同名的方法，下面的会将上面的替换
// function css(config) {}

// function css(config, value) {}

//ts中的重载
function info(name: string): string;
function info(age: number): number;
function info(str: any): any {
  if (typeof str === "string") {
    return "我叫：" + str;
  } else {
    return "我的年龄是：" + str;
  }
}
console.log(info("zhangsan"));
console.log(info(20));
```

#### 7.箭头函数

```js
//箭头函数中的this指向上下文
setTimeout(() => {
  console.log(111);
}, 1000);
```

### 四.类

#### 1.ts中定义类

```js
class Person1 {
  name: string; //属性   前面省略了public关键词

  constructor(n: string) {
    //构造函数  实例化类的时候触发的方法
    this.name = n;
  }
  run(): void {
    console.log(this.name);
  }
  getName(): string {
    return this.name;
  }
  setName(name: string): void {
    this.name = name;
  }
}

let p1 = new Person1("zhangsan");
p1.run();
console.log(p1.getName());
p1.setName("lisi");
console.log(p1.getName());
```

#### 2.ts中实现继承 

> 关键字：extends super

```js
class Person2 {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  run(): string {
    return `${this.name}在运动`;
  }
}

class Web2 extends Person2 {
  constructor(name: string) {
    super(name); //初始化父类的构造函数
  }
}

let web2 = new Web2("lisi");
console.log(web2.run());
```

#### 3.ts中继承的探讨

>  父类的方法和子类的方法一致时，遵循就近原则

```js
class Person3 {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  run(): string {
    return `${this.name}在运动`;
  }
}

class Web3 extends Person3 {
  constructor(name: string) {
    super(name); //初始化父类的构造函数
  }
  run(): string {
    return `${this.name}在工作`;
  }
}

let web3 = new Web3("lisi");
console.log(web2.run()); //lisi在工作
```

#### 4.修饰符

> 类里面的修饰符 typescript里面定义属性的时候给我们提供了三种修饰符
>
> <span style="color: red">注：属性如果不加修饰符，默认就是共有（public）</span>

##### (1).public（共有）

> 在类里面、子类、类外面都可以访问

```js
class Person4 {
  public name: string; //共有属性
  constructor(name: string) {
    this.name = name;
  }
  run(): string {
    return `${this.name}在运动`; //可以访问name
  }
}

class Web4 extends Person5 {
  constructor(name: string) {
    super(name); //初始化父类的构造函数
  }
  work(): string {
    return `${this.name}在工作`; //可以访问name
  }
}

let p4 = new Person4('哈哈哈')
console.log(p4.name); //可以访问name
```

##### (2).protected（保护）

> 在类里面、子类里面可以访问，在类外面没法访问

```js
class Person5 {
  protected name: string; //保护属性
  constructor(name: string) {
    this.name = name;
  }
  run(): string {
    return `${this.name}在运动`; //可以访问name
  }
}

class Web5 extends Person5 {
  constructor(name: string) {
    super(name); //初始化父类的构造函数
  }
  work(): string {
    return `${this.name}在工作`; //可以访问name
  }
}
let p5 = new Person5('哈哈哈')
console.log(p5.name); //不可以访问name
```

##### (3).provate（私有）  

>  在类里面可以访问，在子类、类外部都没法访问

```js
class Person6 {
  private name: string; //私有属性
  constructor(name: string) {
    this.name = name;
  }
  run(): string {
    return `${this.name}在运动`; //可以访问name
  }
}

class Web6 extends Person6 {
  constructor(name: string) {
    super(name); //初始化父类的构造函数
  }
  work(): string {
  	return `${this.name}在工作`; //不可以访问name
  }
}

let p6 = new Person6('哈哈哈')
console.log(p6.name); //不可以访问name
```

#### 5.静态属性 静态方法

```js
class Preson7 {
  public name: string;

  static age: number = 20; //静态属性
  constructor(name: string) {
    this.name = name;
  }

  run(): string {
    //实例方法
    return `${this.name}在运动`;
  }

  static print(): void {
    //静态方法 里面没有办法直接调用类里面的属性（没有this）
    console.log("这是一个静态方法" + Preson7.age);
  }
}

//静态方法的调用
Preson7.print();
```

#### 6.多态

> 父类定义一个方法不去实现，让继承它的子类去实现 每一个子类有不同的表现
>
> 多态属于继承

```js
class Animal {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  eat() {
    //动物机具体吃什么不知道   具体吃什么由它的子类去实现，每一个子类表现不一样
    console.log("吃的方法");
  }
}

class Dog extends Animal {
  constructor(name: string) {
    super(name);
  }

  eat() {
    return this.name + "吃粮食";
  }
}

class Cat extends Animal {
  constructor(name: string) {
    super(name);
  }

  eat() {
    return this.name + "吃老鼠";
  }
}

const dog = new Dog("ccc");
console.log(dog.eat());
```

#### 7.抽象类

> typescript中的抽象类，他是提供其他类继承的基类，不能直接实例化
>
> 用abstract关键字定义抽象类和抽象方法，抽象类中的抽象方法不包含具体实现并且必须在派生类中实现
>
> abstract抽象方法只能放在抽象类里面
>
> 抽象类和抽象方法用来定义标准

```js
abstract class Animal2 {
  public name: string;
  constructor(name: string) {
    this.name = name;
  }
  abstract eat(): void;
}

// const a = new Animal2() 错误的写法
class Dog2 extends Animal2 {
  //抽象类的子类必须实现抽象类里面的方法
  constructor(name:string) {
    super(name)
  }
  eat() {
    console.log(this.name + '吃粮食');
  }
}

const d = new Dog2('小黑')
d.eat()
```

### 五.接口

> 接口，行为和动作的规范，对批量方法进行约束

#### 1.属性类接口

> 属性类接口：就是对传入属性的约束

##### (1).对批量方法传入参数进行约束

```js
interface FullName1 {
  firstName: string; //注意：以分号结束
  lastName: string;
}
function printName(name: FullName1) {
  //必须传入对象 对象中必须有firstName lastName
  console.log(name.firstName + "--" + name.lastName);
}

const obj = { firstName: "name", lastName: "xxx" };
printName(obj);
```

##### (2).可选属性

```js
interface FullName2 {
  firstName: string; //注意：以分号结束
  lastName?: string; //加？就说明该属性可有可无
}

function getName(name: FullName2) {
  console.log(name);
}

getName({
  firstName: 'fafs'
})
```

##### (3).原生js封装ajax

```js
interface Config {
  type: string;
  url: string;
  data?: string;
  dataType: string;
}

function ajax(config: Config) {
  const xhr = new XMLHttpRequest();
  xhr.open(config.type, config.url, true);
  xhr.send(config.data);
  xhr.onreadystatechange = function () {
    if (xhr.readyState === 4 && xhr.status === 200) {
      console.log("成功");
      if (config.dataType === "json") {
        JSON.parse(xhr.responseText);
      } else {
        console.log(xhr.responseText);
      }
    }
  };
}

ajax({
  type: "get",
  url: "https://www.baidu.com",
  dataType: "json",
});
```

#### 2.函数类型接口

> 函数类型接口：对方法传入的参数，以及返回值进行约束 可以批量约束

```js
//加密的函数类接口
interface encrypt {
  (key: string, value: string): string;//传入的两个参数必须都是字符串类型，并且函数返回值必须是字符串
}

const md5: encrypt = function (key: string, value: string): string {
  //模拟操作
  return key + value;
};
console.log(md5('name', 'wangwu'));
```

#### 3.可索引接口

> 对数组 对象的约束（不常用）

##### (1).对数组的约束

```js
interface UserArr {
  [index: number]: string; //索引值必须是数字，值必须是字符串
}

let arr: UserArr = ["123123", "16548456156"];
// let arr11:UserArr = [123123, '16548456156'] //错误写法
console.log(arr[0]);
```

##### (2).对对象的约束

```js
interface UserObj {
  [index: string]: string; //键为字符串，值也是字符串
}

let obj11: UserObj = { name: "张三" };
```

#### 4.类类型接口

> 对类的约束 和抽象类抽象方法有点相似

```js
interface Animal3 {
  name: string;
  eat(str: string): void; //这里参数可以不传，但是方法必须实现
}
class Dog1 implements Animal3 {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  eat(): void {
    console.log(this.name + "吃粮食");
  }
}

let dog1 = new Dog1('xx')
dog1.eat()


class Cat1 implements Animal3 {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  eat(food:string): void {
    console.log(this.name + "吃" + food);
  }
}

let cat1 = new Cat1('小花')
cat1.eat('老鼠')
```

#### 5.扩展类接口

> 接口类扩展：接口可以继承接口

```js
interface Animal4 {
  eat(): void;
}

//Person接口相当于有两个方法需要被约束的类去实现
interface Person extends Animal4 {
  work(): void;
}

class Programmer {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  coding(code: string): void {
    console.log(this.name + code);
  }
}

//Web类必须实现Person的两个方法，一个是Person自己的work方法，另一个是Person继承于Animal4的eat方法
//类不仅可以继承类还可以实现接口
class Web extends Programmer implements Person {
  constructor(name: string) {
    super(name)
  }
  eat() {
    console.log(this.name + "喜欢吃馒头");
  }
  work() {
    console.log(this.name + "写代码");
  }
}
let w = new Web("小李");
w.work();
w.coding('是一个古币的程序员')
```

### 六.泛型

> 通俗理解：泛型就是解决 类 接口 方法的复用性，以及对不特定的数据类型的支持

#### 1.泛型的定义

##### (1).引入

```js
//只能返回string类型的数据
function getData1(value: string): string {
  return value
}

//同时返回string类型 和 number类型(代码冗余)
function getData2(value: string): string {
  return value
}
function getData3(value: number): number {
  return value
}

//同时返回string类型 和 number类型 使用any类型可以解决这个问题 
function getData4(value: any): any {
  return value
}

//使用any类型相当于放弃了类型检查 要求是传入什么类型就返回什么类型。比如传入number类型就返回number类型
//使用any类型 传入的参数类型和返回的参数类型可以不一致，不满足设计需要求
function getData5(value: any): any {
  return '哈哈哈'
}

//怎么解决这个问题？
//下面的定义的泛型可以很好的解决这个问题
```

##### (2).定义(泛型函数)

> 泛型：可以支持不特定的数据类型(类型校验)    要求：传入的参数类型和返回的参数类型一致

```js
//T表示泛型，具体是什么类型是调用这个方法的时候决定的
function getData6<T>(value:T):T {
  return value
}

getData6<number>(123)
getData6<string>('123')
// getData6<number>('123') //错误写法
```

#### 2.泛型类

```js
class MinClass<T> {
  // public list:Array<T> = []
  public list:T[] = []

  add(value:T):void {
    this.list.push(value)
  }

  min():T {
    let minNum = this.list[0]
    for(let i = 0; i < this.list.length; i++) {
      if (minNum > this.list[i]) minNum = this.list[i]
    }
    return minNum
  }
}

let m1 = new MinClass<number>() //实例化对象，并且定制了类的T代表的类型是number

m1.add(123)
m1.add(12)
m1.add(1)
m1.add(1232)
console.log(m1.min());

let m2 = new MinClass<string>() //实例化对象，并且定制了类的T代表的类型是string

m2.add('a')
m2.add('b')
m2.add('c')
m2.add('d')
console.log(m2.min());
```

#### 3.泛型接口

```js
//方法一
interface ConfigFn2 {
  <T>(value:T):T
}

let setData2:ConfigFn2 = function <T>(value:T):T {
  return value
}

console.log(setData2<string>('李四'));


//方法二
interface ConfigFn3<T> {
  (value:T):T
}

// let setData3:ConfigFn3<string> = function <T>(value:T):T {
//   return value
// }

function setData4<T> (value:T):T {
  return value
}

let getData:ConfigFn3<string> = setData4

console.log(getData('李四'));
```

#### 4.补充：把类作为参数类型的泛型类

```js
//把类作为参数来约束数据传入的类型
class User {
  username: string | undefined;
  password: string | undefined;
}

class MysqlDb {
  add(user: User): boolean {
    console.log(user);
    return true;
  }
}
let u = new User();
u.username = "张三";
u.password = "123456";

let Db = new MysqlDb();
Db.add(u);
```

```js
//操作数据库的泛型类
class Sql<T> {
  add(info: T): boolean {
    console.log(info);
    return true;
  }

  update(info: T, id:number):boolean {
    console.log(info);
    console.log(id);
    return true
  }
}

//1.定义一个MyUser类 和数据库进行映射
class MyUser {
  username: string | undefined;
  password: string | undefined;
}

let mu = new MyUser();
mu.username = "李四";
mu.password = "123";
let mdb = new Sql<MyUser>();
mdb.add(mu);

//2.定义一个ArticleCategory类 和数据库进行映射
class ArticleCategory {
  title: string | undefined;
  desc: string | undefined;
  status: number | undefined;
  constructor(param: {
    title: string | undefined;
    desc: string | undefined;
    status?: number | undefined;
  }) {
    this.title = param.title;
    this.desc = param.desc;
    this.status = param.status;
  }
}

let art = new ArticleCategory({
  title: "文章",
  desc: "很好",
  status: 1,
});

let db = new Sql<ArticleCategory>();
//添加数据
db.add(art);

//修改数据
art.status = 123
db.update(art, 456)
```

#### 5.泛型的综合练习

> 功能：定义一个操作数据库的库 支持Mysql Mssql Mongodb
>
> 要求：Mysql Mssql Mongodb功能一样 都有 add update delete get 方法
>
> 注意：约束统一的规范、以及代码重用所以用到泛型
>
> 解决方案：需要约束规范，所以要定义接口，需要重用代码所以用到泛型
>
> ​		1.接口：在面向对象的编程中，接口是一种规范的定义，它定义了行为和动作的规范
>
> ​		2.泛型：通俗的理解，泛型就是解决 类 接口 方法的复用性

```js
interface DBI<T> {
  add(info: T): boolean;
  update(info: T, id: number): boolean;
  delete(id: number): boolean;
  get(id: number): any[];
}

//定义一个操作MysqlDB数据库的类  注意：要实现泛型接口，这个类也应该是一个泛型类
class MysqlDB<T> implements DBI<T> {
  add(info: T): boolean {
    console.log(info);
    return true
  }
  update(info: T, id: number): boolean {
    throw new Error("Method not implemented.");
  }
  delete(id: number): boolean {
    throw new Error("Method not implemented.");
  }
  get(id: number): any[] {
    throw new Error("Method not implemented.");
  }
}

//定义一个操作MsSqlDB数据库的类 
class MsSqlDB<T> implements DBI<T> {
  add(info: T): boolean {
    console.log(info);
    return true
  }
  update(info: T, id: number): boolean {
    throw new Error("Method not implemented.");
  }
  delete(id: number): boolean {
    throw new Error("Method not implemented.");
  }
  get(id: number): any[] {
    throw new Error("Method not implemented.");
  }
}

//操作用户表  定义一个User类和数据表做映射
class UserData {
  username: string | undefined;
  password: string | undefined;
}

let ud = new UserData()
ud.username = '张三'
ud.password = '123456'

let mysql = new MysqlDB<UserData>()
mysql.add(ud)

let mssql = new MsSqlDB<UserData>()
mssql.add(ud)
```

### 七.模块

> 目录结构
>
> ​		modules
>
> ​			| — db.js
>
> ​		index.js

#### 1.导入导出基础

```js
//db.ts
//方法一
export function getData7(): any[] {
  console.log('获取数据库里的数据');
  return [
    {
      title: '123'
    },
    {
      title: '123'
    }
  ];
}

export function save() {
  console.log('保存数据成功');
}

//方法二
export {getData7, save}
//方法三(对应下面方法三)
export default getData7
```

```js
//index.ts
//方法一
import { getData7, save } from "./modules/db";
getData7();
save();

//方法二
import { getData7 as get, save } from "./modules/db";
get();
save();

//方法三(对应上面方法三)
import getData7 from './modules.db'
```

#### 2.模块化封装DB库

```js
//db.ts
interface DBI<T> {
  add(info: T): boolean;
  update(info: T, id: number): boolean;
  delete(id: number): boolean;
  get(id: number): any[];
}

//定义一个操作MysqlDB数据库的类  注意：要实现泛型接口，这个类也应该是一个泛型类
export class MysqlDB<T> implements DBI<T> {
  add(info: T): boolean {
    console.log(info);
    return true;
  }
  update(info: T, id: number): boolean {
    throw new Error("Method not implemented.");
  }
  delete(id: number): boolean {
    throw new Error("Method not implemented.");
  }
  get(id: number): any[] {
    throw new Error("Method not implemented.");
  }
}
```

```js
//index.ts
//操作用户表  定义一个User类和数据表做映射
class UserData {
  username: string | undefined;
  password: string | undefined;
}

let ud = new UserData();
ud.username = "张三";
ud.password = "123456";
import { MysqlDB } from "./modules/db";
let mysql1 = new MysqlDB<UserData>();
mysql1.add(ud);
```

### 八.命名空间

> 命名空间和模块的区别 
>
>  		命名空间：内部模块，主要用于组织代码，避免命名冲突
>	
>  		模		块：ts的外部模块的简称，侧重代码的复用，一个模块里可能有多个命名空间

```js
namespace A {
  export class Person {
    name: string;
    constructor(name: string) {
      this.name = name
    }
    eat() {
      console.log(this.name + "喜欢吃馒头");
    }
    work() {
      console.log(this.name + "写代码");
    }
  }
}
let pa = new A.Person('张三')
pa.work() //张三写代码
pa.eat() //张三喜欢吃馒头
namespace B {
  export class Person {
    name: string;
    constructor(name: string) {
      this.name = name
    }
    eat() {
      console.log(this.name + "喜欢吃馒头");
    }
    work() {
      console.log(this.name + "写代码");
    }
  }
}
let pb = new B.Person('李四')
pb.work() //李四写代码
pb.eat() //李四喜欢吃馒头
```

### 九.装饰器

> 装饰器是一种特殊类型的声明，它能够被附加到类声明，方法， 属性或参数上，可以修改类的行为
>
>  通俗的讲装饰器就是一个方法，可以注入到类、方法、属性参数上来扩展类、属性、方法、参数的功能
>
>  常见的装饰器有：类装饰器、属性装饰器、方法装饰器、参数装饰器
>
>  装饰器的写法：普通装饰器（无法传参）、装饰器工厂（可以传参）

#### 1.类装饰器

> 类装饰器在类声明之前被声明（紧挨着类声明）。类装饰器应用于类构函数，可以用来监视，修改或替换类定义(传入一个参数)

##### (1).普通装饰器（无法传参）

```js
function logClass1(params: any) {
  console.log(params); //params就是当前类
  params.prototype.url = "动态扩展的属性";
}

@logClass1
class HttpClient1 {
  url: any;
  constructor() {}
  getData() {}
}

let http1 = new HttpClient1();
console.log(http1.url);
```

##### (2).装饰器工厂（可以传参）

```js
function logClass2(params: string) {
  return function (target: any) {
    console.log(params, target); //target是当前类

    target.prototype.url = params;
  };
}
@logClass2("www.baidu.com")
class HttpClient2 {
  constructor() {}
  getData() {}
}
let http2:any = new HttpClient2();
console.log(http2.url);


//修改类
function logClass3(target: any) {
  console.log(target);
  return class extends target {
    url: any = "我是修改后的url";
    getData() {
      console.log(this.url + "----------");
    }
  };
}

@logClass3
class HttpClient3 {
  public url: string | undefined;
  constructor() {
    this.url = "我是构造函数里面的url";
  }

  getData() {
    console.log(this.url);
  }
}

let http3 = new HttpClient3();
http3.getData();
```

#### 2.属性装饰器

> 属性装饰器表达式会在运行的时候当做函数被调用，传入下列两个参数
>
> ​		1.对于静态成员来说是类的构造函数，对于实例成员是类的原型对象
>
> ​		2.成员的名字

```js
function Property(params: string) {
  return function (target: any, attr: any) {
    console.log(target, attr);
    target[attr] = params;
  };
}

class HttpClient4 {
  @Property("www.baidu.com")
  public url: string | undefined;
  constructor() {
    // this.url = "我是构造函数里面的url";
  }

  getData() {
    console.log(this.url);
  }
}

let http4 = new HttpClient4();
http4.getData();
```

#### 3.方法装饰器

> 它会被应用到方法的属性描述符上，可以用来监视，修改或者替换方法定义
>
>  方法装饰会在运行是传入下列3个参数
>
>   		1.对于静态成员来说是类的构造函数，对于实例成员来说是原型对象
>	
>   		2.成员的名字
>	
>   		3.成员的属性描述符

```js
function get(params: any) {
  return function (target: any, methodName: any, desc: any) {
    console.log(target, methodName, desc);
    //添加属性、方法
    target.url = "xxx";
    target.run = function () {
      console.log("run");
    };
    //修改装饰器的方法 把装饰器方法里面传入的所有参数都改为string类型
    //保存当前的方法
    let oMethod = desc.value;
    desc.value = function (...args: any[]) {
      args = args.map((value) => String(value));
      console.log(args);
      oMethod.apply(this, args); //对象冒充实现继承
    };
  };
}

class HttpClient5 {
  run() {}
  public url: string | undefined;
  constructor() {
    // this.url = "我是构造函数里面的url";
  }
  @get("www.baidu.com")
  getData(...args: any[]) {
    console.log("我是getData里面的方法", args);
  }
}

let http5 = new HttpClient5();
console.log(http5.url);
http5.run();
http5.getData(123, "xxx");
```

#### 4.方法参数装饰器

> 参数装饰器表达式会在运行时当做函数来调用，可以使用参数装饰器为类的原型增加一些元素数据，传入三个参数
>
>  		1.对于静态成员来说是类的构造函数，对于实例成员来说是原型对象
>	
>  		2.方法的名字
>	
>  		3.参数装饰器在函数参数列表中的索引

```js
function logParams(params: any) {
  return function(target:any, paramsName: any, index: any) {
    console.log(target, paramsName, index);
    target.url = 'xxx'
  }
}

class HttpClient6 {
  constructor() {
    // this.url = "我是构造函数里面的url";
  }

  getData(@logParams("uuid") uuid: any) {
    console.log("我是getData里面的方法", uuid);
  }
}

let http6:any = new HttpClient6()
http6.getData(13)
console.log(http6.url);
```

#### 5.装饰器执行顺序

- 属性 > 方法 > 方法参数 > 类

- 如果有多个同样的装饰器，它会先执行后面的