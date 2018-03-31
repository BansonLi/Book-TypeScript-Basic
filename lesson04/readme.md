# 函数 Functions
---
## 1. 基本语法
概念：函数是一组可以随时随地运行的语句。是 ECMAScript 的核心。和JavaScript一样，TypeScript函数可以创建有名字的函数和匿名函数。  
javascript中的函数：
```
  /*普通函数*/
  function add(x, y) {
      console.log(x + y);
  }
  add(1, 2);
  /*匿名函数*/
  var addFun = function (x, y) {
      console.log(x + y);
  };
  addFun(2, 3);
```
在TypeScript中，我们可以给函数指定类型，这样在编译阶段会规避很多错误.函数类型分为参数类型和返回值类型，如下代码所示，参数类型直接在参数后指定，返回值类型则在该函数后面指定。 TypeScript能够根据返回语句自动推断出返回值类型，因此我们通常省略它。
```
  /*普通函数*/
  function add(x:number,y:number):number{
      return x+y;
  }
  add(1,2);
  /*匿名函数*/
  var addFun=function (x:number,y:number):number {
      return x+y;
  }

  addFun(2,3);
```
## 2. 编写案例
函数有多种写法或者叫多种类型，以下是我们常用的类型。
### 2.1 完整函数类型
在TypeScript中，为了提高代码的可读性，提供给我们另一种写法，这种写法会明确的指出参数的意义。 
如下代码所示，我们很容易就明白这个方法传的参数是数学分数和英语分数，该方法返回的是数学和英语分数的和。返回值类型是函数类型的必要部分，如果函数没有返回任何值，你也必须指定返回值类型为 void而不能留空。
```
  let myAdd:(mathScores:number,englishScores:number)=>number=function(x:number,y:number):number {
    return x+y;
  }
```
### 2.2 推断类型 
如果你在赋值语句的一边指定了类型但是另一边没有类型的话，TypeScript编译器会自动识别出类型
```
 let myAdd:(mathScores:number,englishScores:number)=>number=function (x:number,y:number) {
    return x+y;//这里会推断返回的是number类型的数据
}
```

### 2.3 可选参数类型
TypeScript里的每个函数参数都是必须的。也就是说我们在函数中定义了几个参数在调用的时候就要传几个参数。而在javaScript中，参数是可传可不传的，如果不传，默认是undefined。 在TypeScript里我们可以在参数名后加上 ?实现可选参数的功能。 
如下图代码所示，z是个可选参数，看打印结果可以看到，如果不传，默认就是undefined。需要注意的是，可选参数必须跟在必须参数后面。也就是说，如果x是可选参数，那么，x就要放到y的后面。
```
/*可选参数 可选参数用?表示 调用的时候可传可不传 不传的话默认值是undefined 可选参数必须放到其他参数后面*/
function add1(x:string,y?:string):string{
    console.log(x+y);
    return x+y;
}
add1("888");//888undefined
add1("88","yzq");//yzq
```

### 2.4 默认参数
在TypeScript里，我们也可以为参数提供一个默认值，当用户没有传递这个参数或传递的值是undefined时。 它们叫做有默认初始化值的参数。与可选参数不同的是，默认参数不是一定要放到最后的，但是如果默认参数放到了普通参数的前面，恰好我们不想给默认参数传其它值的话，则必须传入 undefined值来获得默认值。
```
/*默认参数 默认参数和可选参数类似  调用的时候可传可不传 不传的话是默认值  传的话会覆盖值 如果不想给默认参数指定值必须传入undefined*/
function add2(x="hello",y:string,z?:string){
    console.log(`${x}--${y}--${z}`);
}
add2(undefined,"yzq");//hello--yzq--undefined
add2("hi","yzq");//hi--yzq--undefined
add2("hi","yzq","hahaha");//hi--yzq--hahaha
```
### 2.5 剩余参数类型
必要参数，可选参数和默认参数都代表一个参数，有时候我们并不知道到底有多少个参数传进来，这个时候我们就可用剩余参数了。我们可以传多个，也可以什么都不传。
```
/*剩余参数（可变参数）*/
function add3(...name:string[]){
    console.log(name.join(","));
}
add3();//什么都不传返回空
add3("1","2","3");//1,2,3
```
## 3. 注意事项

### 3.1 this关键字和Lambda表达式 
this是Javascript语言的一个关键字。在 Java 等面向对象的语言中，this 关键字的含义是明确且具体的，即指代当前对象。一般在编译期确定下来，或称为编译期绑定。而在 JavaScript 中，this 是动态绑定，或称为运行期绑定的，这就导致 JavaScript 中的 this 关键字有能力具备多重含义。关于this的详解请看：[链接](http://www.cnblogs.com/justany/archive/2012/11/01/the_keyword_this_in_javascript.html)
```
/**
/**
 * Created by yzq on 2017/1/11.
 */
var say = {
    name: "yzq",
    age: 23,
    hello: function () {
        return function () {
            return this.name;
        };
    },
    hi: function () {
        return this.name;

    }
}
var h=say.hello();//这里返回了一个方法
console.log(h()) ;//这里调用的话相当于window.h();获取不到say对象里的name。此时，该函数里的this指向的是全局对象window.
console.log(say.hi());//这里调用的话，hi方法里面的this指向的是say这个对象,因此，会打印.
```

### 3.2 Lambda表达式 
也叫箭头函数，就是用来声明匿名函数，并且能消除传统的javascript中匿名函数this指针问题。
```
/**
 * Created by yzq on 2017/1/11.
 */

/*简单的箭头函数 无参数且无返回值*/
var add = () => {console.log("无参数且无返回值")};

/*有一个参数 且有返回值*/
var add1=x=>`有一个参数的时候可以不写()，${x}
 ass`;

/*多个参数*/
var add2=(x,y,z)=>{
    console.log(x+y+z);
}
add2(1,2,3);
```
编译成js代码后
```
/**
 * Created by yzq on 2017/1/11.
 */
/*简单的箭头函数 无参数且无返回值*/
var add = function () { console.log("无参数且无返回值"); };
/*有一个参数 且有返回值*/
var add1 = function (x) { return ("\u6709\u4E00\u4E2A\u53C2\u6570\u7684\u65F6\u5019\u53EF\u4EE5\u4E0D\u5199()\uFF0C" + x + "\n ass"); };
/*多个参数*/
var add2 = function (x, y, z) {
    console.log(x + y + z);
};
add2(1, 2, 3);
```