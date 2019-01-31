* bad
```javascript
```
* good
```javascript
```
## DEMO1
### 这种一看就明白吧
* bad
```javascript
    function Demo1(age) {
      if (age > 20) {
        return true;
      } else {
        return false;
      }
    }
    function isWeiXin() {
      var ua = window.navigator.userAgent.toLowerCase();
      if (ua.match(/MicroMessenger/i) == 'micromessenger' || ua.match(/_SQ_/i) == '_sq_') {
        return true;
      } else {
        return false;
      }
    }
```
* good
```javascript
    function Demo1(age) {
        return age > 20
    }
    function isWeiXin() {
      var ua = window.navigator.userAgent.toLowerCase();
      return ua.match(/MicroMessenger/i) == 'micromessenger' || ua.match(/_SQ_/i) == '_sq_'
    }
```


## DEMO2
### 循环
* bad
```javascript
    for (var i = 0; i < arr.length; i++) {
      //do something...
    }
```
* good--避免重复计算arr的长度
```javascript
    for (var i = 0,len=arr.length; i < len; i++) {
      //do something...
    }
```
* bad
```javascript
    var arr = ["a", "b", "c"];
    arr.forEach(function (elem, index) {
      console.log("index = " + index + ", elem = " + elem);
    });
    // Output:
    // index = 0, elem = a
    // index = 1, elem = b
    // index = 2, elem = c
```
* good--for…of的循环可以避免我们开拓内存空间，增加代码运行效率，所以建议大家在以后的工作中使用for…of循环
```javascript
    const arr = ["a", "b", "c"];
    for (const [index, elem] of arr.entries()) {
      console.log(`index = ${index}, elem = ${elem}`);
    }
```
* bad 
```javascript
    var result = []
    var a = [1, 2, 3, 4, 5, 6]
    for (i = 1; i <= 10; i++) {
      if (a[i] > 4) {
        result.push(a[i])
      }
    }
```
* good--对于for循环优先使用命令式编程
```javascript
    var a = [1, 2, 3, 4, 5, 6]
    const result = a.filter(item => item > 4);
```


## DEMO3
### 巧用||和&&布尔运算符
* bad
```javascript
    function eventHandler(e) {
      if (!e) e = window.event;
    }
```
* good
```javascript
    function eventHandler(e) {
      e = e || window.event;
    }
```
* bad
```javascript
    if (age > 20) {
      console.log("年龄大于20");
    }
```
* good
```javascript
    (age > 20) && console.log("年龄大于20");    
```


## DEMO4
### 设默认值
* bad
```javascript
    var link = function (height, color, url) {
      var height = height || 50;
      var color = color || 'red';
      var url = url || 'http://azat.co';
    }
```
* good
```javascript
    var link = function (height = 50, color = 'red', url = 'http://azat.co') {
    }   
```
* bad
```javascript
    function test(fruit) {
      // 如果有值，则打印出来
      if (fruit && fruit.name) {
        console.log(fruit.name);
      } else {
        console.log('unknown');
      }
    }
```
* good
```javascript
    var link = function (height = 50, color = 'red', url = 'http://azat.co') {
    }   
```
* bad
```javascript
    function test(fruit) {
      // 如果有值，则打印出来
      if (fruit && fruit.name) {
        console.log(fruit.name);
      } else {
        console.log('unknown');
      }
    }
```
* good
```javascript
    // 解构 —— 只得到 name 属性
    // 默认参数为空对象 {}
    function test({ name } = {}) {
      console.log(name || 'unknown');
    }
```


## DEMO5
### 使用解构赋值
* bad
```javascript
    var user = { name: 'dys', age: 1 };
    var name = user.name;
    var age = user.age;
```
* good
```javascript
    // 解构 —— 只得到 name 属性
    // 默认参数为空对象 {}
    var fullName = ['jackie', 'willen'];
    var firstName = fullName[0];
    var lastName = fullName[1];
```
* bad
```javascript
    var a = [1,2,3];
    var b = [4,5,6];
    var c = a.concat(b);
    //c=[1,2,3,4,5,6];
    const d = {"a":1}
    const e = {"b":2}
    const f = Object.assign({},d,e)
```
* good-引入babel-pollly
```javascript
    const a = [1,2,3];
    const b = [4,5,6]
    const c = [...a,...b]
    //c=[1,2,3,4,5,6];
    const d = {"a":1}
    const e = {"b":2}
    const f = {...a,...b}
    //c={"a":1,"b":2};
```

## DEMO5
### 重复函数
* bad
```javascript
    var paging = function (currPage) {
      if (currPage <= 0) {
        currPage = 0;
        jump(currPage);
      } else if (currPage >= totalPage) {
        currPage = totalPage;
        jump(currPage);
      } else {
        jump(currPage);
      }
    }
```
* good-重构后，把重复函数独立出来
```javascript
    var paging = function (currPage) {
        if (currPage <= 0) 
            currPage = 0
        else if (currPage >= totalPage) 
            currPage = totalPage;
        // currPage <= 0 && currPage = 0
        // currPage >= totalPage && currPage = totalPage
        jump(currPage);
    };
```

