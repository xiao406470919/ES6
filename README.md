# es6新增特性

个人主观意见整理es6中常用十个特性：
1、默认参数
2、模版表达式
3、多行字符串
4、解构赋值
5、改进的对象表达式
6、箭头函数 =&>
7、Promise
8、块级作用域的let和const
9、类
10、模块化

首先简单的了解一下JavaScript的发展历史就是它的时间线：
1、1995年：JavaScript以LiveScript之名诞生
2、1997年：ECMAScript标准确立
3、1999年：ES3发布，IE5非常生气
4、2000年-2005年：XMLHttpRequest，熟知为AJAX，在如Outlook Web Access(2002)、Oddpost(2002)、Gmail(2004)、Google Maps(2005)中得到了广泛的应用
5、2009年：ES5发布（这是我们目前用的最多的版本），带来了forEach / Object.keys / Object.create（特地为Douglas Crockford所造，JSON标准创建者） ，还有JSON标准。
6、2015：ES6/ECMAScript2015出现

了解完历史我们来进入正题：

### 1. ES6中的默认参数

我们以前要这样子来定义默认参数：

```
var link = function (height, color, url) {
    var height = height || 50;
    var color = color || 'red';
    var url = url || 'http://azat.co';
    ……
}
```

这样做一直都没什么问题，直到参数的值为0，因为0在JavaScript中算是false值，它会直接变成后面硬编码的值而不是0本身。当然了，谁要用0来传值啊（讽刺脸）？所以我们也忽略了这个瑕疵，沿用了这个逻辑，否则的话只能…..没有否则！在ES6中，我们可以把这些默认值直接放在函数签名中。

```
var link = function(height = 50, color = 'red', url = 'http://azat.co') {
  ……
}
```

类似于Ruby的语法

### 2. ES6中的模版表达式

在其它语言中，使用模板和插入值是在字符串里面输出变量的一种方式。因此，在ES5，我们可以这样组合一个字符串：

```
var name = 'Your name is ' + first + ' ' + last + '.';
var url = 'http://localhost:3000/api/messages/' + id;
```

在ES6中我们有了新语法，在反引号包裹的字符串中，使用${NAME}语法来表示模板字符:

```
var name = `Your name is ${first} ${last}. `;
var url = `http://localhost:3000/api/messages/${id}`;
```

### 3. ES6中的多行字符串

ES6的多行字符串是一个非常实用的功能。在ES5中，我们不得不使用以下方法来表示多行字符串：

```
var roadPoem = 'Then took the other, as just as fair,nt'
    + 'And having perhaps the better claimnt'
    + 'Because it was grassy and wanted wear,nt'
    + 'Though as for that the passing therent'
    + 'Had worn them really about the same,nt';
var fourAgreements = 'You have the right to be you.n
    You can only be you when you do your best.';
```

但是在ES6中，只要充分利用反引号就可以解决了：

```
var roadPoem = `Then took the other, as just as fair,
    And having perhaps the better claim
    Because it was grassy and wanted wear,
    Though as for that the passing there
    Had worn them really about the same,`;
var fourAgreements = `You have the right to be you.
    You can only be you when you do your best.`;
```

### 4.解构赋值

解构可能是一个比较难以掌握的概念。先从一个简单的赋值讲起，其中house 和 mouse是key，同时house 和mouse也是一个变量，在ES5中是这样：

```
var data = $('body').data(), // data has properties house and mouse
   house = data.house,
   mouse = data.mouse;
```

以及在node.js中用ES5是这样：

```
var jsonMiddleware = require('body-parser').jsonMiddleware ;
var body = req.body, // body has username and password
   username = body.username,
   password = body.password;
```

在ES6，我们可以使用这些语句代替上面的ES5代码：

```
var { house, mouse} = $('body').data(); // we'll get house and mouse variables
var {jsonMiddleware} = require('body-parser');
var {username, password} = req.body;
```

这个同样也适用于数组，非常赞的用法：

```
var [col1, col2]  = $('.column'),
   [line1, line2, line3, , line5] = file.split('n');
