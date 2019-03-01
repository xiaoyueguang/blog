# Typescript 声明入门

使用`TypeScript`开发有一段时间了, 在`vscode`编辑器中使用`TypeScript`能有非常棒的开发体验.  
`TypeScript`大部分的语法与`JavaScript`相一致, 因此并没有多少的学习成本, 反而在编写声明文件的时候花费了不少时间, 因此在本文中, 记录一些声明方式, 方便以后查阅.

## 基础类型
* 字符串 `string`
* 数字 `number`
* 布尔 `boolean`
* 数组 `[]`
* 枚举 `enum`
* 元组 `[type]`
* Any `any`
* Void `void`
* Null `null`
* Undefined `undefined`
* Never `never`
* Object `object`

## 基础类型声明
```typescript
/** 昵称(声明字符串) */
type nickname = string;
/** 年龄(声明数字) */
type age = number;
/** 是否激活账号(声明布尔值) */
type active = boolean;
/** 喜欢的数字(声明数组) */
type likeNumbers = number[];
/** 性别(枚举) */
enum sex {
  male = 1,
  female = 2
}
/** 能力名称 */
type abilityName = string;
/** 能力分值 */
type abilityPower = number;
/** 能力(声明元组) */
type ability = [abilityName, abilityPower]
/** 能力集合(数组的另一种声明方式) */
type abilities = Array<ability>
/** 报名(声明方法) */
type sayName = () => nickname;

/** 用户(声明对象, 接口) */
interface User {
  /** 昵称 */
  nickname: nickname;
  /** 年龄 */
  age: age;
  /** 是否激活 */
  active: active;
  /** 喜欢的数字 */
  likeNumbers?: likeNumbers;
  /** 性别 */
  sex: sex;
  /** 能力 */
  abilities?: abilities;
  /** 报名 */
  sayName: sayName;
}
```

