# 环境/作用域/作用域链/函数执行/执行上下文&this

## 全局执行环境

在web浏览器中，全局执行环境被认为是window对象,因此所有全局变量和函数都是作为window对象的属性和方法创建的。代码载入浏览器时，全局执行环境被创建\(当我们关闭网页或者浏览器时全局执行环境才被销毁\)

## 局部执行环境

每个函数都有自己的执行环境，因此局部执行环境为函数对象。当函数被调用时函数的局部环境被创建（函数内的代码执行完毕后，该环境被销毁，同时保存在其中的所有变量和函数定义也随之被销毁\)。

```
var a = 1;
function fn(num1,num2){
    var b = 2;
    function fnInner(){
        var c = 3;
        alert(a + b + c);
    }
    fnInner();//fnInner调用时局部执行环境创建
}
fn(4,5);//fn调用时局部执行环境创建
```

## 作用域链

javascript函数的执行用到了作用域链，这个作用域链是函数定义的时候创建的,当定义一个函数时，它实际保存一个作用域链。当调用这个函数时，它创建一个新的对象来存储它的局部变量，并将这个对象添加至保存的作用域链。作用域链的前端始终都是当前执行的代码所在环境的变量对象。作用域链的末端始终都是全局执行环境的变量对象。作用域链的用途，是保证对执行环境有权访问的所有变量和函数的有权访问

```
var scope = 'global scope';
function checkscope(){
    var scope = 'local scope';
    function f(){return scope};
    return f;
}
checkscope()(); //local scope

// 当调用checkscope时，函数f被定义并作为局部变量绑定到了checkscope作用域链上,
// 因此函数f无论在哪里调用,这种绑定依然有效,因此返回值为local scope
```

```
var num1 = 1;
function Outer(){
    var num2 = 2;
    console.log(num1 + num2);//3
    function Inner(){
        //这里可以访问num3,num2,num1
        var num3 = 3;
        console.log(num1 + num2 + num3);//6
        }
    //这里可以访问num2,Inner(),num1但不能访问num3
    Inner();
}
Outer();
console.log(num1); //1，执行环境

// 这里只能访问num1
// 作用域链(向上搜索):内部环境可以通过作用域链访问所有的外部环境，但外部环境不能访问内部环境中的任何变量和函数。
```

## 函数执行

函数调用进入执行环境时，首先处理arguments，初始化形参\(默认值为undefined\)，然后初始化函数内的函数声明，当代码一步一步执行时再初始化函数内的变量声明\(进入环境未开始执行代码时，值为undefined\)。所以函数内的初始化顺序为变量声明，函数声明，形参。可以从上图图一看出。下面我来举个例子\(整个全局环境也是函数\)。

```
alert(typeof fn);//function，函数声明提前
alert(typeof fn0);//undefined,变量声明提前但未赋值
function fn(){
    //函数表达式
}
var fn0 = function(){
    //函数定义式
}

alert(typeof fn0);//function，此时变量已被赋值
```

## 执行上下文

执行上下文，简称上下文，是ECMA-262标准的一个抽象概念，没有从技术实现的角度定义标准类型和结构，不同于可执行代码概念。每当控制器转到ECMAScript可执行代码的时候，就会产生一个EC

## this

```
var name = 'mt'
var obj = {
    name : 'suyi',
    getName: function(){
        return this.name
    },
    gName: function(){
        function inner(){
            return this.name
        }        
        inner()
    },
    getClosure: function(){
        var y = 0
        return function(){
          console.log(this.name, y++)
        }
    }    
}

var func = obj.getName
var clus = obj.getClosure()


obj.getName() // 'suyi'

(obj.getName=obj.getName)() // 'mt'

func() // 'mt'

(obj.getName=obj.getName())() // Error, not function

clus() // 'mt', 0

(obj.getClosure=obj.getClosure())()   // 'mt', 0
((obj.getClosure=obj.getClosure)())() // 'mt', 0
```



