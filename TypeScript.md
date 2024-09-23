# TypeScript 基础类型
|数据类型	|关键字	|描述|
|------|------|--------|
|任意类型	|any	|声明为 any 的变量可以赋予任意类型的值。|
|数字类型	|number|	双精度 64 位浮点值。它可以用来表示整数和分数。|
|字符串类型	|string|	一个字符系列，使用单引号（'）或双引号（"）来表示字符串类型。反引号来定义多行文本和内嵌表达式。|
|布尔类型	|boolean|	表示逻辑值：true 和 false。|
|数组类型	|无	|声明变量为数组。|
|元组	|无	|元组类型用来表示已知元素数量和类型的数组，各元素的类型不必相同，对应位置的类型需要相同。|
|枚举	|enum	|枚举类型用于定义数值集合。|
|void	|void|	用于标识方法返回值的类型，表示该方法没有返回值。|
|null	|null|	表示对象值缺失。|
|undefined	|undefined|	用于初始化变量为一个未定义的值|
|never|	never	|never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值。|

**Note:** TypeScript 和 JavaScript 没有整数类型。

## Any 类型
任意值是 TypeScript 针对编程时类型不明确的变量使用的一种数据类型，它常用于以下三种情况。

1、变量的值会动态改变时，比如来自用户的输入，任意值类型可以让这些变量跳过编译阶段的类型检查，示例代码如下：
```
let x: any = 1;    // 数字类型
x = 'I am who I am';    // 字符串类型
x = false;    // 布尔类型
```
2、改写现有代码时，任意值允许在编译时可选择地包含或移除类型检查，示例代码如下：
```
let x: any = 4;
x.ifItExists();    // 正确，ifItExists方法在运行时可能存在，但这里并不会检查
x.toFixed();    // 正确
```
3、定义存储各种类型数据的数组时，示例代码如下：
```
let arrayList: any[] = [1, false, 'fine'];
arrayList[1] = 100;
```
<hr>

## Null 和 Undefined
### null
在 JavaScript 中 null 表示 "什么都没有"。
null是一个只有一个值的特殊类型。表示一个空对象引用。
用 typeof 检测 null 返回是 object。

### undefined
在 JavaScript 中, undefined 是一个没有设置值的变量。
typeof 一个没有值的变量会返回 undefined。
Null 和 Undefined 是其他任何类型（包括 void）的子类型，可以赋值给其它类型，如数字类型，此时，赋值后的类型会变成 null 或 undefined。而在TypeScript中启用严格的空校验（--strictNullChecks）特性，就可以使得null 和 undefined 只能被赋值给 void 或本身对应的类型。

### never 类型
never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值。这意味着声明为 never 类型的变量只能被 never 类型所赋值，在函数中它通常表现为抛出异常或无法执行到终止点（例如无限循环）。
```
let x: never;
let y: number;

// 编译错误，数字类型不能转为 never 类型
x = 123;
// 运行正确，never 类型可以赋值给 never类型
x = (()=>{ throw new Error('exception')})();
// 运行正确，never 类型可以赋值给 数字类型
y = (()=>{ throw new Error('exception')})();
// 返回值为 never 的函数可以是抛出异常的情况
function error(message: string): never {
    throw new Error(message);
}
// 返回值为 never 的函数可以是无法被执行到的终止点的情况
function loop(): never {
    while (true) {}
}
```

# 类型断言
类型断言可以用来手动指定一个值的类型，即允许变量从一种类型更改为另一种类型。
语法格式：

> <类型>值

或:

> 值 as 类型

## 实例
```
var str = '1' 
var str2:number = <number> <any> str   //str、str2 是 string 类型
console.log(str2)
```
**TypeScript 是怎么确定单个断言是否足够**
当 S 类型是 T 类型的子集，或者 T 类型是 S 类型的子集时，S 能被成功断言成 T。这是为了在进行类型断言时提供额外的安全性，完全毫无根据的断言是危险的，如果你想这么做，你可以使用 any。

它之所以不被称为类型转换，是因为转换通常意味着某种运行时的支持。但是，类型断言纯粹是一个编译时语法，同时，它也是一种为编译器提供关于如何分析代码的方法。

