# 私有变量
在C++中，有一个私有成员的概念，私有成员只能通过设定接口函数来进行访问。JavaScript没有私有成员，但是有私有变量的概念。
我们知道，一个JavaScript对象的所有属性都可以在外部被直接访问，因此，它的所有对象属性都是公有的。但是在日常的使用中，我会希望，在对象的内部存在一种变量，它只能通过对象的属性访问，而不能在外部被直接访问。这个时候就要使用私有变量。要学习私有变量首先要了解一下闭包。
## 闭包
先看一个例子
```javascript
function init(){
    var name = 'tom';
    return function(){
        return name;
    };
};

var showname = init();
console.log(showname());
```
被函数`init`返回的匿名函数就是一个闭包，《高级程序设计》中指出，**闭包是指有权访问另一个函数作用域中的变量的函数**。要理解好闭包，就要对JavaScript的
作用域链和垃圾回收机制有一定了解。同时，要清楚闭包的目的。  
通过作用域链的模型可以知道，从内层作用域可以访问外层作用域的变量，但是从外层作用域却不能访问内层作用域的变量。背后的原理是：当一个函数执行完，它的作用域就会被销毁，它内部的变量会被回收，外层作用域根本来不及访问这些变量。
通俗来讲，从外面看，函数就是一个个黑屋子。而闭包的目的，就是为了在这些黑屋子上开窗户。闭包通过返回的匿名函数保持对内部变量的引用，避免变量被垃圾收集机制回收。这就提供了一种从外层作用域访问内层作用域的方法。
# 块级作用域