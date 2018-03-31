# 类 Class
---
传统的JavaScript程序使用函数和基于原型`（prototype）`的继承来创建可重用的组件，但对于熟悉使用面向对象方式的程序员来讲就有些棘手，因为他们用的是基于类的继承并且对象是由类构建出来的。 从**ECMAScript 2015**，也就是**ECMAScript 6**开始，JavaScript程序员将能够使用基于类的面向对象的方式。 但是可能要等到下个版本的javascript发布发布才能使用，在**TypeScript**中我们现在就可以使用这些新特性。
## 1. 语法结构 Grammatical structure
TypeScript中类的概念跟java是差不多的。有个别地方是不一样的，比如构造方法。
```
class Person {
    name: String;//属性
    age: number;//属性
    /*构造方法 与java不同，不是使用类名，而是使用constructor*/
    constructor(name: String, age: number) {
        this.name = name;
        this.age = age;
    }
    /*方法*/
    say() {
        return `Hello，my name is ${this.name},I'm ${this.age} years old`;
    }
}

var person = new Person("Banson", 23);
alert(person.say());
```
上面的代码就是定义了一个Person类。这个类有name和age两个属性，还有一个构造方法和一个say方法，很简单，和java基本一样，除了构造器名字和java的有点区别。需要注意的是，TypeScript好像暂时不能存在多个构造器，没有重载方法。子类可以重写父类方法。 
## 2. 特型
JavaScript 的类也具备各种高级编程语言的特型：
### 2.1 继承
TypeScript的继承和java也类似。
```
class Person {
    name: String;//属性
    age: number;//属性
    /*构造方法 与java不同，不是使用类名，而是使用constructor*/
    constructor(name: String, age: number) {
        this.name = name;
        this.age = age;
    }
    /*方法*/
    say() {
        return `Hello，my name is ${this.name},I'm ${this.age} years old`;
    }

}
// var person = new Person("yzq", 23);
// alert(person.say());

class Student extends Person{
    school:String;
    constructor(){
        super("yzq",23);
        this.school="清华"
    }
    /*这个地方类似 重写 */
    say(){
        return `姓名:${this.name},年龄${this.age},学校${this.school}`
    }
}
var s=new Student();
// s.name="张三";//这里可以重新赋值
// s.age=34;
// s.school="北大";
alert(s.say());
```

### 2.2 访问修饰符 
跟java类似，TypeScript的访问修饰符有三个，分别是`public`、`private`、`protected` 。  
**TypeScript**的默认访问修饰符是`public`。 
* public声明的属性和方法在类的内部和外部均能访问到。 
* protected声明的方法和属性只能在类的内部和其子类能访问。 
* private声明的方法和属性只能在其类的内部访问。
```
class Person {
    private name: String;
    protected age: number;
    constructor(name: String,age: number) {
        this.name = name;
        this.age = age;
    }
    private say(){
        console.log(`${this.name},${this.age}`);
    }
    protected tell(){
        console.log(`${this.name},${this.age}`);
    }
}
var p=new Person("yzq",23);
// p.name="yzq";//错误，无法访问到
// p.age=23;
class Student extends Person{
    /*错误 无法访问*/
    // say(){
    //  }

    tell(){

    }
}
```
### 2.3 readonly修饰符 
使用**ReadOnly**关键字会将属性设置为只读的。 只读属性必须在声明时或构造函数里被初始化。且值不能不能被修改，和const类似。 
最简单判断该用`readonly`还是`const`的方法是看要把它做为变量使用还是做为一个属性。 做为变量使用的话用 `const`，若做为属性则使用`readonly`。
```
class Person { 
    readonly name: string;

    readonly age: number=23;//必须初始化

    constructor(name:string) {
        this.name = name;//必须初始化
   }

}

let p = new Person("Banson");
p.name = "aaa"//错误，不能被修改
p.age=24//错误，不能被修改
```
### 2.4 参数属性
在上面的例子中，我们都是在类中先声明一个属性，然后再赋值。这跟java比较像。在**TypeScript**中，参数属性可以方便地让我们在一个地方定义并初始化一个成员。参数属性通过给构造函数参数添加一个访问限定符来声明。
```
class Person {

    /*参数属性  直接给参数添加一个访问修饰符来定义一个属性,注意，必须要添加修饰符*/
    constructor(private name: String, protected age: number) {

    }

    private say() {
        console.log(`${this.name},${this.age}`);
    }

    protected tell() {
        console.log(`${this.name},${this.age}`);
    }
}
```

### 2.5 存取器（Getter And Setter） 
**TypeScript**中的存取器就类似与java中`set`和`get`方法，只不过调用的方式不一样。比如在一个类中我们将其中一个属性用private修饰，那么，在类的外部就无法访问到该属性，这个时候我们可以通过`getters/setters`来封装一下，以便在类的外部去访问该属性。需要注意的是，只带有 get不带有set的存取器自动被推断为`readonly`。也就是说，如果只写get但没有set的话，我们是不能更改值的。
```
/*存取器*/

class Person {
    private _age: number;

    /*相当于java中的getAge()*/
    get age() {
        return this._age;
    }
    /*相当于java中的setAge()*/
    set age(inputAge: number) {
        /*这里可以做一些逻辑处理 */
        if (inputAge < 0 || inputAge > 150) {
            alert("年龄异常");
        } else {
            this._age = inputAge;
        }
    }
    say() {
        console.log(`i'm ${this._age} years old`);
    }
}
let p = new Person();
//p._age=23;//错误，这里访问不到
p.age = 23;//这里相当于调用java中的setAge()方法。
p.say();
```

### 2.6 静态属性 
跟java类似，也是使用`static`关键字修饰。用`static`修饰的属性会在这个类被加载的时候就进行初始化。 
静态属性直接通过类名访问，无需实例化。
```
class Person {
    // private name: String;
    // protected age: number;
    static school:String;

    /*参数属性  直接给参数添加一个访问修饰符来定义一个属性*/
    constructor(private name: String, protected age: number) {
        this.name = name;
        this.age = age;
    }

    private say() {
        console.log(`${this.name},${this.age}`);
    }

    protected tell() {
        console.log(`${this.name},${this.age}`);
    }
}
Person.school;//直接通过类名访问，无需实例化
```

### 2.7 抽象类 
这里跟java抽象类 类似，抽象类做为其它派生类的基类使用。 它们一般不会直接被实例化。 不同于接口，抽象类可以包含成员的实现细节。 abstract关键字是用于定义抽象类和在抽象类内部定义抽象方法。子类必须实现父类的抽象方法。抽象类不能直接实例化。
```
abstract class Person{
    name:string;
    age:number;
    abstract say();
    tell(){
        console.log("hello")
    }
}
class Student extends Person{
    school:string;
    /*必须实现父类抽象方法*/
    say(){
        console.log(`i’m a student`);
    }
    test(){

    }
}
let p:Person;
p=new Student();//子类对象指向父类引用。p无法获取到子类中的属性和方法
p.tell();
let s=new Student();
s.say();
```

## 3. 注意事项