编译后，以上代码会生成如下 JavaScript 代码：
```
var str = '1';
var str2 = str;  //str、str2 是 string 类型
console.log(str2);
```
执行输出结果为：
```
1
```

# 可选参数和默认参数
## 可选参数
在 TypeScript 函数里，如果我们定义了参数，则我们必须传入这些参数，除非将这些参数设置为可选，可选参数使用问号标识 ？。

**实例**
```
TypeScript
function buildName(firstName: string, lastName: string) {
    return firstName + " " + lastName;
}
 
let result1 = buildName("Bob");                  // 错误，缺少参数
let result2 = buildName("Bob", "Adams", "Sr.");  // 错误，参数太多了
let result3 = buildName("Bob", "Adams");         // 正确
```
以下实例，我们将 lastName 设置为可选参数：
```
TypeScript
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}
 
let result1 = buildName("Bob");  // 正确
let result2 = buildName("Bob", "Adams", "Sr.");  // 错误，参数太多了
let result3 = buildName("Bob", "Adams");  // 正确
```
可选参数必须跟在必需参数后面。 如果上例我们想让 firstName 是可选的，lastName 必选，那么就要调整它们的位置，把 firstName 放在后面。
如果都是可选参数就没关系。

## 默认参数
我们也可以设置参数的默认值，这样在调用函数的时候，如果不传入该参数的值，则使用默认参数，语法格式为：
> function function_name(param1[:type],param2[:type] = default_value) { 
}

注意：参数不能同时设置为可选和默认。

**实例**
以下实例函数的参数 rate 设置了默认值为 0.50，调用该函数时如果未传入参数则使用该默认值：
```
TypeScript
function calculate_discount(price:number,rate:number = 0.50) { 
    var discount = price * rate; 
    console.log("计算结果: ",discount); 
} 
calculate_discount(1000) 
calculate_discount(1000,0.30)
```

## 剩余参数
有一种情况，我们不知道要向函数传入多少个参数，这时候我们就可以使用剩余参数来定义。
剩余参数语法允许我们将一个不确定数量的参数作为一个数组传入。
```
TypeScript
function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
}
  
let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
```
函数的最后一个命名参数 restOfName 以 ... 为前缀，它将成为一个由剩余参数组成的数组，索引值从0（包括）到 restOfName.length（不包括）。
```
TypeScript
function addNumbers(...nums:number[]) {  
    var i;   
    var sum:number = 0; 
    
    for(i = 0;i<nums.length;i++) { 
       sum = sum + nums[i]; 
    } 
    console.log("和为：",sum) 
 } 
 addNumbers(1,2,3) 
 addNumbers(10,10,10,10,10)
 ```
 
 
 # 匿名函数
 匿名函数是一个没有函数名的函数。
匿名函数在程序运行时动态声明，除了没有函数名外，其他的与标准函数一样。
我们可以将匿名函数赋值给一个变量，这种表达式就成为函数表达式。
语法格式如下：
```
var res = function( [arguments] ) { ... }
```

## 匿名函数自调用
匿名函数自调用在函数后使用 () 即可：
```
TypeScript
(function () { 
    var x = "Hello!!";   
    console.log(x)     
 })()
 ```
 
 # 元组
 我们知道数组中元素的数据类型都一般是相同的（any[] 类型的数组可以不同），如果存储的元素数据类型不同，则需要使用元组。
元组中允许存储不同类型的元素，元组可以作为参数传递给函数。

创建元组的语法格式如下：
```
var tuple_name = [value1,value2,value3,…value n]
```
## 元组运算
我们可以使用以下两个函数向元组添加新元素或者删除元素：
- push() 向元组添加元素，添加在最后面。
- pop() 从元组中移除元素（最后一个），并返回移除的元素。

# 枚举类型
在 TS 中枚举(enum)是一个比较常用的语法, 通俗来说, 枚举就是对一个对象的所有可能取到的值的集合。
## 普通枚举
如下我们使用 普通枚举 的方式, 声明了一个枚举 Color：
```
// 普通枚举
enum Color { Red, Green, Blue }
```
ts 编译后的结果：
![46a206c30a15a66af6f4807a441462f6.png](../_resources/46a206c30a15a66af6f4807a441462f6.png)

