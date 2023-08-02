# TypeScript

1. 类型注解
> let age: number = 18
* 代码中的: number就是类型注解
* 作用：为变量添加类型约束

## 1. TS常用类型

1. JS已有类型
* 原始类型：number/string/boolean/null/undefined/symbol
* 对象类型：object(包括，数组、对象、函数等类型)
2. TS新增类型
* 联合类型、自定义类型、接口、元组、字面量类型、枚举、void、any等

### 1.1 联合类型

1. 数组类型
> let numbers: number[] = [1, 3, 5]

2. 数组中既有number类型，又有string类型的  ``| 叫做联合类型``
> let arr: (number | string)[] = [1, 'a', 3, 'b']

### 1.2 类型别名(自定义类型)
```ts
type CustomArray = (number | string)[]

let arr1: CustomArray = [1, 'a', 2, 'b']
let arr2: CustomArray = ['x', 'y', 6, 7]
```

### 1.3 函数类型

1. 单独指定参数、返回值的类型
```ts
function add(num1: number, num2: number): number{
    return num1 + num2
}
```
```ts
const add = (num1: number, num2: number): number => {
    return num1 +num2
}
```
2. 同时指定参数、返回值的类型
```ts
const add: (num1: number, num2: number) => number = (num1, num2) => {
    return num1 + num2
}
```
3. void类型
```ts
function greet(name: string): void {
    console.log('Hello', name)
}
```
4. 可选参数
```ts
function mySlice(start?: number, end?: number): void {
    console.log('起始索引：', start, '结束索引：', end)
}
```
> 可选参数只能出现在参数列表的最后，也就是说可选参数后面不能再出现必选参数

### 1.4 对象类型
js中的对象是由属性和方法构成的，而TS中对象的类型就是在描述对象的结构
```ts
let person: {name: string, age: number, sayHi(): void, greet(name: string): void } = {
    name: 'jack',
    age: 19,
    sayHi() {},
    greet(name) {}
}
```
```ts
let person: {
    name: string
    age: number
    sayHi: () => void
    greet(name: string): void 
} = {
    name: 'jack',
    age: 19,
    sayHi() {},
    greet(name) {}
}
```
1. 对象可选属性
```ts
function myAxios(config: { url: string, method?: string }) {
    console.log(config)
}
```

### 1.5 接口
当一个对象类型被多次使用时，一般会使用接口(Interface)来进行对象的类型，达到复用的目的
```ts
interface IPerson {
    name: string
    age: number
    sayHi: () => void
}

let Person: IPerson = {
    name: 'jack',
    age: 19,
    sayHi() {}
}
```

1. 接口继承
```ts
interface Point2D {x: number, y: number}
interface Point3D extends Point2D {z: number}
```

### 1.6 元组
元组类型是另一种类型的数组，它确切地知道包含多少个元素，以及特定索引对应的类型。
> let position: [number, number] = [39.5427, 116.3217]

### 1.7 类型推论

### 1.8 类型断言
> const aLink = document.getElementById('link') as HTMLAnchorElement

### 1.9 字面量类型
```ts
let str1 = 'Hello Ts'

const str2 = 'Hello Ts'
```
1. 字面量类型配合联合类型一起使用，用来表示一组明确的可选值列表
```ts
function changeDirection(direction: 'up' | 'down' | 'left' | 'right') {
    console.log(direction)
}
```
### 1.10 枚举类型

功能类似于字面量类型+联合类型组合的功能，也可以表示一组明确的可选值
> 枚举成员是有值的，默认为从0开始自增的数值
```ts
enum Direction { Up, Down, Left, Right }

// 数字枚举
// enum Direction { Up = 2, Down = 4, Left = 8, Right = 16}

// 字符串枚举
// enum Direction { Up = 'Up', Down = 'Down', Left = 'Left', Right = 'Right'}

function changeDirection(direction: Direction) {
    console.log(direction)
}

changeDirection(Direction.Up)
```

### 1.11 any类型

### 1.12 typeof

## 2. TS高级类型

### 2.1 class类
> TS中的class, 不仅提供了class的语法功能，也作为一种类型存在

```ts
class Person {
    age: number
    gender = '男'

    // 构造函数
    constructor(age: number, gender: string) {
        this.age = age
        this.gender = gender
    }
    
    // 实现方法
    scale(n: number): void {
        this.age = 18
        this.gender = '女'
    }
}

const p = new Person()
```