```

### 5.增强的对象字面量

使用对象文本可以做许多让人意想不到的事情！通过ES6，我们可以把ES5中的JSON变得更加接近于一个类。

下面是一个典型ES5对象文本，里面有一些方法和属性：

```
var serviceBase = {port: 3000, url: 'azat.co'},
    getAccounts = function(){return [1,2,3]};
var accountServiceES5 = {
  port: serviceBase.port,
  url: serviceBase.url,
  getAccounts: getAccounts,
   toString: function() {
      return JSON.stringify(this.valueOf());
  },
  getUrl: function() {return "http://" + this.url + ':' + this.port},
  valueOf_1_2_3: getAccounts()
}
```

如果我们想让它更有意思，我们可以用Object.create从serviceBase继承原型的方法：

```
var accountServiceES5ObjectCreate = Object.create(serviceBase)
var accountServiceES5ObjectCreate = {
  getAccounts: getAccounts,
  toString: function() {
    return JSON.stringify(this.valueOf());
  },
  getUrl: function() {return "http://" + this.url + ':' + this.port},
  valueOf_1_2_3: getAccounts()
}
```

我知道 accountServiceES5ObjectCreate 和 accountServiceES5 是不完全相同的。因为一个对象 accountServiceES5 会有如下所示的 **proto** 属性：



![img](https://upload-images.jianshu.io/upload_images/8164826-d52385e04203f65c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/457/format/webp)

image.png

为了方便举例，我们将考虑它们的相似处。所以在ES6的对象文本中，既可以直接分配getAccounts: getAccounts,也可以只需用一个getAccounts，此外，我们在这里通过**proto**（并不是通过’proto’）设置属性，如下所示：

```
var serviceBase = {port: 3000, url: 'azat.co'},
getAccounts = function(){return [1,2,3]};
var accountService = {
    __proto__: serviceBase,
    getAccounts,
```

另外，我们可以调用super防范，以及使用动态key值(valueOf_1_2_3):

```
 toString() {
     return JSON.stringify((super.valueOf()));
    },
    getUrl() {return "http://" + this.url + ':' + this.port},
    [ 'valueOf_' + getAccounts().join('_') ]: getAccounts()
};
console.log(accountService)
```



![img](https://upload-images.jianshu.io/upload_images/8164826-63813b6d8900978f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/375/format/webp)

image.png

ES6对象文本是一个很大的进步对于旧版的对象文本来说。

### 6.箭头函数 ES6

这或许是我最想要的一个特性，我爱 CoffeeScript 就是因为他胖胖的箭头(=&>相对于-&>)，现在ES6中也有了。这些箭头最神奇的地方在于他会让你写正确的代码。比如，this在上下文和函数中的值应当是相同的，它不会变化，通常变化的原因都是因为你创建了闭包。

有了箭头函数在ES6中， 我们就不必用that = this或 self = this 或 _this = this 或.bind(this)。例如，下面的代码用ES5就不是很优雅：

```
var _this = this;
$('.btn').click(function(event){
  _this.sendData();
})
```

在ES6中就不需要用 _this = this：

```
$('.btn').click((event) =>{
  this.sendData();
})
```

不幸的是，ES6委员会决定，以前的function的传递方式也是一个很好的方案，所以它们仍然保留了以前的功能。

下面这是一个另外的例子，我们通过call传递文本给logUpperCase() 函数在ES5中：

```
var logUpperCase = function() {
  var _this = this;

  this.string = this.string.toUpperCase();
  return function () {
    return console.log(_this.string);
  }
}
logUpperCase.call({ string: 'ES6 rocks' })();
```

而在ES6，我们并不需要用_this浪费时间：

```
var logUpperCase = function() {
  this.string = this.string.toUpperCase();
  return () => console.log(this.string);
}
logUpperCase.call({ string: 'ES6 rocks' })();
```

请注意，只要你愿意，在ES6中=>可以混合和匹配老的函数一起使用。当在一行代码中用了箭头函数，它就变成了一个表达式。它将暗地里返回单个语句的结果。如果你超过了一行，将需要明确使用return。

这是用ES5代码创建一个消息数组：

```
var ids = ['5632953c4e345e145fdf2df8','563295464e345e145fdf2df9'];
var messages = ids.map(function (value) {
  return "ID is " + value; // explicit return
});
```

用ES6是这样：

```
var ids = ['5632953c4e345e145fdf2df8','563295464e345e145fdf2df9'];
var messages = ids.map(value => `ID is ${value}`); // implicit return
```

请注意，这里用了字符串模板。
在箭头函数中，对于单个参数，括号()是可选的，但当你超过一个参数的时候你就需要他们。
在ES5代码有明确的返回功能：

```
var ids = ['5632953c4e345e145fdf2df8', '563295464e345e145fdf2df9'];
var messages = ids.map(function (value, index, list) {
  return 'ID of ' + index + ' element is ' + value + ' '; // explicit return
});
```

在ES6中有更加严谨的版本，参数需要被包含在括号里并且它是隐式的返回：

```
var ids = ['5632953c4e345e145fdf2df8','563295464e345e145fdf2df9'];
var messages = ids.map((value, index, list) => `ID of ${index} element is ${value} `); // implicit return
```

### 7. ES6中的Promise

Promises 是一个有争议的话题。因此有许多略微不同的promise 实现语法。Q，bluebird，deferred.js，vow, avow, jquery 一些可以列出名字的。也有人说我们不需要promises，仅仅使用异步，生成器，回调等就够了。但令人高兴的是，在ES6中有标准的Promise实 现。

下面是一个简单的用setTimeout()实现的异步延迟加载函数:

```
setTimeout(function(){
  console.log('Yay!');
}, 1000);
```

在ES6中，我们可以用promise重写:

```
var wait1000 =  new Promise(function(resolve, reject) {
  setTimeout(resolve, 1000);
}).then(function() {
  console.log('Yay!');
});
```

或者用ES6的箭头函数：

```
var wait1000 =  new Promise((resolve, reject)=> {
  setTimeout(resolve, 1000);
}).then(()=> {
  console.log('Yay!');
});
```

到目前为止，代码的行数从三行增加到五行，并没有任何明显的好处。确实，如果我们有更多的嵌套逻辑在setTimeout()回调函数中，我们将发现更多好处：

```
setTimeout(function(){
  console.log('Yay!');
  setTimeout(function(){
    console.log('Wheeyee!');
  }, 1000)
}, 1000);
```

在ES6中我们可以用promises重写：

```
var wait1000 =  ()=> new Promise((resolve, reject)=> {setTimeout(resolve, 1000)});
wait1000()
    .then(function() {
        console.log('Yay!')
        return wait1000()
    })
    .then(function() {
        console.log('Wheeyee!')
    });
