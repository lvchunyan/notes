## 简介

- **babel转码器**

Babel 是一个广泛使用的 ES6 转码器，可以将 ES6 代码转为 ES5 代码，从而在现有环境执行。这意味着，你可以用 ES6 的方式编写程序，又不用担心现有环境是否支持。

     // 转码前
     input.map(item => item + 1);
     
     // 转码后
     input.map(function (item) {
       return item + 1;
     });
     
     //上面的原始代码用了箭头函数，Babel 将其转为普通函数，就能在不支持箭头函数的 JavaScript 环境执行了。
     
  1. 安装 Babel
  ```
     npm install --save-dev @babel/core
  ```
  2. 配置文件.babelrc
  
  Babel 的配置文件是.babelrc，存放在项目的根目录下。使用 Babel 的第一步，就是配置这个文件。
  
  该文件用来设置转码规则和插件，基本格式如下：
  ```
    {
      "presets": [],
      "plugins": []
    }
  ```
  presets字段设定转码规则，官方提供以下的规则集，你可以根据需要安装：
  ```
     # 最新转码规则
     $ npm install --save-dev @babel/preset-env
     
     # react 转码规则
     $ npm install --save-dev @babel/preset-react
    ```
  将这些规则加入.babelrc：
  ```
      {
          "presets": [
            "@babel/env",
            "@babel/preset-react"
          ],
          "plugins": []
      }
  ```
<font color= red>注意，以下所有 Babel 工具和模块的使用，都必须先写好.babelrc。</font>
 
  3. 命令行转码
  
  Babel 提供命令行工具@babel/cli，用于命令行转码。安装命令如下：
  ```
     $ npm install --save-dev @babel/cli
  ```
  
  4. babel/polyfill
  
  Babel 默认只转换新的 JavaScript 句法（syntax），而不转换新的 API，比如Iterator、Generator、Set、Map、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码。
  
  举例来说，ES6 在Array对象上新增了Array.from方法。Babel 就不会转码这个方法。如果想让这个方法运行，必须使用babel-polyfill，为当前环境提供一个垫片。安装命令如下：
  
  ```
     $ npm install --save-dev @babel/polyfill
  ```  
  在脚本头部，加入如下一行代码：
  
  ```
     import '@babel/polyfill';
     // 或者
     require('@babel/polyfill');
  ```  

## let 和 const 命令

- **let命令**

  - 基本用法
  
  ES6 新增了let命令，用来**声明变量**。它的用法类似于var，但是所声明的变量，只在let命令所在的**代码块**内有效。
       {
           let a = 10;
           var b = 1;
       }
              
       a // ReferenceError: a is not defined.
       b // 1
  - 不存在变量提升
  
  let命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错
      // var 的情况
      console.log(foo); // 输出undefined
      var foo = 2;
             
      // let 的情况
      console.log(bar); // 报错ReferenceError
      let bar = 2;
  - 暂时性死区
  
  只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。
      var tmp = 123;
      
      if (true) {
        tmp = 'abc'; // ReferenceError
        let tmp;
      }
  
  - 不允许重复声明
  
  let不允许在相同作用域内，重复声明同一个变量。
      // 不能在函数内部重新声明参数
      // 报错
      function func() {
          let a = 10;
          var a = 1;
      }
                  
       // 报错
       function func() {
          let a = 10;
          let a = 1;
       }
       
       // 报错
       function func(arg) {
         let arg;
       }
       func() 
       
       // 不报错
       function func(arg) {
         {
           let arg;
         }
       }
       func() 

- **块级作用域**
  - ES5无块级作用域
  
  ES5 只有全局作用域和函数作用域，没有块级作用域
      // 第一种场景，内层变量可能会覆盖外层变量。
      var tmp = new Date();
      
      function f() {
        console.log(tmp);
        if (false) {
          var tmp = 'hello world';
        }
      }
      
      f(); // undefined
      
      // 第二种场景，用来计数的循环变量泄露为全局变量。
      var s = 'hello';
      
      for (var i = 0; i < s.length; i++) {
        console.log(s[i]);
      }
      
      console.log(i); // 5
  
  - ES6 的块级作用域
  
  let实际上为 JavaScript 新增了块级作用域
      function f1() {
        let n = 5;
        if (true) {
          let n = 10;
        }
        console.log(n); // 5
      }
      // 函数有两个代码块，都声明了变量n，运行后输出 5。这表示外层代码块不受内层代码块的影响。
      // 如果两次都使用var定义变量n，最后输出的值才是 10
  
  - 块级作用域与函数声明
  
  ES5 规定，函数只能在顶层作用域和函数作用域之中声明，**不能在块级作用域声明**。
  
  ES6 规定，块级作用域之中，函数声明语句的行为类似于let，**在块级作用域之外不可引用**。