从图中我们会发现 普通枚举 编译后的 JS 代码是会去创建一个对象 Color, 然后往对象上添加一些列的值。这里我们可以在自己项目中测试下, 打印出 Color 看看它是长啥样的：
![6dd1175606c2d2786a9e38be5cb8bb74.png](../_resources/6dd1175606c2d2786a9e38be5cb8bb74.png)
如上图, 可以看出, 最终编译后生成的 Color 对象实际上是一组 key => value 以及 value => key 的一个映射表。
对于普通枚举, 最终编译出来的就是一个普通的对象, 如上代码译出来的 Color 就是一个普通的对象, 之后 Color 是有可能被篡改的(尽管它们是只读的, 但我们依然是有办法修改的)
```
// 普通枚举
enum Color { Red, Green, Blue }

Color.Blue; // 2

// Color 中属性都是只读的, 可通过 Object.defineProperty 进行修改
Object.defineProperty(Color, 'Blue', { writable: true, value: 666 });

Color.Blue; // 666
Color.Blue; // 666
```

## 常量枚举
那么相对于普通枚举(只使用 enum 进行声明), 常量枚举(使用 const + enum 进行声明)在编译结果上又有什么不一样呢?

如下我们使用 常量枚举 的方式, 声明了一个枚举 Color：
```
// 常量枚举(使用 const)
const enum Color { Red, Green, Blue }

const red = Color.Red
const red = Color.Green
```

ts编译后的结果：
![5752b064b68176facbdae9df05ad0616.png](../_resources/5752b064b68176facbdae9df05ad0616.png)
从 TS 编译后的结果会发现, 常量枚举和普通枚举的差异还是很大的, 对于常量枚举会在 TS 编译期间被删除, 常量枚举的成员在使用的地方会被替换为对应的值。

> 1. 普通枚举 enum A {...} 和常量枚举 const enum A {...} 之间的区别主要在于 TS 的编译结果上有所差别
> 2. 普通枚举 enum A {...}, 会将其编译为一个 JS 对象, 对象内就是枚举成员和值的一个相互映射
> 3. 常量枚举 const enum A {...}, 编译后不会生成任何代码, 会删除 TS 部分内容, 对于使用到的成员只会进行值的替换
> 4. 由此可见, 使用 常量枚举 会有更好的性能, 避免额外的性能开销; 那么大部分情况下我们更推荐使用常量枚举。


# 联合类型
联合类型（Union Types）可以通过管道(|)将变量设置多种类型，赋值时可以根据设置的类型来赋值。
**注意：** 只能赋值指定的类型，如果赋值其它类型就会报错。

创建联合类型的语法格式如下：
```
Type1|Type2|Type3
```
## 联合类型数组
我们也可以将数组声明为联合类型：
```
var arr:number[]|string[]; 
var i:number; 
arr = [1,2,4] 
console.log("**数字数组**")  
 
for(i = 0;i<arr.length;i++) { 
   console.log(arr[i]) 
}  
 
arr = ["Runoob","Google","Taobao"] 
console.log("**字符串数组**")  
 
for(i = 0;i<arr.length;i++) { 
   console.log(arr[i]) 
}
```

# TypeScript 接口
接口是一系列抽象方法的声明，是一些方法特征的集合，这些方法都应该是抽象的，需要由具体的类去实现，然后第三方就可以通过这组抽象方法调用，让具体的类执行具体的方法。
TypeScript 接口定义如下：
```
interface interface_name { 
}
```

## 实例
以下实例中，我们定义了一个接口 IPerson，接着定义了一个变量 customer，它的类型是 IPerson。
customer 实现了接口 IPerson 的属性和方法。
```
interface IPerson { 
    firstName:string, 
    lastName:string, 
    sayHi: ()=>string 
} 
 
var customer:IPerson = { 
    firstName:"Tom",
    lastName:"Hanks", 
    sayHi: ():string =>{return "Hi there"} 
} 
console.log("Customer 对象 ") 
console.log(customer.firstName) 
console.log(customer.lastName) 
console.log(customer.sayHi())  
 
var employee:IPerson = { 
    firstName:"Jim",
    lastName:"Blakes", 
    sayHi: ():string =>{return "Hello!!!"} 
} 
console.log("Employee  对象 ") 
console.log(employee.firstName) 
console.log(employee.lastName)
```

