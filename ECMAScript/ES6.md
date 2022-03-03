# 一、let和const命令

在ES6中不再建议使用var声明变量，而是使用`let`和`const`,分别用于声明变量和常量,不同与var命令,`let`和`const`都是块级作用域。

```JavaScript
const DELAY=1000;
let count = 0;
count += 1;
```

# 二、析构赋值

ES6允许按照一定模式,从数组和对象中提取值,对变量就行赋值,这被称为析构(Destructuring)

```JavaScript
        //数组的析构赋值
        let [a,b,c] =[1,2,3];
        console.info(a,b,c);
        //析构赋值允许指定默认值
        let [x,y='b']=['a'];
        console.info(x,y);
        //对象额析构赋值
        let {name,age}={name:"zen",age:30};
        console.info(name,age);
        //字符串的析构赋值
        const [q,w,e,r,t] = "hello";
        console.info(q,w,e,r,t);        
        //函数参数的析构赋值
        function add([x,y]){
            return x+y;
        }
        console.info(add([2,3]));
        //不定参数求和
        function sum(...args){
            return args.reduce(function(x,y){
                return x+y;
            },0)


            // return args.reduce((pre,cur)=>{
            //     return cur+pre;
            // },0)
        }
        console.info(sum(1,2,3,4,5,6,7));
```

## 变量的析构赋值使用场景很多。例如

### 1.交换变量的值

```JavaScript
        {
            let x = 1;
            let y = 2;
            [x,y] = [y,x];
            console.info(x,y);
        }
```

### 2.从函数返回多个值

```JavaScript
        
    function example(){
    return [1,2,3]
    }
    console.info(example());

    let [a,b,c] = example();
    console.info(a,b,c);
    function example2(){
    return{
        foo:1,
        bar:2
    };
    let{foo,bar} = example2();
    console.info(example2());  
```

### 3.函数参数的定义

```JavaScript
        {
            // 参数是一组有次序的值 
            function f([x, y, z]) {

            } 
            f([1, 2, 3]);
            // 参数是一组无次序的值
             function f({x, y, z}) {

             } 
             f({z: 3, y: 2, x: 1});
        }
```

### 4.提取JSON数据

```JavaScript
        
    let jsonData = { id: 42, status: "OK", data: [867, 5309] };
    let { id, status, data: number } = jsonData; 
    console.log(id, status, number);
        
```

### 5. 函数参数的默认值

```JavaScript
    jQuery.ajax = function (url, {
            async = true,
            beforeSend = function () {}, 
            cache = true, 
            complete = function () {}, 
            crossDomain = false, 
            global = true, 
            // ... more config 
    }) {
            // ... do stuff 
    };
```

### 6. 遍历Map结构

任何部署了Iterator接口的对象，都可以用 for...of 循环遍历。Map结构原生支持Iterator接口，配合变量的 解构赋值，获取键名和键值就非常方便。

```JavaScript
        const map = new Map();
        map.set('first', 'hello'); 
        map.set('second', 'world'); 
        for (let [key, value] of map) {
             console.log(key + " is " + value
             ); 
            }
        // 获取键名 
        for (let [key] of map) { 
            console.info(key);
        }
        // 获取键值 
        for (let [,value] of map) { 
            console.log(value);

        }
```

### 7. 输入模块的指定方法

加载模块时，往往需要指定输入哪些方法。解构赋值使得输入语句非常清晰。

```JavaScript
const { SourceMapConsumer, SourceNode } = require("source‐map");
```

# 三、字符串的扩展

### 1. includes()、startsWith()、endsWith()

传统上，JavaScript只有 indexOf 方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6又提供了 三种新方法。

* includes()：返回布尔值，表示是否找到了参数字符串。
* startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
* endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。

```JavaScript

        let s = 'Hello world!';
        console.log(s.startsWith('Hello'));// true
        console.log(s.endsWith('!'));// true
        console.log(s.includes('o'));// true

        console.log(s.startsWith('world', 6)) // true 
        console.log(s.endsWith('Hello', 5)) // true 
        console.log(s.includes('Hello', 6)) // false

```

### 2. repeat()

```JavaScript
        console.log('x'.repeat(3));//xxx
        console.log('hello'.repeat(2));//hellohello
        console.log('na'.repeat(0));//""
```

### 3. padStart(),padEnd()

ES2017 引入了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。 padStart() 用于头部补全， padEnd() 用于尾部补全。

```JavaScript
        console.log('x'.padStart(5,'ab'));//ababx
        console.log('x'.padStart(4,'ab'));//abax

        console.log('x'.padEnd(5,'ab'));//xabab
        console.log('x'.padEnd(4,'ab'));//xaba
        //如果原字符串的长度，等于或大于指定的最小长度，则返回原字符串。
        console.log('xxx'.padStart(2,'ab'));//xxx
        console.log('xxx'.padEnd(2,'ab'));//xxx
        //如果用来补全的字符串与原字符串，两者的长度之和超过了指定的最小长度，则会截去超出位数的补全字符串。
        console.log('abc'.padStart(10,'0123456789'));//0123456abc
        //如果省略第二个参数，默认使用空格补全长度。
        console.log('x'.padStart(4));//'    x'
        console.log('x'.padEnd(4));//'x    '
        //padStart的常见用途是为数值补全指定位数。下面代码生成10位的数值字符串。
        console.log('1'.padStart(10,'0'));//"0000000001"
        console.log('12'.padStart(10,'0'));//"0000000012"
        console.log('123456'.padStart(10,'0'));//"0000123456"
        //另一个用途是提示字符串格式。
        console.log('12'.padStart(10, 'YYYY-MM-DD')); // "YYYY‐MM‐12" 
        console.log('09-12'.padStart(10, 'YYYY-MM-DD')); // "YYYY‐09‐12"
```

