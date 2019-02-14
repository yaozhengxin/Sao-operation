## 一、变量类型和计算
### JS中使用`typeof`能得到哪些类型
  * `值类型`：变量本身就是含有赋予给它的数值的，它的变量本身及保存的数据都存储在栈的内存块当中
  * `引用类型`：引用类型当然是分配到堆上的对象或者数据变量，根据官方一点的解释就是引用类型的变量只包括对其所表示的数据引用
  ```javascript
  typeof abc      //"undefined"
  typeof NaN      //"number"
  typeof true     //"boolean"
  typeof {}       //"object"
  typeof []       //"object"
  typeof null     //"object"
  typeof console.log //"function"
  ```
  `typeof只能区分值类型，对引用类型无能为力，只能区分函数function`

  NaN表示特殊的非数字值，null是空指针，并没有指向任何一个地址

  `typeof`能区分的五种基本类型：`string`、`boolean`、`number`、`undefined`、`object`和函数`function`

* 变量计算
  * 可能发生强制类型转换的情况：
  ```javascript
  100  ==  '100'  //true
  0    ==  ""     //true
  null ==  undefined  //true
  ```
  * `在js逻辑运算中，0、NaN、""、null、false、undefined都会判为false，其他都为true`
  ```javascript
  var add_level = (add_step == 5 && 1)||(add_step == 10 && 2)||(add_step == 12 && 4)||(add_step==15 && 5 )|| 0;

  var add_level = {'5':1,'10':2,'15':5,'12':4}[add_step]||0; //更精简
  ```
### 何时使用 `===` 何时使用 `==`
  ```javascript
  if(obj.a == null){
      //这里相当于obj.a === null || obj.a === undefined,简写
      //这是jquery源码中推荐的写法，除此之外其他全用 ===
      //主要是用于判断这个属性是否存在
  }
  function (a,b){
      if(a == null){
          //判断函数参数是否存在
      }
  }
  //这种写法不能用
  if(xxx == null){
      //可能会报错，这个参数未定义  not defined
  }
  ```
### JS中有哪些内置函数-数据封装类对象
  * Object
  * Array
  * Boolean
  * Number
  * String
  * Function
  * Date
  * RegExp
  * Error

## 二、原型和原型链
  ### 构造函数
  ```javascript
  // 构造函数要以大写字母开头
  function Foo(name,age){
    this.name = name
    this.age = age
    this.class = 'class - 1'
    // return this // 默认有这一行
  }
  
  var f  = new Foo('zhangsan',20)
  var f1 = new Foo('lisi',22)  //创建多个对象
  ```
  #### new一个对象的过程
  * 1、把参数穿进去
  * 2、new执行的时候，this会先变成一个空对象，
  * 3、然后会把this.xxx赋值，默认会把 this
  ```javascript
  return function Foo(name,age){ 
    this.name = name;
    this.age = age;
  }    
  ```
  #### 构造函数-扩展
  `var a = {}`         其实是var a = new Object()的语法糖   
  `var a = []`              其实是var a = new Array()的语法糖  
  `function foo(){...}`       其实是var Foo = new Function(...)的语法糖   
  `使用instanceof+构造函数 判断一个函数是否是另一变量的构造函数`

  #### 构造函数-扩展
  * 所有的引用类型(数组、函数、对象)都具有对象特性，即可自由扩展属性(`null除外`)
  * 所有的引用类型(数组、对象、函数)有一个`__proto__` 属性，属性值是一个普通的对象
  * 所有的函数都有一个`prototype`属性，属性值也是一个普通的对象
  * 所有的引用类型(数组、对象、函数),`__proto__`属性指向它的构造函数的`prototype`属性
  * 当试图得到一个对象的某个属性时，如果这个对象本身没有这个属性，那么回去他的`__proto__`(即它的构造函数的`prototype`中寻找)

  ```javascript
  function Foo(name,age){
	  this.name = name
  }
  Foo.prototype.alertName = function(){
    alert(this.name)
  }
  //创建示例
  var f = new Foo('zhangsan');
  f.printName = function(){
    console.log(this.name);
  }
  f.printName();
  f.alertName();
  ```

  * 如何准确判断一个变量是否是数组类型
  ```javascript
  Object.prototype.toString.call('') ;   // [object String]
  Object.prototype.toString.call(1) ;    // [object Number]
  Object.prototype.toString.call(true) ; // [object Boolean]
  Object.prototype.toString.call(undefined) ; // [object Undefined]
  Object.prototype.toString.call(null) ; // [object Null]
  Object.prototype.toString.call(new Function()) ; // [object Function]
  Object.prototype.toString.call(new Date()) ; // [object Date]
  Object.prototype.toString.call([]) ; // [object Array]
  Object.prototype.toString.call(new RegExp()) ; // [object RegExp]
  Object.prototype.toString.call(new Error()) ; // [object Error]
  Object.prototype.toString.call(document) ; // [object HTMLDocument]
  Object.prototype.toString.call(window) ; //[object global] window是全局对象global的引用
  ```
  ```javascript
  function Animal(){ 
    this.eat = function(){ 
        console.log('Animal eat'); 
      }
  }
  function Dog(){ 
      this.bark = function(){ 
          console.log("dog bark"); 
      }
  }
  Dog.prototype = new Animal(); 
  var hashiqi = new Dog();
  ```