1. 类继承
* extends
```ts
class Animal {
    move() { console.log('Moving along!') }
}

class Dog extends Animal {
    bark() { console.log("汪") }
}

const dog = new Dog()
```
* implements(实现接口)
```ts
interface Singable {
    sing(): void
}

class Person implements Singable{
    sing() {
    }
}
```

2. 类成员可见性
* public
* protected
* private

3. 只读修饰符readonly
> 只能修饰属性不能修饰方法
>
> 接口或者{}表示的对象类型，也可以使用readonly
```ts
class Person {
    readonly age = 18

    constructor(age: number) {
        this.age = age
    }
}
```

### 2.2 类型兼容性
1. Structural Type System(结构化类型系统)
```ts
class Point { x: number; y: number }
class Point2D { x: number; y: number }

const p: Point = new Point2D()
```
2. Nominal Type System(标明类型系统)

3. 对象之间的类型兼容性
> 对于对象来说，y的成员至少与x相同，则x兼容y(成员多的可以赋值给少的)
```ts
class Point { x: number; y: number }
class Point3D { x: number; y: number, z: number }

const p: Point = new Point3D()
```
4. 接口之间的类型兼容性
> 接口之间的类型兼容性, 类似于class
```ts
interface Point { x: number; y: number }
interface Point2D { x: number; y: number }

let p1: Point
let p2: Point2D = p1

interface Point3D { x: number; y: number, z: number }
let p3: Point3D
p2 = p3

// let p3: Point2D = new Point3D()
```
5. 函数之间的类型兼容性
* 参数个数  ``参数多的兼容参数少的``(或者说，参数少的可以赋值给多的)
```ts
type F1 = (a: number) => void
type F2 = (a: number, b:number) => void

let f1: F1
let f2: F2 = f1
```
* 参数类型  ``相同位置的参数类型要相同(原始类型)或兼容(对象类型)``
```ts
type F1 = (a: number) => string
type F2 = (a: number) => string

let f1: F1
let f2: F2 = f1
```
```ts
interface Point2D { x: number; y: number }
interface Point3D { x: number; y: number, z: number }

type F2 = (p: Point2D) => void
type F3 = (p: Point3D) => void

let f2: F2
let f3: F3 = f2
// 错误： f2 = f3
```
* 返回值  ``只关注返回值类型本身即可``
```ts
type F5 = () => string
type F6 = () => string

let f5: F5
let f6: F6 = f5
```
```ts
type F7 = () => { name: string }
type F6 = () => { name: string, age: number }

let f7: F7
let f8: F8

f7 = f8
```


### 2.3 交叉类型(&)

功能类似于接口继承，用于组合多个类型为一个类型(常用于对象型)
> 交叉类型与接口继承的不同：两种实现类型组合时，对于同名属性之间。处理类型冲突的方式不同
```ts
interface Person { name: string }
interface Contact { phone: string }

type PersonDetail = Person & Contact

let obj: PersonDetail = {
    name: 'jack',
    phone: '123...'
}
```

### 2.4 泛型

泛型是可以在保证类型安全前提下，让函数与多种类型一起工作，从而实现复用(参数和返回值类型相同)

``function id<Type>(value: Type): Type { return value }``

1. 简化泛型函数调用 ``在调用泛型函数时，可以省略<类型>来简化泛型函数的调用``
> TS内部会采用一种叫做类型参数推断的机制，来根据传入的实参自动推断出类型变量Type的类型

