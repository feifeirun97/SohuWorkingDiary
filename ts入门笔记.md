## Ts基础

### 基础类型

```ts
//unknow可以避免潜在风险
function divide(param: unknown) {
  return param as number / 2; 
}
//never
function fn(msg: string): never {
  throw new Error(msg);
}
```

Boolean, number & string

Undefined & null

Any, unknown & void

Never

### 数组类型

```tsx
//可以说string[]等
let list: number[] =[1,2,3]
```

### 元组类型

tuple并不意味2个元素列表。tuple指定类型都能push进去

```tsx
let tuple: [number, string] = [18, 'fri']
//声明越界报错
let tuple2:  [number, string] = [18, 'fri', 100]
//push指定类型不报错
tuple.push(123)
```

### 函数类型

需要定义输入输出类型, 有时输出类型可以忽略因为ts能根据返回语句自动推断返回类型

未明确返回值默认回void类型

```tsx
function add(x:number, y:number):number { return x+y }
add(1+2)
//默认void返回类型
function welcome():void { console.log('hello') }
//es6写法
let add2 = (x:number, y:number): number => { return x+y }
```

#### 赋值

ts中函数不能随便赋值会报错

```tsx
add = 2 //error
```

#### 重载

函数相同但是参数type不同, 不需要把相似功能函数拆分成多个函数 [js会直接覆盖函数]

```tsx
let add = (x:number[]):number => { return x.reduce((acc, cur) => acc+cur )}
let add = (x:string[]):string => { return x.join() }

function add(x:number[]):number {
  return x.reduce((acc, cur) => acc+cur )
}
function add(x:string[]):string {
  return x.join()
}
```

#### 可选参数?

注意可选参数要放在入参后面不然报错

```tsx
function add(x:number, y:number, z?:number):number { return x+y }
```

#### 默认参数

```tsx
function add(x:number, y:number = 100):number { return x+y }
```



### 接口Interface

#### 定义

用于定义对象类型, 首字母大写. *声明赋值时不得多/少写属性

```tsx
interface Person {
  name:string,
  age:number,
}
const p1: Person = {name:'fei', age:24}
```

#### 可选属性?

```tsx
interface Person {name:string, age?:number}
```

#### 只读属性

```tsx
interface Person {readonly id:number}
```

#### 描述函数类型

```tsx
interface ISum { (x:number, y:number):number }
const add:ISum = (num1, num2) => return num1+num2
```

#### 自定义属性

同对象上有多个不确定属性时

```tsx
interface RandomKey {
  [propName: string]: string //对key和value都有约束
}
const obj: RandomKey = {
  a:'hello',
  b:'lin',
  c:'welcome'
}
```

#### Duck Typing

只要满足interface定义的类型ts都能通过m, 所以接口可以非常灵活.

```tsx
//用借口描述了函数类型, 还向函数类型添加了name属性
interface FuncWithProps {
  (x:number): number //function
  name: string
}
const fn: FuncWithProps = (x) => return x
fn.name = 'hello'
```

> 实际上React的函数式组件就是这么写的

```tsx
interface FunctionComponent<P = {}> {
  (props: PropsWithChildren<P>, context?: any): ReactElement<any, any> | null;
  propTypes?: WeakValidationMap<P> | undefined;
  contextTypes?: ValidationMap<any> | undefined;
  defaultProps?: Partial<P> | undefined;
  displayName?: string | undefined;
}
```



### 类class

ts通过`public`, `private`, `protected`三个修饰符来增强了JS中的类

#### 基本写法

```tsx
class Person {
  //name规定两次type
  name: string
  constructor(name:string) { this.name = name }
  speak() { console.log(`${this.name} is speaking`) }
}
const p1 = new Person('lin')
p1.name; p1.speak()
```

#### 继承

```tsx
class Student extends Person {
	study() { console.log(`${this.name} needs study`) }
}
const s1 = new Student('hao')
s1.study()
```

#### super

```tsx
//操作继承类属性
class Student extends Person {
  grade: number
  constructor(name: string, grade:number) {
    super(name)
    this.grade = grade
  }
}
const s1 = new Student('lin'. 60)
```

#### 多态

```tsx
//子类对父类方法重写, 但不会影响父类
class Student extends Person {
  speak() { return `Student ${super.speak()}`}
}
```

#### Public

公有的, 一个类里默认的所有方法属性都是public. 写与不写都可以

#### Private

私有的, 它的实例和继承它的子类都访问不到

```tsx
class Person {
  public name:string //public写不写都行
  private age:number
  //..
}
```

#### Protected

受保护的, 只有类和继承的子类可以访问, 实例不能访问

```tsx
class Person {
    protected name: string
    public constructor(name: string) { this.name = name }
    public speak() { console.log(`${this.name} is speaking`) }
}
const p1 = new Person('lin')
p1.name //error
p1.speak() //ok
class Student extends Person {
	study() { console.log(`${this.name} needs study`) }
}
const s1 = new Student('hao')
s1.name //error 
s1.study() //ok
```

#### Static

静态属性, 类上的一些常量, 实例和子类都不能访问

```tsx
class Circle {
  static pi: 3.14
  public radius: number
  public constructor(radius: number) { this.radius = radius }
  public calcLength { return Circle.pi * this.radius *2 }
}
```





//受控 与非受控组建的区别