## 三、作用域和闭包
  ### 执行上下文
  ```javascript
  console.log(a)    //undefined
  var a = 100
  fn('zhangsan')    //'zhangsan' 20
  function fn(name){
    age = 20
    console.log(name,age)
    var age
  }
  ```
  * 函数声明会被提出来，函数表达式会和变量一样，以undefined占位
  * 在函数执行之前,变量定义、函数声明、this、arguments都要先提取出来

  ### 函数声明和函数表达式的区别
  ```javascript
  fn()                    //不会报错
  function fn(){
      //声明
  }
  ```
  ```javascript
  fn() //Uncaught TypeError:fn1 is not a function 
  var a = function(){
      //表达式
  }
  ```
  ### 说明this几种不同的使用场景
  * 作为构造函数执行
  * 作为对象属性执行
  * 作为普通函数执行(window)
  * `call`、`apply`、`bind`
  ```javascript
  var a = {
	name : 'A',
	fn:function(){
		console.log(this.name)
    }
  }
  a.fn()                  //this  === a
  a.fn.call({name:'B'})   //this  === {name:'B'}
  var fn1 = a.fn
  fn1()                   //this === window
  ```
  ### 闭包
  * 函数作为返回值
  ```javascript
  function F1(){
    var a = 100
    //返回一个函数(函数作为返回值)
    return function(){
      console.log(a)
    }
  }
  var f1 = F1()
  var a = 200
  f1()
  ```
  * 函数作为参数传递
  ```javascript
  function F1(){
    var a = 100
    return function(){
      console.log(a)   //自由变量，父作用域寻找
    }
  }
  var f1 = F1()
  function F2(fn){
    var a = 200
    fn()
  }
  F2(f1)
  ```
  * 实际开发中闭包的应用
  ```javascript
  //闭包实际应用中主要用于封装变量，收敛权限
  function isFirstLoad(){ 
    var _list = []; 
    return  function(id){
              if(_list.indexOf(id) >= 0){
                  return false; 
              }else{ 
                  _list.push(id);
                  return true;
              } 
            }
  }
  var firstLoad = isFirstLoad();
  firstLoad(10);
  ```
## 四、异步和单线程
  * 什么是异步
    * 是否阻塞程序的运行
  * 何时需要异步
    * 在可能发生等待的情况
      * 定时任务：setTimeout,setTimeInterval
      * 网络请求：ajax请求，动态<img>加载
      * 事件绑定

