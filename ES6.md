## 学习教材
https://es6.ruanyifeng.com/

## 1.let和var的区别
 - let是ES6新增的，用来声明变量。只在所在代码块生效。var声明的变量在代码块外面也能使用。
 - let不允许重复声明，var允许。
 
 - for循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。
 
 ```javascript
 for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
 }
 // abc
 // abc
 // abc
 ```
 
## 2.常量定义
 ```javascript
 const a = "a";
 const b = {}; //{}表示对象，[]表示数组
 ```
 const声明常量实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。
 对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。
 但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，
 const只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，
 就完全不能控制了。因此，将一个对象声明为常量必须非常小心。
 

## 3.变量的解构赋值
 ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。
 
 #### 数组解构
 ```javascript
 let [foo, [[bar], baz]] = [1, [[2], 3]];
 foo // 1
 bar // 2
 baz // 3
 ```
 只要等号两边模式一致，就可以给左边的变量赋予对应的值。
 如果解构不成功，变量的值就等于undefined。
 数组结构赋值是浅拷贝，如果右边是对象，只会拷贝引用（对象解构也是一样）
 ```javascript
 let [x, y, ...z] = ['a'];
 x // "a"
 y // undefined
 z // []
 // 注 : ...可以接收多个值，当作数组处理。
 ```
 
 默认值，当解构不成功时，赋值为默认值。
 ```javascript
 let [x, y = 'b'] = ['a']; // x='a', y='b'
 ```
 
 #### 对象解构
 ```javascript
 let { foo, bar } = { foo: 'aaa', bar: 'bbb' };
 foo // "aaa"
 bar // "bbb"
 ```
 对象的解构是需要变量名和属性名同名才能取到对应的值。
 没有取到则为undefined
 
 变量名和属性名不一致时，需要写成下面形式
 ```javascript
 let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
 baz // "aaa"
 ```
 foo：baz中的foo只是用来匹配的，不会被赋值。
 
 对象解构可以方便的将对象的方法赋值给变量，
 ```javascript
 let { log, sin, cos } = Math;
 ```
 
 在模块加载时，用来加载模块中的对应方法。
 ```javascript
 import { connect } from 'react-redux';
 ```
 
 ## 4.字符串
  #### 码点解析方法
  把码点解析为字符：String.fromCodePoint(0x20BB7); ES6提供的方法。
  ES5 提供String.fromCharCode()方法，用于从 Unicode 码点返回对应字符，但是这个方法不能识别码点大于0xFFFF的字符。
  
  #### 字符串遍历for ...of
  ```javascript
  for (let codePoint of 'foo') {
   console.log(codePoint)
  }
  // "f"
  // "o"
  // "o"
  ```
 
 #### 字符串模板（使用反引号[`]）
 可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。
 ```javascript
 // 普通字符串
 `In JavaScript '\n' is a line-feed.`
 // 多行字符串
 console.log(`string text line 1
 string text line 2`);
 // 字符串中嵌入变量
 let name = "Bob", time = "today";
 `Hello ${name}, how are you ${time}?`
 ```
 

 #### 新增方法
 - JavaScript 只有indexOf方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6 又提供了三种新方法。
   - includes()：返回布尔值，表示是否找到了参数字符串。
   - startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
   - endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。
 
 - repeat():方法返回一个新字符串，表示将原字符串重复n次。
  'hello'.repeat(2) // "hellohello"
 - padStart(),padEnd()如果某个字符串不够指定长度，会在头部或尾部补全
  'x'.padStart(5, 'ab') // 'ababx'
  'x'.padEnd(5, 'ab') // 'xabab'
 - trimStart(),trimEnd(),trim()删除开头/末尾/开头和末尾的空格(tab 键、换行符等不可见的空白符号也有效。)
 - matchAll()方法返回一个正则表达式在当前字符串的所有匹配
  
  