### 4. 模板字符串

ES6中引入了模板字符串解决传统的字符串拼接问题。

```JavaScript
        const user ='world';
        console.log(`hello ${user}`);//hello world
        const firstName = 'zen';
        const qty = 3;
        const event ='event';
        //模板字符串``
        const content = `
        Hello ${firstName},
        Thanks for ordering ${qty} tickets to ${event}`;
        console.log(content);
```

模板字符串的功能，不仅用于字符串拼接。它可以紧跟在一个函数名后面，该函数将被调用来处理这个模板 字符串。这被称为“标签模板”功能（tagged template）。

```JavaScript
        alert`123` // 等同于 alert(123)

        //“标签模板”的一个重要应用，就是过滤 HTML 字符串，防止用户输入恶意内容。 
        let sender = '<script>alert("abc")</script>'; // 恶意代码 
        let message = SaferHTML`<p>${sender} has sent you a message.</p>`;
        标签模板的另一个应用，就是多语言转换（国际化处理）。 
        i18n`Welcome to ${siteName}, you are visitor number ${visitorNumber}!` 
        // "欢迎访问xxx，您是第xxxx位访问者！"
```

# 4.数值的扩展

### 1. 二进制和八进制表示法 ES6 提供了二进制和八进制数值的新的写法，分别用前缀 0b （或 0B ）和 0o （或 0O ）表示

```JavaScript
        console.log(0b111110111 === 503); // true 
        console.log(0o767 === 503); // true
```

### 2. Number.parseInt(), Number.parseFloat()

```JavaScript
        // ES5的写法 
        console.log(parseInt('12.34')); // 12 
        console.log(parseFloat('123.45#')); // 123.45
        // ES6的写法 
        console.log(Number.parseInt('12.34')); // 12 
        console.log(Number.parseFloat('123.45#')); // 123.45
```

### 3. Number.isInteger()

`Number.isInteger()` 用来判断一个值是否为整数。需要注意的是，在 JavaScript 内部，整数和浮点数是同样 的储存方法，所以3和3.0被视为同一个值。

```JavaScript
        console.log(Number.isInteger(25)); // true 
        console.log(Number.isInteger(25.0)); // true 
        console.log(Number.isInteger(25.1)); // false 
        console.log(Number.isInteger("15")); // false 
        console.log(Number.isInteger(true)); // false
```

### 4. Math对象的扩展

ES6在Math对象上新增了17个与数学相关的方法。相关方法可在附录链接中查询，在此仅对相对常用的一个 方法做简单说明。

```JavaScript
        console.log(Math.trunc(4.1)); // 4 
        console.log(Math.trunc(4.9)); // 4 
        console.log(Math.trunc(-4.1)); // ‐4 
        console.log(Math.trunc(-4.9)); // -4 
        console.log(Math.trunc(-0.1234)); // -0
```

### 5. 指数运算符

```JavaScript
        console.log(2 ** 2); // 4 
        console.log(2 ** 3); // 8 
        //指数运算符可以与等号结合，形成一个新的赋值运算符（**=）。 
        let a = 1.5; 
        console.log(a **= 2); // 等同于 a = a * a; 
        let b = 4; 
        console.log(b **= 3); // 等同于 b = b * b * b;
```

# 五、函数的扩展

### 1. 函数参数的默认值

```JavaScript
        function log(x,y='World'){
            console.log(x,y);
        }
        log('Hello');
        log('Hello','China');
        log('Hello','');

        function Point(x=0,y=0){
            this.x = x;
            this.y = y;
        }
        const p = new Point();
        console.log(p);

```

### 2. rest参数

ES6 引入 rest 参数（形式为 `...变量名` ），用于获取函数的多余参数，这样就不需要使用 `arguments` 对象 了。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。

```JavaScript
        function add(...values){
            let sum = 0;
            for(var val of values){
                sum+=val;
            }
            return sum;
        }

        console.log(add(2,5,3,4));

```

注意，rest 参数之后不能再有其他参数（即只能是最后一个参数），否则会报错。

```JavaScript
        // 报错 
        function f(a, ...b, c) {
            // ... 
            }
```
### 3. 箭头函数

```JavaScript
        var f = v =>v;
        //等同于
        // var f = function(v){
        //     return v;
        // }
        console.log(f(5));
        //如果箭头函数不需要参数或需要多个参数，就是用一个圆括号代表参数部分
        var f =()=>5;
        //等同于
        var f = function(){return 5}

        var sum = (mum1,mum2)=>num1+num2;
        //等同于
        var sum = function(num1,num2){
            return num1+num2;
        }
        //如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。 
        var sum = (num1, num2) => { return num1 + num2; }

```
