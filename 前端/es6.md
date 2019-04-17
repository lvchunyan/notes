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

     2 ** 2 // 4
     2 ** 3 // 8
     2 ** 3 ** 2  //相当于 2 ** (3 ** 2) = 512

## **函数扩展**

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
   
## **数组扩展**

- 