```

还是不确信Promises 比普通回调更好？其实我也不确信，我认为一旦你有回调的想法，那么就没有必要额外增加promises的复杂性。

虽然，ES6 有让人崇拜的Promises 。Promises 是一个有利有弊的回调但是确实是一个好的特性，更多详细的信息关于promise:[Introduction to ES6 Promises](https://link.jianshu.com/?t=http%3A%2F%2Fjamesknelson.com%2Fgrokking-es6-promises-the-four-functions-you-need-to-avoid-callback-hell).

### 8.块作用域和构造let和const

在ES6代码中，你可能已经看到那熟悉的身影let。在ES6里let并不是一个花俏的特性，它是更复杂的。Let是一种新的变量申明方式，它允许你把变量作用域控制在块级里面。我们用大括号定义代码块，在ES5中，块级作用域起不了任何作用：

```
function calculateTotalAmount (vip) {
  var amount = 0;
  if (vip) {
    var amount = 1;
  }
  { // more crazy blocks!
    var amount = 100;
    {
      var amount = 1000;
    }
  }  
  return amount;
}
console.log(calculateTotalAmount(true));
```

结果将返回1000，这真是一个bug。在ES6中，我们用let限制块级作用域。而var是限制函数作用域。

```
function calculateTotalAmount (vip) {
  var amount = 0; // probably should also be let, but you can mix var and let
  if (vip) {
    let amount = 1; // first amount is still 0
  } 
  { // more crazy blocks!
    let amount = 100; // first amount is still 0
    {
      let amount = 1000; // first amount is still 0
    }
  }  
  return amount;
}
console.log(calculateTotalAmount(true));
```

这个结果将会是0，因为块作用域中有了let。如果（amount=1）.那么这个表达式将返回1。谈到const，就更加容易了；它就是一个不变量，也是块级作用域就像let一样。下面是一个演示，这里有一堆常量，它们互不影响，因为它们属于不同的块级作用域:

```
function calculateTotalAmount (vip) {
  const amount = 0;  
  if (vip) {
    const amount = 1;
  } 
  { // more crazy blocks!
    const amount = 100 ;
    {
      const amount = 1000;
    }
  }  
  return amount;
}
console.log(calculateTotalAmount(true));
```

从我个人看来，let 和const使这个语言变复杂了。没有它们的话，我们只需考虑一种方式，现在有许多种场景需要考虑。

### 9. ES6中的类

如果你喜欢面向对象编程，那么你会特别喜欢这个特性。他让你编写和继承类时就跟在Facebook上发一个评论这么简单。

在ES5中，因为没有`class`关键字（但它是毫无作用的保留字），类的创建和使用是让人十分痛苦的事情。更惨的是，很多伪类的实现像[pseude-classical](https://link.jianshu.com/?t=http%3A%2F%2Fjavascript.info%2Ftutorial%2Fpseudo-classical-pattern), [classical](https://link.jianshu.com/?t=http%3A%2F%2Fwww.crockford.com%2Fjavascript%2Finheritance.html), [functional](https://link.jianshu.com/?t=http%3A%2F%2Fjavascript.info%2Ftutorial%2Ffactory-constructor-pattern)让人越来越摸不着头脑，为JavaScript的信仰战争火上浇油。

我不会给你展示在ES5中怎么去编写一个类（是啦是啦从对象可以衍生出来其他的类和对象），因为有太多方法去完成。我们直接看ES6的示例，告诉你ES6的类会用`prototype`来实现而不是`function`。现在有一个`baseModel`类，其中我们可以定义构造函数和`getName()`方法。

```
class baseModel {
  constructor(options = {}, data = []) { // class constructor
        this.name = 'Base'
    this.url = 'http://azat.co/api'
        this.data = data
    this.options = options
    }
 