- **const命令**
  - 基本用法
  
  const声明一个**只读的常量**。一旦声明，常量的值就不能改变。
  
  const声明的变量不得改变值，这意味着，const一旦声明变量，就必须**立即初始化**，不能留到以后赋值。
  
  const的作用域与let命令相同：只在声明所在的**块级作用域内有效**。
  
  const命令声明的常量也是**不提升**，同样**存在暂时性死区**，只能在声明的位置后面使用。
  
  const声明的常量，也与let一样**不可重复声明**。
       const PI = 3.1415;
       PI // 3.1415
       
       PI = 3;
       // TypeError: Assignment to constant variable.
  
  - ES6 声明变量的六种方法
  
  ES5 只有两种声明变量的方法：var命令和function命令。ES6 除了添加let和const命令，还有另外两种声明变量的方法：import命令和class命令。

## 字符串扩展

- **字符串的遍历器接口（for...of）**

      for (let codePoint of 'foo') {
        console.log(codePoint)
      }
      // "f"
      // "o"
      // "o"
      
- **字符串包含另一个字符串**

JavaScript 只有indexOf方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6 又提供了三种新方法。
 1. includes()：返回布尔值，表示是否找到了参数字符串。
 2. startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
 3. endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。
 
 
      let s = 'Hello world!';
      
      s.startsWith('Hello') // true
      s.endsWith('!') // true
      s.includes('o') // true
      
      // 支持第二个参数，表示开始搜索的位置
      let s = 'Hello world!';
      
      s.startsWith('world', 6) // true
      s.endsWith('Hello', 5) // true
      s.includes('Hello', 6) // false
      // endsWith的行为与其他两个方法有所不同。它针对前n个字符，而其他两个方法针对从第n个位置直到字符串结束
 
- **repeat()重复**

repeat方法返回一个**新字符串**，表示将原字符串**重复n次**, n如果是小数，会被**取整**; 如果repeat的参数是**负数或者Infinity，会报错**; 但是，如果参数是 **0 到-1 之间的小数，则等同于 0。**
      
      'hello'.repeat(2) // "hellohello"
      'na'.repeat(0) // ""
      'na'.repeat(2.9) // "nana"
      'na'.repeat(-0.9) // ""
      'na'.repeat(-1) // RangeError
      
- **补头补尾**

 - padStart()用于**头部补全**，padEnd()用于**尾部补全**。
 - padStart()和padEnd()一共接受两个参数，第一个参数是字符串补全生效的**最大长度**，第二个参数是用来**补全的字符串**。
 - 如果原字符串的长度，**等于或大于最大长度**，则字符串补全不生效，返回**原字符串**。
 - 如果用来补全的字符串与原字符串，两者的长度之和**超过了最大长度**，则会**截去超出位数的补全字符串**。
 - 如果**省略第二个参数**，默认使用**空格补全长度**。
 
## 数值扩展

- **Number.isFinite(), Number.isNaN()**

 - ES6 在Number对象上，新提供了Number.isFinite()和Number.isNaN()两个方法。
 - Number.isFinite()用来检查一个数值是否为有限的（finite），即不是Infinity。
 - Number.isNaN()用来检查一个值是否为NaN。
    ```
     Number.isFinite(15); // true
     Number.isFinite('15'); // false
     Number.isNaN(NaN) // true
     Number.isNaN(15) // false
    ```
- **Number.parseInt(), Number.parseFloat()** 
 
ES6 将全局方法parseInt()和parseFloat()，移植到Number对象上面，行为完全保持不变。
     
     // ES5的写法
     parseInt('12.34') // 12
     parseFloat('123.45#') // 123.45
     
     // ES6的写法
     Number.parseInt('12.34') // 12
     Number.parseFloat('123.45#') // 123.45

- **Number.isInteger()**

Number.isInteger()用来判断一个数值是否为整数。JavaScript 内部，整数和浮点数采用的是同样的储存方法，所以 25 和 25.0 被视为同一个值。
     
     Number.isInteger(25) // true
     Number.isInteger(25.0) // true
     
