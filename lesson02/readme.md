#  基本类型 Data Types 

## 1. 语法结构
**TypeScript** 可以给变量指定类型。指定类型后只能给该变量赋指定类型的值，如果不给初始值的话默认是`undefined`.  
格式： 变量声明 变量名：类型=初始值;  
```
var isDone:boolean=false;  
```
在**TypeScript**中，如果不指定类型直接给初始值的话,编译器会认为你给的初始值的类型就是这个变量的类型。

## 2. 类型

### 2.1 boolean类型
```
let isDone: boolean = false;
```
### 2.2 number类型 
和JavaScript一样，TypeScript里的所有数字都是浮点数。 这些浮点数的类型是 number。 除了支持十进 制和十六进制字面量，Typescript还支持ECMAScript 2015中引入的二进制和八进制字面量。 
```
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;
```

### 2.3 String类型
String类型有两个新特性，用起来很方便 :
 - **多行文本**  
      用键盘1左边的反引号括起来后，直接编写要显示的内容，编译成js会自动加上双引号和加号。比如在平时开发中我们拼接html代码，这个时候用多行文本就很方便了省去了编写加号和双引号，可读性也提高了。非常好用！
      ```
      var str1 = `<div><h1>Hello TypeScript</h1><span>xxx</span></div>`;
      ```
      编译成js代码后
      ```
      var str1 = "<div><h1>Hello TypeScript</h1><span>xxx</span></div>";
      ```
 - **字符串模版**  
      字符串模版就是在多行文本里插入表达式或调用方法。表达式的形式是 `${ expr }`.  
      这是TypeScript代码
      ```
      var name: string = "yzq";
      function getAge(){
          return 23;
      }
      var info: string = `hello my name is ${name},
      I'm ${getAge()} years old`;
      ```
      编译成js代码后：
      ```
      var name = "yzq";
      function getAge() {
          return 23;
      }
      var info = "hello my name is " + name + ",\nI'm " + getAge() + " years old";
      ```
  - **自动拆分字符串**  
      自动拆分字符串是指当你用字符串模版去调用一个方法的时候，字符串模版中的表达式的值会自动赋给被调用方法的参数。
      ```
      var name: string = "yzq";
      function getAge(){
          return 23;
      }
      function showInfo(template,name,age){
          console.log(template);
          console.log(name);
          console.log(age);
      }
      showInfo`my name is ${name},I'm ${getAge()} years old.`;//调用方式为方法命后面跟上反引号，将值传入。
      ```
      编译成js代码后：
      ```
      var name = "Banson";
      function getAge() {
          return 33;
      }
      function showInfo(template, name, age) {
          console.log(template);
          console.log(name);
          console.log(age);
      }
      (_a = ["my name is ", ",I'm ", " years old."], _a.raw = ["my name is ", ",I'm ", " years old."], showInfo(_a, name, getAge())); //调用方式为方法命后面跟上反引号，将值传入。
      var _a;
      ```
      打印结果，可以看到，当我们调用这个方法的时候，会把传进去参数根据表达式的数量自动拆分了。 第一个`template`的值就是被拆分的字符串模版的一个数组，后面的参数值就是相应表达式的值。 

### 2.4 Array类型
**TypeScript** 像JavaScript一样可以操作数组元素。 有两种方式可以定义数组。  
- 第一种，可以在元素类型后面接上 []，表示由此类型元素组成的一个数组.
  ```
  var list:number[]=[1,2,3];
  ```
- 第二种方式是使用数组泛型，Array<元素类型>：
  ```
  var list1:Array<String>=["1","2","3"];
  ```

### 2.5 Enum(枚举)类型
一般在数据个数固定且值固定的情况下使用枚举。例如一个星期有七天，周一到周日。
```
enum Color{red,green,blue};
var c:Color=Color.blue;
var c1:Color=Color[0];
console.log(c);//这里打印的是下标
console.log(c1);//这里打印的是值
```
可以手动指定下标
```
enum Color{red=2,green=4,blue};
var c:Color=Color.blue;
var c1:Color=Color[2];
console.log(c);//这里打印的是下标  5
console.log(c1);//这里打印的是值   red
```

### 2.6 any（任意值） 类型
有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 any类型来标记这些变量。   
如果变量被指定位any类型，则可以任意赋值。 
```
var anyStr:any=123;
anyStr="字符串";
anyStr=false;
var list:Array<any>=[1,"2",false];
```

### 2.7 void 类型
**void** 类型像是与 **any** 类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 `void`
```
function tell():string{//指定返回值类型为string
    return "1";
}
function tell1():void{//无返回值

}
```

## 3. 常见问题