## 接口继承
接口继承就是说接口可以通过其他接口来扩展自己。
Typescript 允许接口继承多个接口。
继承使用关键字 extends。
单接口继承语法格式：
```
Child_interface_name extends super_interface_name
```
多接口继承语法格式：
```
Child_interface_name extends super_interface1_name, super_interface2_name,…,super_interfaceN_name
```

### 多继承实例
```
interface IParent1 { 
    v1:number 
} 
interface IParent2 { 
    v2:number 
}
interface Child extends IParent1, IParent2 { } 
var Iobj:Child = { v1:12, v2:23} 
console.log("value 1: "+Iobj.v1+" value 2: "+Iobj.v2)
```


# TypeScript对象
## TypeScript 类型模板
假如我们在 JavaScript 定义了一个对象：
```
var sites = { 
   site1:"Runoob", 
   site2:"Google" 
};
```
这时如果我们想在对象中添加方法，可以做以下修改：
```
sites.sayHello = function(){ return "hello";}
```
如果在 TypeScript 中使用以上方式则会出现编译错误，因为Typescript 中的对象必须是特定类型的实例。
```
TypeScript
var sites = {
    site1: "Runoob",
    site2: "Google",
    sayHello: function () { } // 类型模板
};
sites.sayHello = function () {
    console.log("hello " + sites.site1);
};
sites.sayHello();
```

# TypeScript泛型

泛型（Generics）是一种编程语言特性，允许在定义函数、类、接口等时使用占位符来表示类型，而不是具体的类型。
泛型是一种在编写可重用、灵活且类型安全的代码时非常有用的功能。
使用泛型的主要目的是为了处理不特定类型的数据，使得代码可以适用于多种数据类型而不失去类型检查。

**泛型的优势包括：**

- 代码重用： 可以编写与特定类型无关的通用代码，提高代码的复用性。

- 类型安全： 在编译时进行类型检查，避免在运行时出现类型错误。

- 抽象性： 允许编写更抽象和通用的代码，适应不同的数据类型和数据结构。

## 泛型函数
使用泛型来创建一个可以处理不同类型的函数：

### 实例
```
function identity<T>(arg: T): T {
    return arg;
}

// 使用泛型函数
let result = identity<string>("Hello");
console.log(result); // 输出: Hello

let numberResult = identity<number>(42);
console.log(numberResult); // 输出: 42
```
**解析：** 以上例子中，identity 是一个泛型函数，使用 <T> 表示泛型类型。它接受一个参数 arg 和返回值都是泛型类型 T。在使用时，可以通过尖括号 <> 明确指定泛型类型。第一个调用指定了 string 类型，第二个调用指定了 number 类型。

## 泛型约束
有时候你想限制泛型的类型范围，可以使用泛型约束：

### 实例
```
// 基本语法
interface Lengthwise {
    length: number;
}

function logLength<T extends Lengthwise>(arg: T): void {
    console.log(arg.length);
}

// 正确的使用
logLength("hello"); // 输出: 5

// 错误的使用，因为数字没有 length 属性
logLength(42); // 错误
```
**解析：** 在这个例子中，定义了一个泛型函数 logLength，它接受一个类型为 T 的参数，但有一个约束条件，即 T 必须实现 Lengthwise 接口，该接口要求有 length 属性。因此，可以正确调用 logLength("hello")，但不能调用 logLength(42)，因为数字没有 length 属性。

## 泛型与默认值
可以给泛型设置默认值，使得在不指定类型参数时能够使用默认类型：

**实例**
```
// 基本语法
function defaultValue<T = string>(arg: T): T {
    return arg;
}

// 使用带默认值的泛型函数
let result1 = defaultValue("hello"); // 推断为 string 类型
let result2 = defaultValue(42);      // 推断为 number 类型
```
**说明：** 这个例子展示了带有默认值的泛型函数。函数 defaultValue 接受一个泛型参数 T，并给它设置了默认类型为 string。在使用时，如果没有显式指定类型，会使用默认类型。在例子中，第一个调用中 result1 推断为 string 类型，第二个调用中 result2 推断为 number 类型。