    getName() { // class method
        console.log(`Class name: ${this.name}`)
    }
}
```

注意我们对options 和data使用了默认参数值。此外方法名也不需要加function关键字，而且冒号(：)也不需要了。另外一个大的区别就是你不需要分配属性this。现在设置一个属性的值，只需简单的在构造函数中分配。

AccountModel 从类baseModel 中继承而来:

```
class AccountModel extends baseModel {
    constructor(options, data) {
```

为了调用父级构造函数，可以毫不费力的唤起super()用参数传递：

```
super({private: true}, ['32113123123', '524214691']); //call the parent method with super
       this.name = 'Account Model';
       this.url +='/accounts/';
    }
```

如果你想做些更好玩的，你可以把 accountData 设置成一个属性：

```
 get accountsData() { //calculated attribute getter
    // ... make XHR
        return this.data;
    }
}
```

那么，你如何调用他们呢？它是非常容易的：

```
let accounts = new AccountModel(5);
accounts.getName();
console.log('Data is %s', accounts.accountsData);
```

如果好奇输出结果的话：

```
Class name: Account Model
Data is 32113123123,524214691
```

### 10. ES6中的模块化

众所周知，在ES6以前JavaScript并不支持本地的模块。人们想出了AMD，RequireJS，CommonJS以及其它解决方法。现在ES6中可以用模块import 和export 操作了。

在ES5中，你可以在<script>中直接写可以运行的代码（简称IIFE），或者一些库像AMD。然而在ES6中，你可以用export导入你的类。下面举个例子，在ES5中,module.js有port变量和getAccounts 方法:

```
module.exports = {  port: 3000,  getAccounts: function() {    ...  }}
```

在ES5中，main.js需要依赖require(‘module’) 导入module.js：

```
var service = require('module.js');console.log(service.port); // 3000
```

但在ES6中，我们将用export and import。例如，这是我们用ES6 写的module.js文件库：

```
export var port = 3000;export function getAccounts(url) {  ...}
```

如果用ES6来导入到文件main.js中，我们需用import {name} from ‘my-module’语法，例如：

```
import {port, getAccounts} from 'module';console.log(port); // 3000
```

或者我们可以在main.js中把整个模块导入, 并命名为 service：

```
import * as service from 'module';console.log(service.port); // 3000
```

个人来说，我觉得这样的模块化有些搞不懂。确实，这样会更传神一些 。但是Node.js中的模块不会马上就改过来，浏览器和服务器的代码最好是用同样的标准，所以目前我还是会坚持`CommonJS/Node.js`的方式。

目前来说浏览器对ES6的支持还遥遥无期（本文写作时），所以你需要一些像[jspm](https://link.jianshu.com/?t=http%3A%2F%2Fjspm.io%2F)这样的工具来用ES6的模块。

想要了解更多ES6中的模块化和例子的话，来看[这篇文章](https://link.jianshu.com/?t=http%3A%2F%2Fexploringjs.com%2Fes6%2Fch_modules.html)，不管怎么说，写现代化的JavaScript吧！

### 怎么使用ES6

ES6标准已经敲定，但还未被所有浏览器支持（[Firefox的ES6功能一览](https://link.jianshu.com/?t=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FJavaScript%2FNew_in_JavaScript%2FECMAScript_6_support_in_Mozilla)），如果想马上就用上ES6，需要一个像[Babel](https://link.jianshu.com/?t=https%3A%2F%2Fbabeljs.io%2F)这样的编译器。你可以把他当独立工具用，也可以将他集成到构建系统里，Babel对`Gulp`，`Grunt`和`Webpack`都有对应的[插件](https://link.jianshu.com/?t=http%3A%2F%2Fbabeljs.io%2Fdocs%2Fsetup)。

安装Gulp插件示例：

```
$ npm install --save-dev gulp-babel
```

在gulpfile.js中，定义这么一个任务，将src目录下的app.js文件编译到build目录下：

```
var gulp = require('gulp'),
  babel = require('gulp-babel')