2. 泛型约束
> 默认情况下，泛型函数的类型变量Type可以代表多个类型，这导致无法访问任何属性。此时就需要为泛型添加约束来收缩类型(缩窄类型取值范围)
* 指定更加具体的类型
```ts
function id<Type>(value: Type[]): Type[] {
    console.log(value.length)
    return value
}
```
* 添加约束
```ts
// 该约束表示：传入的类型必须有length属性
interface ILength { length: number}

function id<Type extends ILength>(value: Type): Type {
    console.log(value.length)
    return value
}
```
* 多个泛型变量之间还可以约束
```ts
// keyof关键字接收一个对象类型，生成其键名称(可能是字符串或数字)的联合类型
function getProp<Type, Key extends keyof Type>(obj: Type, key: Key) {
    return obj[key]
}

let person = { name: 'jack', age: 18 }
getProp(person, 'name')
```
3. 泛型接口
```ts
interface IdFun<Type> {
    id: (value: Type) => Type
    ids: () => Type[]
}

let obj: IdFun<number> = {
    id(value) { return value},
    ids() { return [1, 3, 5]}
}
```
4. 泛型类
```ts
class GenericNumber<NumType> {
    defaultValue: NumType
    add: (x: NumType, y: NumType) => NumType
}

const myNum = new GenericNumber<number>()
myNum.defaultValue = 10
```
5. 泛型工具类
* Partial<Type>  ``用来构造一个类型，将Type的所有属性设置为可选``
```ts
interface Props {
    id: string
    children: number[]
}

type PartialProps = Partial<Props>
```
* Readonly<Type>  ``用来构建一个类型，将Type的所有属性都设置为readonly``
```ts
interface Props {
    id: string
    children: number[]
}

type ReadonlyProps = Readonly<Props>
```
* Pick<Type, Keys>  ``从Type中选择一组属性来构造新类型``
```ts
interface Props {
    id: string
    title: string
    children: number[]
}

type PickProps = Pick<Props, 'id' | 'title'>
```
* Record<Keys, Type>  ``构造一个对象类型，属性键为Keys，属性类型为Type``
```ts
type RecordObj = Record<'a' | 'b' | 'c', string[]>
let obj: RecordObj = {
    a: ['1'],
    b: ['2'],
    c: ['3']
}
```

### 2.5 索引签名类型

当无法确定对象中有哪些属性(或者说对象中可以出现任意多个属性)，此时，就用到索引签名类型了
> 在JS中数组是一类特殊的对象，特殊在数组的键(索引)是数值类型
```ts
// 使用[key: string]来约束该接口中允许出现的属性名称。表示只要是string类型的属性名称，都可以出现在对象中
interface AnyObject {
    [key: string]: number
}

let obj: AnyObject = {
    a: 1,
    b: 2,
}
```

### 2.6 映射类型

基于旧类型创建新类型(对象类型)，减少重复、提升开发效率
```ts
type PropKeys = 'x' | 'y' | 'z'
// type Type1 = {x: number, y: number, z: number}
// Key in PropKeys表示Key可以是PropKeys联合类型中的任意一个，类似于forin(let k in obj)
type Type2 = { [Key in PropKeys] : number}
```
1. keyof关键字
```ts
type Props = { a: number, b: string, c: boolean }
type Type3 = { [key in keyof Props]: number}
```

### 2.7 索引查询类型

T[P]：索引查询类型，用来查询属性的类型
```ts
// Partial<Type>的实现
type Partial<T> = {
    [P in keyof T]?: T[P]
}
```
```ts
type Props = { a: number, b: string, c: boolean }

type TypeA = Props['a']
type TypeB1 = Props['a' | 'b']
type TypeB2 = Props[keyof Props]
```



## 3. TypeScript类型声明文件

类型声明文件：用来为已存在的JS库提供类型信息

### 3.1 TS中的两种文件类型

1. .ts文件(implementation, 代码实现文件)
* 既包含类型信息又可以执行代码
* 可以被编译成.js文件，然后执行代码
* 用途：编写程序代码的地方
2. .d.ts文件(declaration, 类型声明文件)
* 只包含类型信息的类型声明文件
* 不会生成.js文件，仅用于提供类型信息
* 用途：为JS提供类型信息

### 3.2 类型声明文件的使用说明

1. 使用已有的类型声明文件
* 内置类型的声明文件
* 第三方库的类型声明文件
    * 库自带类型声明文件
    * 由Definitely Typed提供
2. 创建自己的类型声明文件
* 项目内共享类型
> 如果多个.ts文件中都用到同一个类型，此时可以创建.d.ts文件提供该类型，实现类型共享
```
操作步骤：
1. 创建index.d.ts类型声明文件
2. 创建需要共享的数据类型，并使用export导出(TS中的类型也可以使用import/export实现模块化功能)
3. 在需要使用共享类型的.ts文件中，通过import导入即可
```
* 为已有JS文件提供类型声明
    * 在将JS项目迁移到TS项目时，为了让已有的.js文件由类型声明
    * 成为库作者，创建库给其他人使用
> 在导入.js文件时，TS会自动加载与.js同名的.d.ts文件，以提供类型声明
> 
> declare关键字：用于类型声明，为其他地方已存在的变量声明类型，而不是创建一个新的变量