## 5.正则表达式
  语法：/正则表达式主体/修饰符(可选)
  
  #### 修饰符
  ##### ES5现存修饰符
  - i	执行对大小写不敏感的匹配。
  - g	执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。
  - m	执行多行匹配。
  
  ##### ES6新增修饰符
  - u 含义为“Unicode 模式”，用来正确处理大于\uFFFF的 Unicode 字符。
    ```javascript
    /^\uD83D/u.test('\uD83D\uDC2A') // false
    /^\uD83D/.test('\uD83D\uDC2A') // true
    ```
    \uD83D\uDC2A是一个四个字节的 UTF-16 编码，代表一个字符。但是，ES5 不支持四个字节的 UTF-16 编码，
    会将其识别为两个字符，导致第二行代码结果为true。加了u修饰符以后，ES6 就会识别其为一个字符，所以第一行代码结果为false。
  - y “粘连”（sticky）修饰符。自带头部匹配（^）。从字符串开头或指定位置开始匹配，并且要符合头部匹配。
  - s dotAll 模式，使点表示一切字符。
    点（.）是一个特殊字符，代表任意的单个字符，但是有两个例外。一个是四个字节的 UTF-16 字符，这个可以用u修饰符解决；
    另一个是行终止符：U+000A 换行符（\n），U+000D 回车符（\r），U+2028 行分隔符（line separator），U+2029 段分隔符（paragraph separator）
 
 #### 断言
 ##### 先行断言
 “先行断言”指的是，x只有在y前面才匹配，必须写成`/x(?=y)/`。比如，只匹配百分号之前的数字，要写成`/\d+(?=%)/`。
 “先行否定断言”指的是，x只有不在y前面才匹配，必须写成`/x(?!y)/`。比如，只匹配不在百分号之前的数字，要写成`/\d+(?!%)/`。
 ```javascript
 /\d+(?=%)/.exec('100% of US presidents have been male')  // ["100"]
 /\d+(?!%)/.exec('that’s all 44 of them')                 // ["44"]
 ```
 
 ##### 后行断言（ES2018 引入后行断言）
 “后行断言”正好与“先行断言”相反，x只有在y后面才匹配，必须写成`/(?<=y)x/`。比如，只匹配美元符号之后的数字，要写成`/(?<=\$)\d+/`。
 “后行否定断言”则与“先行否定断言”相反，x只有不在y后面才匹配，必须写成`/(?<!y)x/`。比如，只匹配不在美元符号后面的数字，要写成`/(?<!\$)\d+/`。
 ```javascript
 /(?<=\$)\d+/.exec('Benjamin Franklin is on the $100 bill')  // ["100"]
 /(?<!\$)\d+/.exec('it’s is worth about €90')                // ["90"]
 ```
 
  #### 元字符
  - \w 查找字母数字和下划线 
  - \W 查找非字母数字和下划线 
  - \d	查找数字。
  - \D 查找非数字
  - \s	查找空白字符。
  - \S 查找非空白字符
  - \b	匹配单词边界。
  - \uxxxx	查找以十六进制数 xxxx 规定的 Unicode 字符。
  
  #### 具名组匹配
  正则表达式使用圆括号进行组匹配。
  ES2018 引入了具名组匹配（Named Capture Groups），允许为每一个组匹配指定一个名字，既便于阅读代码，又便于引用。
  ```javascript
  const RE_DATE = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;

  const matchObj = RE_DATE.exec('1999-12-31');
  //["1999-12-31", "1999", "12", "31", index: 0, input: "1999-12-31", groups: undefined]
  const year = matchObj.groups.year; // 1999
  const month = matchObj.groups.month; // 12
  const day = matchObj.groups.day; // 31
  ```
  
  #### 正则表达式相关的方法
  String.prototype.match 调用 RegExp.prototype[Symbol.match]
   在设置了 global 或 sticky 标志位的情况下（如 /foo/g or /foo/y）,会返回一个符合条件的所有子字符串的数组。
  
  String.prototype.replace 调用 RegExp.prototype[Symbol.replace]
  String.prototype.search 调用 RegExp.prototype[Symbol.search]
  String.prototype.split 调用 RegExp.prototype[Symbol.split]
  
  String.prototype.matchAll() 可以一次性取出所有匹配。不过，它返回的是一个遍历器（Iterator），而不是数组。
  
  RegExp.prototype.exec
   在设置了 global 或 sticky 标志位的情况下（如 /foo/g or /foo/y），JavaScript RegExp 对象是有状态的。
   他们会将上次成功匹配后的位置记录在 lastIndex 属性中。直到匹配失败，lastIndex被设为0。
   使用此特性，exec() 可用来对单个字符串中的多次匹配结果进行逐条的遍历（包括捕获到的匹配），
   而相比之下， String.prototype.match() 只会返回匹配到的结果。
   
  RegExp.prototype.test
 
## 6.函数
 #### 支持默认值
 函数支持默认值了，当参数值严格等于undefined是才会使用默认值（typeof y === 'undefined'）。
 当默认值是计算式时，是惰性求值。
 ```javascript
 function log(x, y = 'World') {
   console.log(x, y);
 }
 ```

 #### 变量赋值为函数时，带括号表示把运行结果赋值，不带括号表示把定义赋值。
 ```javascript
 function throwIfMissing() {
  throw new Error('Missing parameter');
 }
 let x = throwIfMissing()
 let y = throwIfMissing
 ```

#### rest参数
 ES6 引入 rest 参数（形式为 ...变量名），用于获取函数的多余参数
 function add(...values)values可以接受多个参数，是一个数组

 #### 箭头函数
 ```javascript
 var sum = (num1, num2) => num1 + num2;
 // 等同于
 var sum = function(num1, num2) {
  return num1 + num2;
 };
 ```

 由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。
 ```javascript
 // 报错
 let getTempItem = id => { id: id, name: "Temp" };

 // 不报错
 let getTempItem = id => ({ id: id, name: "Temp" });
 ```
 
 箭头函数有几个使用注意点。
