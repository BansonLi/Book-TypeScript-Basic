# 泛型 Generic type
---
## 1. 概念
泛型的本质是参数化类型，通俗的将就是所操作的数据类型被指定为一个参数，这种参数类型可以用在类、接口和方法的创建中，分别成为泛型类，泛型接口、泛型方法。 
**TypeScript**中的泛型跟java中的泛型基本类似。

## 2. 基本语法

### 2.1 基础泛型
使用泛型的好处不仅能够检查类型，保证类型正确，而且能够提高代码的服用性。指定泛型类型一般用来表示，当然，T可以更改为其他值。
```
/*假如我们有个方法  需要返回传进去的参数 */

// function returnVal(x: number): number {
//     return x;
// }
// returnVal(1);//不使用泛型的话这里只能传进去number类型

/*这里使用any虽然可以传进去任何类型的值 但是无法保证返回值就是传进去的参数值*/
// function returnVal(x: any): any {
//     return "" + x;
// }
//
// returnVal(1);
/*这里使用泛型  不仅可以传任何类型的值  也能保证返回值类型的正确性  而且代码可以复用*/
function returnVal<T>(x: T): T {
    return x;
}
returnVal<number>(1);
returnVal("1");
returnVal(true);
```

### 2.2 泛型数组
需要注意的是，在java中由于JVM泛型的擦除机制，在运行时JVM是不知道泛型信息的。所以为了防止出现类型转换异常，Java是 不支持泛型数组的。  
而在TypeScript中支持泛型数组。当我们将参数类型指定为泛型数组，那么，也就代表着这个参数具有数组的所有属性和方法。
```
/*这里使用泛型  不仅可以传任何类型的值  也能保证返回值类型的正确性  而且代码可以复用*/
function returnVal<T>(x: T): T {
    //console.log(x.length);//这里的参数可能是number或boolean，所以没有.length属性
    return x;
}
returnVal<number>(1);
returnVal("1");
returnVal(true);

function Arr<T>(arg: T[]): T[] {
    console.log(arg.length);//这里的泛型数组，那么参数就会有数组所具有的属性和方法
    arg.join("333");
    return arg;
}
let a:Array<string>=["a","b"];
Arr(a);
Arr<string>(["1","2","a"]);
```

### 2.3 泛型方法
```
function A<T>(arg: T): T {
    return arg;
}
// let funA:(a:string)=>string=A;
let funA:(a:number)=>number=A;
// funA("1");
funA(1);

/*我们还可以使用带有调用签名的对象字面量来定义泛型函数：*/
let funA1:{<T>(arg:T):T}=A;
```

### 2.4 泛型接口
泛型接口的存在是为了更好规范编程者的实现过程。
```
interface GenericIdentityFn {
    <T>(arg: T): T;
}

function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: GenericIdentityFn = identity;
```
我们可能想把泛型参数当作整个接口的一个参数。 这样我们就能清楚的知道使用的具体是哪个泛型类型。
```
interface GenericIdentityFn<T> {
    (arg: T): T;
}

function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: GenericIdentityFn<number> = identity;
```

### 2.5 泛型类
可以自定义泛型类，起到通用性的目的。
```
class MyClass<T>{
    x:T;
    y:T;
    show(x:T):T{

        return x;
    }
}

let mcls=new MyClass<number>();
let mcls1=new MyClass<string>();
mcls.show(1);
mcls1.show("Banson")
```

### 2.6 泛型约束 
我们可以通过extends给泛型添加约束，添加约束的泛型传参的时候必须要符合约束的定义.
```
interface lenWise {
    length: number;
}
// function loggingIdentity<T>(arg: T): T {
//     console.log(arg.length);  // 错误，不能调用length属性
//     return arg;
// }
function loggingIdentity<T extends lenWise>(arg: T): T {
    console.log(arg.length);  // Error: T doesn't have .length
    return arg;
}

// loggingIdentity(3);//错误，添加了泛型约束，传进去的值必须要符合接口定义
loggingIdentity({length:1,name:"Banson"});
```

### 2.7 泛型约束泛型 
所谓泛型约束泛型，就是我们通过extends关键字来实现一个类型被另一个类型锁约束。  
比如，现在我们有两个对象，并把一个对象的属性拷贝到另一个对象。 我们要确保没有不小心地把额外的属性从源对象拷贝到目标对象，因此我们需要在这两个类型之间使用约束。
```
function copyFields<T extends U, U>(target: T, source: U): T {
    for (let id in source) {
        target[id] = source[id];
    }
    return target;
}

let x = { a: 1, b: 2, c: 3, d: 4 };

copyFields(x, { b: 10, d: 20 }); // okay
copyFields(x, { Q: 90 });  // error: property 'Q' isn't declared in 'x'.
```

### 2.8 在泛型中使用类类型
```
function create<T>(c: {new(): T; }): T {
    return new c();
}
```

## 3. 总结
总体来说，TypeScript中泛型的使用方法跟java类似，只需要把平时常用的记住就行了。其他特殊的感觉暂时没有太大意义。