gulp.task('build', function () {
  return gulp.src('src/app.js')
    .pipe(babel())
    .pipe(gulp.dest('build'))
})
```

### Node.js和ES6

对于Node.js，你可以用构建工具或者直接用独立模块babel-core：

```
$ npm install --save-dev babel-core
```

然后在Node.js中调用这个函数：

```
require("babel-core").transform(ES5Code, options);
```

### ES6的一些总结

这里还有许多ES6的其它特性你可能会使用到，排名不分先后：
1、全新的Math, Number, String, Array 和 Object 方法
2、二进制和八进制数据类型
3、默认参数不定参数扩展运算符
4、Symbols符号
5、tail调用
6、Generators (生成器)
7、New data structures like Map and Set(新的数据构造对像MAP和set)

和有些人吃了一片薯片就一发不可收拾的人一样（再来一片嘛就一片），对于那些停不下来想要知道关于更多ES6相关信息的成绩优秀的同学，我准备了扩展阅读：

1. [ES6速查手册](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Fazat-co%2Fcheatsheets%2Ftree%2Fmaster%2Fes6)（[PDF](https://link.jianshu.com/?t=https%3A%2F%2Fgum.co%2FLDwVU%2Fgit-1CC81D40)）
2. [Nicholas C. Zakas所著的理解ECMAScript6](https://link.jianshu.com/?t=https%3A%2F%2Fleanpub.com%2Funderstandinges6)
3. [Dr. Axel Rauschmayer所著的探索ECMAScript6](https://link.jianshu.com/?t=https%3A%2F%2Fleanpub.com%2Fexploring-es6)