## 五、常见对象
  ### Date
  ```javascript
  Date.now() //获取当前时间毫秒数
  var dt = new Date()
  dt.getTime()  		//获取毫秒数
  dt.getFullYear()  	//年
  dt.getMonth()		//月(0-11)
  dt.getDate()		//日(0-31)
  dt.getHours()		//小时(0-23)
  dt.getMinutes()		//分钟(0-59)
  dt.getSeconds()		//秒(0-59)
  ```
  ### Math
  `Math.random()`：可以用来清除缓存
  ### Array
  * `forEach` 遍历所有数据
  * `every` 判断所有元素是否都符合条件
  * `some` 判断是否有至少一个元素符合条件
  * `sort` 排序
  * `map` 对元素重新组装，生成新数组>- 过滤符合条件的元素
    #### array.forEach
    ```javascript
    arr.forEach( function(item,index){ 
        console.log(index,item); 
    });
    ```
    #### array.every
    ```javascript
    var arr = [1,2,3,4,5];
    var result = arr.every(function(index,item){
    //用来判断所有的数组元素，都满足一个条件 
        if(item<6) 
            return true
    });
    console.log(result);
    ```
    #### array.some
    ```javascript
    var result = arr.some(function(index,item){ 
        if(item<5) 
            return true;
    }); 
    console.log(result);
    ```
    #### arry.sort
    ```javascript
    var arr2 = arr.sort(function(a,b){ 
        //从小到大排序 return a-b; 
        //从大到小排序 
        //return b-a;
    })
    ```
    #### array.map
    ```javascript
    arr.map(function(item,index){
      return '<br>'+index+':'+item+'<br>'
    })
    ```
    #### array.filter
    ```javascript
    var arr2 = arr.filter(function (item,index){ 
        //通过某一个条件过滤数组 
        if(item >= 2) 
            return true
    })
    ```
### 对象API
    ```javascript
    var obj = {
      x:100,
      y:200,
      z:300
    }

    var key

    for(key in obj){
      if(obj.hasOwnProperty(key)){
        console.log(key,obj[key])
      }
    }
    ```

## 六、JS-Web-API
  ### DOM操作
  * DOM的本质 Document、Object、Model 浏览器把拿到的html代码，结构化一个浏览器能够识别并且js可操作的一个模型而已
  * DOM的节点操作
    * 获取DOM节点
    * Attribute 和 properity
      * `attribute`：是HTML标签上的某个属性，如id、class、value等以及自定义属性，它的值只能是字符串，关于这个属性一共有三个相关的方法，setAttribute、getAttribute、removeAttribute；
      注意：在使用setAttribute的时候，该函数一定接收两个参数，setAttribute（attributeName,value）,无论value的值是什么类型都会编译为字符串类型。在html标签中添加属性，本质上是跟在标签里面写属性时一样的，所以属性值最终都会编译为字符串类型。
      * `property`：是js获取的DOM对象上的属性值，比如a，你可以将它看作为一个基本的js对象。这个节点包括很多property，比如value，className以及一些方法onclik等方法。
      一个js对象有很多property，该集合名字为properties，properties里面有其他property以及attributies，attributies里面有很多attribute。
      而常用的attribute，id、class、name等一般都会作为property附加到js对象上，可以和property一样取值、赋值

  * DOM是那种基本的数据结构？
    * 树  
  * DOM操作的常用API有哪些？
    * 获取DOM节点，以及节点的property和Attribute
    * 获取父节点，获取子节点
    * 新增节点，删除节点
  * DOM节点的attr和property有何区别
    * property只是一个JS对象的属性的修改
    * Attribute是对html标签属性的修改
### BOM操作
  ```javascript
  //navigator
  var ua = navigator.userAgent
  var isChrome = ua.indexOf('chrome')
  console.log(isChrome)

  //screen
  console.log(screen.width)
  console.log(screen.height)

  //location
  console.log(location.href)
  console.log(location.protocol)
  console.log(location.pathname)
  console.log(location.search)    //?之后的参数
  console.log(location.hash)      //#号之后

  //history
  history.back()          
  history.forward()
  ```

