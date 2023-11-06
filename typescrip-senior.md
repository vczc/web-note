# typescript高级用法

## 参考文献
- [常用Ts高级类型](https://juejin.cn/post/6844904066489778183)
- [其他Ts高级类型](https://juejin.cn/post/6844904068096196621)

## Record
以 typeof 格式快速创建一个类型，此类型包含一组指定的属性且都是必填。
```typescript
type Coord = Record<'x' | 'y', number>;

// 等同于
type Coord = {
	x: number;
	y: number;
}
```

## Partial
将类型定义的所有属性都修改为可选。
```typescript
type Coord = Partial<Record<'x' | 'y', number>>;

// 等同于
type Coord = {
	x?: number;
	y?: number;
}
```

## Readonly
不管是从字面意思，还是定义上都很好理解：将所有属性定义为只读。
```typescript
type Coord = Readonly<Record<'x' | 'y', number>>;

// 等同于
type Coord = {
    readonly x: number;
    readonly y: number;
}

// 如果进行了修改，则会报错：
const c: Coord = { x: 1, y: 1 };
c.x = 2; // Error: Cannot assign to 'x' because it is a read-only property.
```

## Pick
从类型定义的属性中，选取指定一组属性，返回一个新的类型定义。
```typescript
type Coord = Record<'x' | 'y', number>;
type CoordX = Pick<Coord, 'x'>;

// 等用于
type CoordX = {
	x: number;
}
```

## Required
将类型属性都变成必填。
```typescript
type Coord = Required<{ x: number, y?:number }>;

// 等同于
type Coord = {
	x: number;
	y: number;
}
```

## Exclude
排除一个 联合类型 中指定的子类型
```typescript
type T0 = Exclude<'a' | 'b' | 'c', 'b'> // 'a' | 'c'
type T1 = Exclude<string | number | boolean, boolean> // string | number
```

## Extract
与 Exclude<T, U> 完全相反的功能，用于提取指定的 联合类型
```typescript
type T0 = Extract<'a' | 'b' | 'c', 'a'> // 'a'
type T1 = Extract<string | number | boolean, boolean> // boolean
```

## Omit
排除接口中指定的属性
```typescript
interface I1 {
	a: number;
	b: string;
	c: boolean;
}

type AC = Omit<I1, 'b'>;     // { a:number; c:boolean } 
type C = Omit<I1, 'a' |'b'>  // { c: boolean }
```

## NonNullable
过滤掉 联合类型 中的 null 和 undefined 类型
```typescript
type T1 = NonNullable<string | null | undefined>; // string
```

## Parameters
获取函数的全部参数类型，以 元组类型 返回
```typescript
type F1 = (a: string, b: number) => void;

type F1ParamTypes = Parameters(F1);  // [string, number]
```

## ConstructorParameters
获取的是 构造函数 的全部参数。关于构造函数声明，以及如何使用此 高级类型 的方式
```typescript
interface IEntity {
    count?: () => number
}

interface IEntityConstructor {
    new (a: boolean, b: string): IEntity;
}

class Entity implements IEntity {
    constructor(a: boolean, b: string) { }
}

type EntityConstructorParamType = ConstructorParameters<IEntityConstructor>; // [boolean, string]
```

## ReturnType
接收函数声明，返回函数的返回值类型，如果多个类型则以 联合类型 方式返回
```typescript
type F1 = () => Date;

type F1ReturnType = ReturnType<F1>; // Date
```

## InstanceType
获取 构造函数 的返回类型，如果是多个就以 联合类型 的方式返回
```typescript
type EntityType = InstanceType<IEntityConstructor>; // IEntity
```

## ThisParameterType
获取函数中 this 的数据类型，如果没有则返回 unknown 类型
```typescript
interface Foo {
    x: number
};

function fn(this: Foo) {}

type Test = ThisParameterType<typeof fn>; // Foo
```

## OmitThisParameter
移除函数中的 this 数据类型
```typescript
interface Foo {
    x: number
};

type Fn = (this: Foo) => void

type NonReturnFn = OmitThisParameter<Fn>; // () => void
```