## 4. 在React中使用TypeScript

### 4.1 使用CRA创建支持TS的项目

React脚手架工具create-react-app默认支持TypeScript
> npx create-react-app 项目名称 --template typescript

1. tsconfig.json配置文件：指定文件和项目编译所需的配置项
>  tsc --init
```json
{
  // 编译选项
  "compilerOptions": {
    // 生成代码的语言版本
    "target": "es5",
    // 指定要包含在编译中的 library
    "lib": ["dom", "dom.iterable", "esnext"],
    // 允许 ts 编译器编译 js 文件
    "allowJs": true,
    // 跳过声明文件的类型检查
    "skipLibCheck": true,
    // es 模块 互操作，屏蔽 ESModule 和 CommonJS 之间的差异
    "esModuleInterop": true,
    // 允许通过 import x from 'y' 即使模块没有显式指定 default 导出
    "allowSyntheticDefaultImports": true,
    // 开启严格模式
    "strict": true,
    // 对文件名称强制区分大小写
    "forceConsistentCasingInFileNames": true,
    // 为 switch 语句启用错误报告
    "noFallthroughCasesInSwitch": true,
    // 生成代码的模块化标准
    "module": "esnext",
    // 模块解析（查找）策略
    "moduleResolution": "node",
    // 允许导入扩展名为.json的模块
    "resolveJsonModule": true,
    // 是否将没有 import/export 的文件视为旧（全局而非模块化）脚本文件。
    "isolatedModules": true,
    // 编译时不生成任何文件（只进行类型检查）
    "noEmit": true,
    // 指定将 JSX 编译成什么形式
    "jsx": "react-jsx"
  },
  // 指定允许 ts 处理的目录
  "include": ["src"]
  
}
```
2. React组件的文件拓展名变为*.tsx
3. react-app-env.d.ts: React项目默认的类型声明文件
> 三斜线指令：指定依赖的其他类型声明文件，types表示依赖的类型声明文件包的名称
```ts
/// <reference types="react-scripts" />

// 解释：告诉TS加载react-scripts这个包提供的类型声明
```

### 4.2 React中的常用类型
> TS项目中，推荐使用TypeScript实现组件类型校验(代替PropTypes)

1. 函数组件
* 组件的类型
* 组件的属性(props)
```ts
type Props = { name: string, age?: number }

const Hello:FC<Props> = ({ name, age }) => (
    <div>你好, 我叫:{name}, 我 {age} 岁了</div>
)

// const Hello = ({ name, age }: Props) => (
//     <div>你好，我叫:{name}, 我 {age} 岁了</div>
// )
const App = () => (
    <div>
        <Hello name = "jack" age = {16} />
    </div>
)
```
* 组件属性的默认值(defaultProps)
```ts
const Hello:FC<Props> = ({ name, age }) => (
    <div>你好, 我叫:{name}, 我 {age} 岁了</div>
)


Hello.defaultProps = {
    age: 18
}

// const Hello = ({ name, age = 18 }: Props) => (
//     <div>你好，我叫:{name}, 我 {age} 岁了</div>
// )
```
* 事件绑定和事件对象
```ts
<button onClick={onClick}>点赞</button>

const onClick = () => {}
const onClick1 = (e: React.MouseEvent<HTMLButtonElement>) => {}

<input onChange={onChange} />
const onChange = (e: React.ChangeEvent<HTMLInputElement>) => {}
```
2. class组件
* 组件的类型、属性、事件
```ts
type State = { count: number }
type Props = { message?: string }

class C1 extends React.Component {}
class C2 extends React.Component<Props> {}
class C3 extends React.Component<{}, State> {}
class C4 extends React.Component<Props, State> {}
```
```ts
type Props = { name: string, age?: number }

class Hello extends React.Component<Props>{
  static defaultProps: Partial<Props> = {
      age: 8
  }
  render() {
      const { name, age } = this.props
      return <div> 你好，我叫:{name}, 我 {age} 岁了 </div>
  }
}
```
* 组件状态(state)
```ts
type State = { count: number }

class Counter extends React.Component<{}, State> {
    state: State = {
        count: 0
    }
    handleClick = () => {
        this.setState({
        count: this.state.count + 1
        })
    }
    render() {
        return (
            <div>
               计数器: {this.state.count}
                <button onClick = {this.handleClick}>+1</button> 
            </div>
        )
    }
}
```