## 七、事件
### 通用事件绑定
```javascript
var btn = document.getElementById('btn1')
btn.addEventListener('click', function(event) {
    console.log('clicked')
})

function bindEvent(elem, type, fn) {
    elem.addEventListener(type, fn)
}

var a = document.getElementById('link1')
bindEvent(a, 'click', function(e) {
    e.preventDefault(); //阻止默认行为
    alert('clicked')
})
```
### 事件冒泡
* `e.stopPropatation()` //取消冒泡
### 事件代理
```javascript
<div id="div1">
    <a href = "#">a1</a>
    <a href = "#">a2</a>
    <a href = "#">a3</a>
    <a href = "#">a4</a>
    <!--会随时新增更多 a 标签-->
</div>

function bindEvent(elem, type, selector, fn) {
    if (fn == null) {
        fn = selector
        selector = null
    }
    elem.addEventListener(type, function(e) {
        var target
        if (selector) {
            target = e.target
            if (target.matches(selector)) {
                fn.call(target, e)
            }
        } else {
            fn(e)
        }
    })
}

//使用代理
var div1 = document.getElementById('div1')
bindEvent(div1, 'click', 'a', function(e) {
    console.log(this.innerHTML)
})

//不使用代理
var a = document.getElementById('a1')
bindEvent(div1, 'click', function(e) {
    console.log(a.innerHTML)
})
```
### 手动写一个ajax，不依赖第三方库
```javascript
var xhr = new XMLHttpRequest()
xhr.open("GET","/api",false)
xhr.onreadystatechange = function(){
    //这里的函数异步执行，可参考之前JS基础中的异步模块
    if(xhr.readyState == 4){
        if(xhr.status == 200){
            alert(xhr.responseText)
        }
    }
}
xhr.send(null)

// 状态码说明
// 0 - (未初始化)     还没调用send()方法
// 1 - (载入)        已调send() 方法，正在发送请求
// 2 - (载入完成)     send()方法执行完成，已经接收到全部相应内容
// 3 - (交互)        正在解析响应内容
// 4 - (完成)        响应内容解析完成，可以在客户端调用了

// status说明
// 2XX - 表示成功处理请求。如200
// 3XX - 需要重定向，浏览器直接跳转
// 4XX - 客户端请求错误，如404
// 5XX - 服务器端错误
```
### 跨域
* 浏览器有同源策略，不允许ajax访问其他域的接口
* 跨域条件：协议、域名、端口，有一个不同就算跨域
* 可以跨域的三个标签
  * `<img src=''>` 用于打点统计，统计网站可能是其他域(而且没有任何兼容性问题)
  * `<script src=''>,<link href=''>` 可以使用CDN，CDN也是其他域
  * `<link href=''>` 可以用于JSONP
### 跨越注意的问题
  * 所有的跨域请求都必须经过信息提供方允许
  * 如果未经允许即可获取，那是浏览器同源策略出现漏洞
### JSONP实现原理
  * 加载 coding.m.imooc/classindex.…
  * 不一定服务器端真正有一个classindex.html文件
  * 服务器可以根据请求，动态生成一个文件，返回
  * 通理于<script src="http://coding.m.imooc.com/api.js">
  ```javascript
  //例如你的网站要跨域访问慕课网的一个接口
  //慕课给你一个地址 http://coding.m.imooc.com/api.js
  //返回内容格式 callback({x:100,y:200})
  <script>
  window.callback = function(data){
      //这是我们跨域得到信息
      console.log(data)
  }
  </script>
  <script src="http://coding.m.imooc.com/api.js"></script>
  ```
### cookie、sessionStorage、localStorage的区别
  * 容量
  * 是否会携带到ajax中
  * PI易用性
  * ios safari隐藏模式下，localStorage.getItem会报错，建议统一用try-catch封装