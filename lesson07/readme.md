# 接口介绍 Introduction to Interface
---
## 1.语法结构 Grammatical structure
使用`Interface`关键字来定义接口，接口中可以定义属性类型和方法的名称.
```
  interface Person {
    name: string,
    age : number,
    function run():string;
  }
```
## 2. 编写案例
按多种情况来学习接口编程吧。
### 2.1 接口的可选属性
在实际应用中，接口里的属性不全都是必需的。 有些是只在某些条件下存在，或者根本不存在。在这种个情况下可以是用接口的可选属性去定义。 
如下代码所示，`name`和`age`被定义为可选属性，那么在传对象的时候`name`和`age`就可有可无。
```
  interface Person{
      name?:string;
      age?:number
  }

  function getInfo(p:Person){
      console.log(p.name);
      console.log(p.age);
  }
  let student={}
  // let student={name:"yzq"}
  // let student={name:"yzq",age:23}
  getInfo(student);
```
### 2.2 接口的只读属性
如果我们希望对象属性只能在对象刚刚创建的时候修改其值。 我们可以在属性名前用 readonly来指定只读属性。
```
  interface Point{
    readonly x:number;
    readonly y:number;
  }

  function getPoint(p:Point){
      console.log(p.x);
      console.log(p.y);
  }
  let point:Point={x:1,y:2};
  // point.x=2;//错误  这里不能再子修改值
  getPoint(point);
```
### 2.3 定义只读数组 
只读数组也一样，一旦定义后不能再修改数组
```
  let a:ReadonlyArray<number> =[1,2,3,4,5];
  // a[0]=2;//不能再修改该数组
  // a.length=20;
```
### 2.4 接口的函数类型 
接口可以描述javascript的任何对象，不仅能描述对象的属性类型，当然也能描述对象的函数类型.  
 如下代码所示，接口描述了这个函数的参数类型和返回值类型:
```
  interface getStr {
      /*在这里我们描述了这个接口有个函数 这个函数传进去2个number类型的参数  然后返回一个string类型的数据*/
      (x: number, y: number): string;
  }
  let myStr : getStr;
  myStr = function (gradeNum: number, classNum: number) {
      return `${gradeNum}年级${classNum}班`;
  }
  console.log(myStr(2,8));//2年级8班
```

### 2.5 接口的数组类型（可索引的类型） 
跟接口描述函数类型差不多，我们也可以描述那些能够“通过索引得到”的类型，比如通过下标获取数组中的值 a[2];需要注意的是，索引器的类型只能为 `number` 或者 `string`。
```
  interface stringArr{
      /*描述的一个数组 这个数组里面的元素是string类型 并且只能通过number类型来索引  [index:number]是索引器  string是该数组元素类型*/
      [index:number]:string;
      // age:number;//需要注意的是，当我们将这个接口是数组类型时，那么，接口中定义的其它属性的类型都必须是该数组的元素类型。  这里的number类型是报错的
  }
  let strArr:stringArr;
  strArr=["1","2"];
  // strArr.name="3";
  let str:string=strArr[0];
  console.log(str);//打印1
```
### 2.6 接口的类类型 
所谓类类型，就是一个类去实现接口，而不是直接把接口拿来用，这更符合我们的使用习惯。
```
  interface IClock{
      /*定义了一个接口  这个接口中有一个属性和一个方法*/
      currentTime:Date;
      getTime(d:Date);
  }
  /*Time类实现IClock接口*/
  class Time implements IClock{
      currentTime:Date;
      getTime(d:Date){
          this.currentTime=d;
      }
  }
```

### 2.7 扩展接口 
在TypeScript中，接口跟类一样是可以相互继承的， 这让我们能够从一个接口里复制成员到另一个接口里，可以更灵活地将接口分割到可重用的模块里。
```
  /*接口可以继承接口  并且可以多继承*/

  interface shape{
      color:string;
  }
  interface pen extends shape{
      width:number;
  }
  let circle=<pen>{};//注意这里的写法，创建一个对象并指定泛型
  circle.color="red";//这里可以获取color属性
  circle.width=2;//有width属性
```
一个接口可以继承多个接口，创建出多个接口的合成接口。
```
  /*接口可以继承接口  并且可以多继承*/

  interface shape{
      color:string;
  }
  interface pen extends shape{
      width:number;
  }

  interface Circle extends shape,pen{
      point:number[];
  }
  let c=<Circle>{};//注意这里的写法，创建一个对象并指定泛型
  c.point=[1,2];
  c.color="red";
  c.width=1;
```
### 2.8 混合类型 
所谓的混合类型就是在一个接口中定义多种类型，比如属性，函数，数组等。
```
  interface Counter {
      (start: number): string;
      interval: number;
      reset(): void;
  }

  function getCounter(): Counter {
      let counter = <Counter>function (start: number) { };
      counter.interval = 123;
      counter.reset = function () { };
      return counter;
  }

  let c = getCounter();
  c(10);
  c.reset();
  c.interval = 5.0;
```
## 3.注意事项