声明之后, 在编写时会有良好的提示和报错提醒.
![提醒](https://ws2.sinaimg.cn/large/006tKfTcly1g0jnmxvup7j3142082q3v.jpg)

![对象缺少类型提醒](https://ws4.sinaimg.cn/large/006tKfTcly1g0jnmxrzynj30ws096ta8.jpg)

![类型错误警告](https://ws3.sinaimg.cn/large/006tKfTcly1g0jnmxnpumj30v00awjtb.jpg)


## 接口
`Interface`接口, 主要是用来声明对象的属性, 描述一个对象所需的属性和对应的值的类型, 同时描述该属性是否必须, 是否只读.

```typescript
interface Animal {
  readonly name: string;
  leg: number;
  run?: () => void
}
```

当有`readonly`标记时, 无法完成额外的赋值操作.

![报错警告](https://ws3.sinaimg.cn/large/006tKfTcly1g0jsezqeijj30om0h0gnj.jpg)

### 可索引类型
可索引类型具有一个索引签名, 描述对象索引的类型以及对应的返回值类型.
```typescript
interface Test {
  [index: string]: number;
  length: number;
  /** 报错 */
  name: string;
}
```
![可索引报错](https://ws4.sinaimg.cn/large/006tKfTcly1g0jsor2vilj30ta06gdgw.jpg)

### 继承
`Typescript`接口可以继承, 获得另一个接口所有的成员, 从而实现接口复用.

```typescript
interface Animal {
  new? (name: string, leg: number);
  name: string;
  leg: number;
  run?: () => void
}
/** 
 * 描述
 * 用于描述性格等其他方面
 */
interface Desc {
  character: string;
}

interface Cat extends Animal, Desc {
  tail: number;
}

const cat: Cat = {
  tail: 1,
  name: 'cat',
  leg: 4,
  character: 'shy'
}
```

### 混合类型
`JavaScript`可能会有一个方法, 同时这个方法又拥有自己的一些属性, 即对象可同时作为对象或函数使用.
```typescript
/** 计时器 */
interface Timer {
  (time: number, fn: () => any): number;
  /** 定时器索引 */
  index: number;
  clear(): void;
}

function setTimer (): Timer {
  const timer = <Timer>function (time: number, fn: () => any) {
    const index = setInterval(() => {
      fn()
    }, time)
    timer.index = index
    return index
  }
  timer.clear = () => clearInterval(timer.index)
  return timer
}

const timer = setTimer()
timer(1000, () => console.log(11))

setTimeout(() => {
  timer.clear()
}, 5000)
```

## 函数
```typescript
/** 可直接在方法中进行变量声明 */
function say (name: string): string {
  return name
}
console.log(say('LiLei'))
```

### 函数重载
`TypeScript`可以让函数根据传入的值类型, 返回不同的类型, 这个叫做**函数重载**
```typescript
function say (type: string): number;
function say (type: number): string;

function say (type): any {
  if (typeof type === 'string') {
    return type.length
  }
  if (typeof type === 'number') {
    return type + ''
  }
}
console.log(say(4))
console.log(say('lilei'))
```

## 泛型
泛型, 参数化类型, 允许将数据类型作为参数使用. 可以在编写代码时, 使用一些以后才指定的类型.

### 泛型函数
比如说一个方法, 可将传入的值返回出来.

```typescript
function identity<T>(arg: T): T {
  return arg
}
const liLei = identity(LiLei)
```

使用了泛型后, 编译器会直接利用类型推论, 确定方法返回的类型

![泛型](https://ws1.sinaimg.cn/large/006tKfTcly1g0jv85gio5j30ho05eq3j.jpg)

### 泛型接口
```typescript
interface BaseResponse<T> {
  code: number;
  msg: string;
  data: T;
}

type UsersResponse = BaseResponse<User[]>

const usersResponse = <UsersResponse>{}
usersResponse.data.forEach(user => {
})
```

![泛型接口](https://ws3.sinaimg.cn/large/006tKfTcly1g0jw42h0szj315y0ly77j.jpg)

### 泛型类
与泛型接口差不多

### 泛型约束
可以对泛型进行约束, 可通过`extends`关键字对泛型进行约束.

```typescript
interface LikeArray {
  length: number;
}
/** 函数里的参数必须要有 LikeArray接口的length属性 */
function getLength<T extends LikeArray> (arr: T): number {
  return arr.length;
}
/** 不会报错 */
getLength({
  length: 1,
  nickname: ''
})
```

```typescript
interface User {
  /** 
  * 对参数进行约束, 同时对另外的参数进行推断
  * type keys = 'nickname' | 'age' 也能实现类似功能.
  */
  set?<T extends keyof User> (key: T, value: User[T]): void;
}
const LiLei: User = {
  ...,
  set (key, value) {
    this[key] = value
  }
}
LiLei.set(...)
```

* 约束后参数提示
![约束后参数提示](https://ws2.sinaimg.cn/large/006tKfTcly1g0jxb685okj30fo0b2myb.jpg)
* 类型推断
![类型推断](https://ws4.sinaimg.cn/large/006tKfTcly1g0jxbeo5kyj30t403u74p.jpg)
![类型推断](https://ws1.sinaimg.cn/large/006tKfTcly1g0jxbvk8n9j30ou03uq3e.jpg)
* 推断错误提示
![推断错误提示](https://ws2.sinaimg.cn/large/006tKfTcly1g0jxceds54j30yu06o3zx.jpg)

## 交叉类型 & 联合类型
* 交叉类型: 将多个类型合并成一个类型, 必须要实现多个类型的全部特性
* 联合类型: 联合类型表示多个类型的**或**关系, 只需实现其中一个的全部特性即可
```typescript
interface Animal {
  name: string;
}
interface Run {
  leg: number;
  run(): void;
}
interface Fly {
  wing: number;
  fly(): void;
}
/** 交叉类型 */
type Dog = Animal & Run;
type Bird = Animal & Fly;
/** 必须要实现全部的特性, 不然会报错 */
const dog: Dog = {
  name: 'dog',
  leg: 4,
  run () {}
}
/** 联合类型 */
type RandomAnimal = Dog | Bird;

const randomAnimal: RandomAnimal = {
  name: '',
  leg: 4,
  run () {}
}
```

交叉类型也会判断是否实现全部特性  
![提醒是否实现全部特性](https://ws4.sinaimg.cn/large/006tKfTcly1g0n5fxd8nej30h306pt9r.jpg)  

联合类型会根据已有的值来提示还未实现的特性  
![提醒类型Dog还未实现run方法](https://ws1.sinaimg.cn/large/006tKfTcly1g0n5tqq2q0j30fw07jwfq.jpg)  
![提醒类型Bird还未实现fly方法](https://ws2.sinaimg.cn/large/006tKfTcly1g0n5zci941j30g707ijso.jpg)  

## 可辨识联合
可辨识联合, 可以自动辨别联合, 可以提供更好的提示, 极大的提高开发体验.
```typescript
interface Square {
  kind: 'square';
  size: number;
}
interface Rectangle {
  kind: 'rectangle';
  width: number;
  height: number;
}
interface Circle {
  kind: 'circle';
  radius: number;
}
/** 联合类型 */
type Shape = Square | Rectangle | Circle;
function area(s: Shape) {
  switch (s.kind) {
    case 'square': return s.size * s.size;
    case 'rectangle': return s.height * s.width;
    case 'circle': return Math.PI * s.radius ** 2;
  }
}
```

![可辨识联合](https://ws1.sinaimg.cn/large/006tKfTcly1g0nbqmk6mij30hl04taak.jpg)