- **Math 对象的扩展**

 - **Math.trunc()** 
 
     Math.trunc方法用于去除一个数的小数部分，返回整数部分。对于非数值，Math.trunc内部使用Number方法将其先转为数值。对于空值和无法截取整数的值，返回NaN。
        
        Math.trunc(4.1) // 4
        Math.trunc(-0.1234) // -0
        
        Math.trunc('123.456') // 123
        Math.trunc(true) //1
        Math.trunc(false) // 0
        Math.trunc(null) // 0
        
        Math.trunc(NaN);      // NaN
        Math.trunc('foo');    // NaN
        
 - **Math.sign()**
     
     Math.sign方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。
     **参数为正数，返回+1；参数为负数，返回-1；参数为 0，返回0；参数为-0，返回-0;其他值，返回NaN。**

- **指数运算符**
   ```
     2 ** 2 // 4
     2 ** 3 // 8
     2 ** 3 ** 2  //相当于 2 ** (3 ** 2) = 512
   ```
## 函数扩展

- **箭头函数**

 - ES6 允许使用“箭头”（=>）定义函数。
   ```
   var f = v => v;
   
   // 等同于
   var f = function (v) {
     return v;
   };
   ```
 - 如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。   
   ```
   var f = () => 5;
   // 等同于
   var f = function () { return 5 };
   
   var sum = (num1, num2) => num1 + num2;
   // 等同于
   var sum = function(num1, num2) {
     return num1 + num2;
   };
   ```
 - 如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。
   ```
   var sum = (num1, num2) => { return num1 + num2; }
   ```
 - 由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。
   ```
   // 报错
   let getTempItem = id => { id: id, name: "Temp" };
   
   // 不报错
   let getTempItem = id => ({ id: id, name: "Temp" });
   ```
 <font color=red>
 箭头函数有几个使用注意点:</font>
 
   **（1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。**

   **（2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。**
 
   **（3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。**
  
   **（4）不可以使用yield命令，因此箭头函数不能用作 Generator 函数。**
   
## 数组扩展

- **扩展运算符**

 扩展运算符（spread）是三个点（...），将一个数组转为用逗号分隔的参数序列。
      
      console.log(1, ...[2, 3, 4], 5)  // 1 2 3 4 5
 <font color=red> 注意，扩展运算符如果放在括号中，JavaScript 引擎就会认为这是函数调用。如果这时不是函数调用，就会报错。 </font> 

 - **复制数组**   

   数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。
     ```
     const a1 = [1, 2];
     const a2 = a1;
     
     a2[0] = 2;
     a1 // [2, 2]
     // a2并不是a1的克隆，而是指向同一份数据的另一个指针。修改a2，会直接导致a1的变化
     
     // 以下两种写法a2都是a1的克隆
     ES5 只能用变通方法来复制数组:
     const a1 = [1, 2];
     const a2 = a1.concat();
     
     a2[0] = 2;
     a1 // [1, 2]
     
     扩展运算符提供了复制数组的简单写法：
     const a1 = [1, 2];
     // 写法一
     const a2 = [...a1];
     // 写法二
     const [...a2] = a1;
     ```
 - **合并数组**  
     ```
     const arr1 = ['a', 'b'];
     const arr2 = ['c'];
     const arr3 = ['d', 'e'];
     
     // ES5 的合并数组
     arr1.concat(arr2, arr3);
     // [ 'a', 'b', 'c', 'd', 'e' ]
     
     // ES6 的合并数组
     [...arr1, ...arr2, ...arr3]
     // [ 'a', 'b', 'c', 'd', 'e' ]
     ```
 
 - **与解构赋值结合**
     ```
     // ES5
     a = list[0], rest = list.slice(1)
     // ES6
     [a, ...rest] = list
     
     const [first, ...rest] = [1, 2, 3, 4, 5];
     first // 1
     rest  // [2, 3, 4, 5]
     
     // 如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。
     const [first, ...rest] = [];
     first // undefined
     rest  // []
     
     const [first, ...rest] = ["foo"];
     first  // "foo"
     rest   // []
     ```
- **Array.of()**

 Array.of方法用于将一组值，转换为数组。
    ```
    Array.of(3, 11, 8) // [3,11,8]
    Array.of(3) // [3]
    Array.of(3).length // 1
    ```

- **数组实例的find()和findIndex()**

 find()用于找出**第一个符合条件的数组成员**。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员，然后返回该成员。如果没有符合条件的成员，则返回undefined。
    ```
    // 三个参数，依次为当前的值、当前的位置和原数组
    [1, 5, 10, 15].find(function(value, index, arr) {
      return value > 9;
    }) // 10
    ```
 findIndex()方法返回**第一个符合条件的数组成员的位置**，如果所有成员都不符合条件，则返回-1。
    ```
    [1, 5, 10, 15].findIndex(function(value, index, arr) {
      return value > 9;
    }) // 2
    ```