## DEMO6
### 处理多重单一条件
* bad
```javascript
    if (color) {
      if (color === 'black') {
        printBlackBackground();
      } else if (color === 'red') {
        printRedBackground();
      } else if (color === 'blue') {
        printBlueBackground();
      } else if (color === 'green') {
        printGreenBackground();
      } else {
        printYellowBackground();
      }
    }
```
* good
```javascript
    switch (color) {
      case 'black':
        printBlackBackground();
        break;
      case 'red':
        printRedBackground();
        break;
      case 'blue':
        printBlueBackground();
        break;
      case 'green':
        printGreenBackground();
        break;
      default:
        printYellowBackground();
    }

    //多重非单一条件
    //goodgood
    switch (true) {
      case (typeof color === 'string' && color === 'black'):
        printBlackBackground();
        break;
      case (typeof color === 'string' && color === 'red'):
        printRedBackground();
        break;
      case (typeof color === 'string' && color === 'blue'):
        printBlueBackground();
        break;
      case (typeof color === 'string' && color === 'green'):
        printGreenBackground();
        break;
      case (typeof color === 'string' && color === 'yellow'):
        printYellowBackground();
        break;
    }

    //解耦合
    //goodgoodgood
    let handler = {
      black: () => { },
      red: () => { },
      blue: () => { },
      default: () => { }
    }
    handler['black']() || handler['default']()
    //or  使用 Map 
    const fruitColor = new Map()
      .set('red', () => { })
      .set('yellow',() => { })
      .set('purple', () => { })
      .set('default', () => { });

    function test(color) {
      return fruitColor.get(color) || fruitColor.get("default");
    }
```


## DEMO7
### 使用 Array.includes 来处理多重条件
* bad
```javascript
    function test(img) {
      const imgType = 'jpg'
      if (imgType === 'jpg' || imgType === 'png' || imgType === 'gif') {
        console.log('hello image')
      }
    }
```
* good
```javascript
    function test(fruit) {
      // 把条件提取到数组中
      const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];
      if (redFruits.includes(fruit)) {
        console.log('red');
      }
    }


    //goodgood少写嵌套，尽早返回
    function test(fruit, quantity) {
      const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];
      // 条件 1：fruit 必须有值
      if (fruit) {
        // 条件 2：必须为红色
        if (redFruits.includes(fruit)) {
          console.log('red');
          // 条件 3：必须是大量存在
          if (quantity > 10) {
            console.log('big quantity');
          }
        }
      } else {
        throw new Error('No fruit!');
      }
    }

    //1个 if/else 语句来筛选无效的条件
    //3层 if 语句嵌套（条件 1，2 & 3）
    //goodgoodgood
    function test(fruit, quantity) {
      const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];
      // 条件 1：尽早抛出错误
      if (!fruit) throw new Error('No fruit!');
      // 条件2：必须为红色
      if (redFruits.includes(fruit)) {
        console.log('red');
        // 条件 3：必须是大量存在
        if (quantity > 10) {
          console.log('big quantity');
        }
      }
    }

    // 当发现无效条件时尽早返回
    // goodgoodgoodgood 
    function test(fruit, quantity) {
      const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];

      if (!fruit) throw new Error('No fruit!'); // 条件 1：尽早抛出错误
      if (!redFruits.includes(fruit)) return; // 条件 2：当 fruit 不是红色的时候，直接返回
      console.log('red');
      // 条件 3：必须是大量存在
      if (quantity > 10) {
        console.log('big quantity');
      }
    }
```

## DEMO7
### 巧用位运算提升性能
* bad
```javascript
    var someText = 'javascript rules';
    if (someText.indexOf('javascript') !== -1) {
    }
    // or
    if (someText.indexOf('javascript') >= 0) {
    }

```
* good
```javascript
    var someText = 'javascript rules';
    !!~someText.indexOf('javascript');
    // javascript rules contains "javascript" - true
    !~someText.indexOf('javascript');
    // javascript rules NOT contains "javascript" - false

    //normal !!
    !!"" // false
    !!0 // false
    !!null // false
    !!undefined // false
    !!NaN // false

    !!"hello" // true
    !!1 // true
    !!{} // true
    !![] // true
```

## DEMO8
### 参数对象化
* bad
```javascript
    function regist(userName, userPwd, userEmail, userPhone) {
      //do something...
    }
```
* good
```javascript
    function regist(user) {
      //do something
    }
```

## DEMO9
### Array.every 和 Array.some 来处理全部 / 部分满足条件
* bad
```javascript
    const fruits = [
      { name: 'apple', color: 'red' },
      { name: 'banana', color: 'yellow' },
      { name: 'grape', color: 'purple' }
    ];
    function test() {
      let isAllRed = true;
      // 条件：所有的水果都必须是红色
      for (let f of fruits) {
        if (!isAllRed) break;
        isAllRed = (f.color == 'red');
      }
      console.log(isAllRed); // false
    }
```
* good
```javascript
    const fruits = [
      { name: 'apple', color: 'red' },
      { name: 'banana', color: 'yellow' },
      { name: 'grape', color: 'purple' }
    ];

    function test() {
      // 条件：（简短形式）所有的水果都必须是红色
      const isAllRed = fruits.every(f => f.color == 'red');
      console.log(isAllRed); // false
    }

    function test() {
      // 条件：至少一个水果是红色的
      const isAnyRed = fruits.some(f => f.color == 'red');
      console.log(isAnyRed); // true
    }
```

## DEMO10
### 杂记
* bad  -- b污染了全局
```javascript
    function test() {
      var a = b = 1;
      console.log(a);
    }
```
* good
```javascript
    function regist(user) {
      //do something
    }
```
* bad 
```javascript
    var a = "aa";
    var b = "bb";
    var c = "cc";
    var d;
```
* good
```javascript
    var a = "aa",
        b = "bb",
        c = "cc",
        d;
```
* bad 
```javascript
    function Demo(params) {
      if (params.id === 10) {
        if (params.name !== "") {
          if (params.email === "email") {
            //do something...
          }
        }
      }
    }
```
* good
```javascript
    if (user.id === 10 && user.name !== "" && user.email === "email") {
      //do something...
    }
```
* bad 
```javascript
    var arr = [1, 2, 3, 4, 5];
    arr.push(6);
```
* good
```javascript
    //new method 快43%
    var arr = [1, 2, 3, 4, 5];
    arr[arr.length] = 6;
```