（1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
（2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
（3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
（4）不可以使用yield命令，因此箭头函数不能用作 Generator 函数。
 上面四点中，第一点尤其值得注意。this对象的指向是可变的，但是在箭头函数中，它是固定的。
 
 
 ## 对象
 #### 简洁表示法
 ES6 允许在大括号里面，直接写入变量和函数，作为对象的属性和方法。
 return {x, y}; 
 let obj = {x, y};
 ```javascript
 let birth = '2000/01/01';

 const Person = {

  name: '张三',

  //等同于birth: birth
  birth,

  // 等同于hello: function ()...
  hello() { console.log('我的名字是', this.name); }

};
 ```
 
 #### 属性名表达式
 属性名可以使用变量或者表达式，用`[]`括起来。
 ```javascript
 obj['a' + 'bc'] = 123;
 
 let obj = {
  [propKey]: true,
  ['a' + 'bc']: 123,
  
  // 方法名也可以使用表达式
  ['h' + 'ello']() {
    return 'hi';
  }
};
```

 #### 属性的可枚举性和遍历
 ##### 可枚举性
 对象的每个属性都有一个描述对象（Descriptor），用来控制该属性的行为。Object.getOwnPropertyDescriptor方法可以获取该属性的描述对象。
 ```javascript
 let obj = { foo: 123 };
 Object.getOwnPropertyDescriptor(obj, 'foo')
 //  {
 //    value: 123,
 //    writable: true,
 //    enumerable: true,
 //    configurable: true
 //  }
 ```
 描述对象的enumerable属性，称为“可枚举性”，如果该属性为false，就表示某些操作会忽略当前属性。

 目前，有四个操作会忽略enumerable为false的属性。
 for...in循环：只遍历对象自身的和继承的可枚举的属性。
 Object.keys()：返回对象自身的所有可枚举的属性的键名。
 JSON.stringify()：只串行化对象自身的可枚举的属性。
 Object.assign()： 忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性。
 
 ##### 属性的遍历
 （1）for...in
 for...in循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）。

（2）Object.keys(obj)
 Object.keys返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名。

（3）Object.getOwnPropertyNames(obj)
 Object.getOwnPropertyNames返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是包括不可枚举属性）的键名。

（4）Object.getOwnPropertySymbols(obj)
 Object.getOwnPropertySymbols返回一个数组，包含对象自身的所有 Symbol 属性的键名。

（5）Reflect.ownKeys(obj)
 Reflect.ownKeys返回一个数组，包含对象自身的所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。
 以上的 5 种方法遍历对象的键名，都遵守同样的属性遍历的次序规则。
 首先遍历所有数值键，按照数值升序排列。
 其次遍历所有字符串键，按照加入时间升序排列。
 最后遍历所有 Symbol 键，按照加入时间升序排列。
 ```javascript
 Reflect.ownKeys({ [Symbol()]:0, b:0, 10:0, 2:0, a:0 })
 // ['2', '10', 'b', 'a', Symbol()]
 ```
上面代码中，Reflect.ownKeys方法返回一个数组，包含了参数对象的所有属性。这个数组的属性次序是这样的，首先是数值属性2和10，其次是字符串属性b和a，最后是 Symbol 属性。