- **数组实例的fill()**
 
 fill方法使用给定值，填充一个数组。fill方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。
    ```
    ['a', 'b', 'c'].fill(7, 1, 2)
    // ['a', 7, 'c']
    ```

- **数组实例的entries(),keys()和values()**

 用于遍历数组。keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历。
    ```
    for (let index of ['a', 'b'].keys()) {
      console.log(index);
    }
    // 0
    // 1
    
    for (let elem of ['a', 'b'].values()) {
      console.log(elem);
    }
    // 'a'
    // 'b'
    
    for (let [index, elem] of ['a', 'b'].entries()) {
      console.log(index, elem);
    }
    // 0 "a"
    // 1 "b"
    
    // 如果不使用for...of循环，可以手动调用遍历器对象的next方法，进行遍历。
    let letter = ['a', 'b', 'c'];
    let entries = letter.entries();
    console.log(entries.next().value); // [0, 'a']
    console.log(entries.next().value); // [1, 'b']
    console.log(entries.next().value); // [2, 'c']
    ```

- **数组实例的includes()**

 Array.prototype.includes方法返回一个布尔值，表示某个数组是否包含给定的值。
    ```
    [1, 2, 3].includes(2)     // true
    [1, 2, 3].includes(4)     // false
    [1, 2, NaN].includes(NaN) // true
    ```
 该方法的第二个参数表示搜索的**起始位置**，默认为0。如果第二个参数为负数，则表示倒数的位置，如果这时它**大于数组长度**（比如第二个参数为-4，但数组长度为3），则会**重置为从0**开始。
    ```
    [1, 2, 3].includes(3, 3);  // false
    [1, 2, 3].includes(3, -1); // true
    
    使用数组的indexOf方法，检查是否包含某个值
    if (arr.indexOf(el) !== -1) {
      // ...
    }
    ```
## 对象扩展

- **属性的遍历**
 - ES6 一共有 5 种方法可以遍历对象的属性。
   
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

- **对象的扩展运算符**
    ```
    let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
    x // 1
    y // 2
    z // { a: 3, b: 4 }
    ```
## 对象的新增方法

 - **Object.is()**
 
   用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致
    ```
    不同之处只有两个：一是+0不等于-0，二是NaN等于自身。
    +0 === -0 //true
    NaN === NaN // false
    
    Object.is(+0, -0) // false
    Object.is(NaN, NaN) // true
    ```
 - **Object.assign()**
 
   Object.assign方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）;如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。
   ```
   const target = { a: 1, b: 1 };
   
   const source1 = { b: 2, c: 2 };
   const source2 = { c: 3 };
   
   Object.assign(target, source1, source2);
   target // {a:1, b:2, c:3}
   // 或者
   const obj = Object.assign({},target, source1, source2)
   obj // {a:1, b:2, c:3}
   ```
 - **Object.keys()，Object.values()，Object.entries()**
   - **Object.keys()**
    ```
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
   - **Object.values()**
    
    Object.values方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历属性的键值。
     ```
     const obj = { foo: 'bar', baz: 42 };
     Object.values(obj)
     // ["bar", 42]
     ```
    
   - **Object.entries()**
    
    Object.entries()方法返回一个数组，成员是参数对象自身的（不含继承的）所有可遍历属性的键值对数组。
     ```
     const obj = { foo: 'bar', baz: 42 };
     Object.entries(obj)
     // [ ["foo", "bar"], ["baz", 42] ]
     ```
     
## Symbol(数据类型)

 - Symbol 值通过**Symbol函数**生成。对象的**属性名**现在可以有两种**类型**，一种是原来就有的**字符串**，另一种就是新增的 **Symbol 类型**。凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突。
 
 - Symbol函数的参数只是表示对当前 Symbol 值的描述，因此**相同参数**的Symbol函数的**返回值是不相等的**。
  ```
  // 没有参数的情况
  let s1 = Symbol();
  let s2 = Symbol();
  
  s1 === s2 // false
  
  // 有参数的情况
  let s1 = Symbol('foo');
  let s2 = Symbol('foo');
  
  s1 === s2 // false
  ```
## Set和Map数据结构

- **Set**
 
 Set本身是一个构造函数，用来生成 Set 数据结构。 Set数据结构类似于数组，成员的值都是唯一的，没有重复的值。
  ```
   const s = new Set();
   [2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x)){
       for(let i of s){
          
       }
   }
  ```
- **WeakSet**


- **Map**



- **WeakMap**

 
 