# this关键字

## 全局代码中的this

* 在node环境中为global
* 在浏览器中为windows

## 函数代码

* 不是静态绑定到一个函数，即执行期间值不变
* 调用函数的方式决定了this值

函数调用和非引用类型

```
var foo = {
  bar: function () {
    alert(this);
  }
};
 
foo.bar();   // Reference, OK => foo
(foo.bar)(); // Reference, OK => foo
 
(foo.bar = foo.bar)(); // global?
(false || foo.bar)();  // global?
(foo.bar, foo.bar)();  // global?
```

引用类型和this为null

```
function foo() {
  function bar() {
    alert(this);  // global
  }
  bar();          // the same as AO.bar()
}
```

构造器调用函数中的this

```
function A() {
  alert(this); // "a"对象下创建一个新属性
  this.x = 10;
}
 
var a = new A();
alert(a.x); // 10
```

函数调用中手动设置this

```
var b = 10;
 
function a(c) {
  alert(this.b);
  alert(c);
}
 
a(20); // this === global, this.b == 10, c == 20
 
a.call({b: 20}, 30); // this === {b: 20}, this.b == 20, c == 30
a.apply({b: 30}, [40]) // this === {b: 30}, this.b == 30, c == 40
```

其他

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

http://www.cnblogs.com/TomXu/archive/2012/01/17/2310479.html

