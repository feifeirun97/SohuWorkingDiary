##### Interface接口

```typescript
interface User {
  name: string;
  id: number;
}

const user:User = {
  name: "Ethan",
  id: 0
}
```



```typescript
//return type User
function getUSer():User {}
//accept param type User
function deleteUser(user:User) {}
```

#### Composing Types

##### Unions

> declare a type can be one of many types == or

```typescript
type LockState = "locked" | "unlocked"

//function that takes an array or string 
function getLength(obj: string | string[]) {
  return obj.length
}
```

##### Generics

> Provide variables to type

```typescript
type StringArr = Array<string>;
type ObjectWithNameArray = Array<{ name:string }>;

interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}
// This line is a shortcut to tell TypeScript there is a
// constant called `backpack`, and to not worry about where it came from.
declare const backpack: Backpack<string>;
backpack.add('23');
```





### 基础类型basics

+ boolean
+ number
+ string
+ array
+ tuple元组
+ Enum枚举
+ any
+ void
+ never
+ object

```typescript
let isDone: boolean = false;
let age:number = 20;
let name:string = 'bob';
let list:string[] = ['a','b']; let list2:Array<number> = [1,2,3]; // 范型
let x:[number, string] = [1,'a'];
enum Color {Red=1, Green, Blue}; let c:Color = Color.Green;
let notSure:any = 4; let list:any[] = [1, true, 'free'];
function warning(): void { alert('sth') };
function error(msg:string):never { return error(msg) };
let person:object = {1}
```

### 接口interface

> 我们只会去关注值的外形。 只要传入的对象满足上面提到的必要条件，那么它就是被允许的。

```typescript
interface LabelledValue { label: string; }
function printLabel(labelledObj: LabelledValue) {
  console.log(labelledObj.label);
}

let myObj = {size: 10, label: "Size 10 Object"};
printLabel(myObj);
```

+ 可选属性
+ 只读属性 [const用于变量]

```typescript
interface SquareConfig { color?: string;  width?: number; };

interface Point { readonly x:number }; let p1:Point = {x:3}; p1.x=10;//error!




```