#### super 关键字
this关键字总是指向函数所在的当前对象，ES6 又新增了另一个类似的关键字super，指向当前对象的原型对象。
```javascript
const proto = {
  x: 'hello',
  foo() {
    console.log(this.x);
  },
};

const obj = {
  x: 'world',
  foo() {
    super.foo();
  }
}

Object.setPrototypeOf(obj, proto);

obj.foo() // "world"
````
super.foo指向原型对象proto的foo方法，但是绑定的this却还是当前对象obj，因此输出的就是world。
 
 #### 链判断运算符
 ```javascript
 const firstName = message?.body?.user?.firstName || 'default';
 const fooValue = myForm.querySelector('input[name=foo]')?.value
 ```
 上面代码使用了?.运算符，直接在链式调用的时候判断，左侧的对象是否为null或undefined。如果是的，就不再往下运算，而是返回undefined。

 链判断运算符有三种用法。
 obj?.prop // 对象属性
 obj?.[expr] // 同上
 func?.(...args) // 函数或对象方法的调用
 
 #### Null 判断运算符
 ES2020 引入了一个新的 Null 判断运算符??。只有运算符左侧的值为null或undefined时，才会返回右侧的值。
 ```javascript
 const headerText = response.settings.headerText ?? 'Hello, world!';
 const animationDuration = response.settings.animationDuration ?? 300;
 const showSplashScreen = response.settings.showSplashScreen ?? true;
 ```
 运算优先级问题
 与&&和||的优先级孰高孰低。必须用括号表明优先级，否则会报错。
 
 #### 对象新增方法
 ##### Object.is() 比较连个对象是否相等。并解决+0和-0相等，NaN不等于自身问题
 ```javascript
 +0 === -0 //true
 NaN === NaN // false

 Object.is(+0, -0) // false
 Object.is(NaN, NaN) // true
 ```
 ##### Object.assign()
 用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。
 ```javascript
 const target = { a: 1, b: 1 };
 const source1 = { b: 2, c: 2 };
 const source2 = { c: 3 };

 Object.assign(target, source1, source2);
 target // {a:1, b:2, c:3}
 ```
 重名的属性，起那面的会被后面的覆盖。
 注意点：
 - Object.assign方法实行的是浅拷贝
 - 同名属性的替换
 - Object.assign可以用来处理数组，但是会把数组视为对象。
    ```javascript
    Object.assign([1, 2, 3], [4, 5])//[1, 2, 3]=>{0:1, 1:2, 2:3}  [4, 5]=>{0:4, 1:5}
    // [4, 5, 3]
   ```
 ##### `__proto__`属性，`Object.setPrototypeOf()`，`Object.getPrototypeOf()`
 javascript是基于原型的，继承是基于原型链。
 每个对象都有一个私有属性（__proto__），指向自己的原型对象prototype。原型对象也有__proto__指向自己的原型对象，由此形成了原型链。
 原型链最上层是null（没有原型）。
 Object.setPrototypeOf 设置一个对象的prototype对象
 ```javascript
 let proto = {};
 let obj = { x: 10 };
 Object.setPrototypeOf(obj, proto);

 proto.y = 20;
 proto.z = 40;

 obj.x // 10
 obj.y // 20
 obj.z // 40
 ```
 Object.getPrototypeOf() 读取原型对象。
 
 ##### Object.keys()，Object.values()，Object.entries()
 Object.keys 返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键名。
 Object.values 返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值。
 Object.entries 返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历（enumerable）属性的键值对数组。
 ```javascript
 let {keys, values, entries} = Object;
 let obj = { a: 1, b: 2, c: 3 };

 for (let key of keys(obj)) {
  console.log(key); // 'a', 'b', 'c'
 }

 for (let value of values(obj)) {
  console.log(value); // 1, 2, 3
 }

 for (let [key, value] of entries(obj)) {
  console.log([key, value]); // ['a', 1], ['b', 2], ['c', 3]
 }
 ```
 ##### Object.fromEntries()
 Object.fromEntries()方法是Object.entries()的逆操作，用于将一个键值对数组转为对象。
 ```javascript
 // 例一
 const entries = new Map([
   ['foo', 'bar'],
   ['baz', 42]
 ]);

 Object.fromEntries(entries)
 // { foo: "bar", baz: 42 }

 // 例二
 const map = new Map().set('foo', true).set('bar', false);
 Object.fromEntries(map)
 // { foo: true, bar: false }
 
 // 配合URLSearchParams对象，将查询字符串转为对象
 Object.fromEntries(new URLSearchParams('foo=bar&baz=qux'))
 // { foo: "bar", baz: "qux" }
 ```
 
 ## Symbol
 ES6新加入的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。
 用来保证每个属性的名字的独一无二，防止属性名的冲突。
 
 ## 模块加载
 ES6 在语言标准的层面上，实现了模块功能，在编译时就能确定模块的依赖关系，以及输入和输出的变量。
 
 模块功能主要由两个命令构成：export和import。export命令用于规定模块的对外接口，import命令用于输入其他模块提供的功能。
 
 export分为命名导出和默认导出   
 命名导出后的接口，导入时需要用一样的名称，并且用大括号括起来。   
 export var firstName = 'Michael';     
 export { firstName, lastName, year };      
 import { firstName, lastName, year } from './xxx.js';      
 默认导出最多只能一次，导入时不需要知道导出接口的名字，可以任意取别名，并且不用大括号。   
 export default foo;
 import abc from './xxx.js';    
 
 // ES6模块   
 import { stat, exists, readFile } from 'fs';

 上面代码的实质是从`fs`模块加载 3 个方法，其他方法不加载。这种加载称为“编译时加载”或者静态加载
 即 ES6 可以在编译时就完成模块加载，效率要比 CommonJS 模块的加载方式高。
 当然，这也导致了没法引用 ES6 模块本身，因为它不是对象。
 ES6 模块不是对象，而是通过`export`命令显式指定输出的代码，再通过`import`命令输入。
 
 模块的整体加载
 除了指定加载某个输出值，还可以使用整体加载，即用星号（*）指定一个对象，所有输出值都加载在这个对象上面。
 import * as circle from './circle';
 
 
 
 
 
 
 
 
 
 















