# 私有变量
在使用JavaScript的过程中，有时候我会希望，在对象的内部存在一种变量，它只能通过对象的属性访问，而不能在外部被直接访问。但是，JavaScript没有私有成员的概念，一个JavaScript对象的所有属性都可以在外部被直接访问，它的所有对象属性都是公有的。怎么办？万幸的是，JavaScript有**私有变量**的概念，对于一个作用域而言，在它内部声明的所有变量都是它的私有变量。比如：
```javascript
function student(){
    var name = 'jerry';
}
```
函数`student`内有一个变量`name`，在函数外部不能直接访问它。要在函数外部访问它，必须在函数内部创建闭包。利用闭包，我们可以创建用于访问私有变量的**特权方法**，实现我们最初的想法。首先想到的是在构造函数中定义特权方法
```javascript
function student(){
    //私有变量
    var name = 'jerry';
    //特权方法
    this.showname = function(){
        console.log(name);
    };
}
var student1 = new student();
student1.showname();   //jerry
```
在调用构造函数的时候，我们为对象定义了一个特权方法`showname`，同时它也是一个闭包，因此他可以访问对象的私有属性。但是，使用构造函数会有一个天生的特点，它会为每一个实例创建一组方法，不利于函数复用。为了避免这个问题，我们需要引入原型，把特权方法定义在原型中。
## 静态私有变量
要使用函数原型，需要先构建一个私有作用域
```javascript
(function(){
    //私有方法
    var name = 'tom';   
    
    //构造函数
    students = function(){};    //定义在全局作用域

    //定义在原型中的特权方法 
    students.prototype.showname = function(){
        console.log(name);
    };
})();

var student2 = new students();
student2.showname();    //tom
```
函数`students`的所有实例都可以通过原型中的特权方法访问私有变量`name`，`name`是静态的，被所有实例共享。在我看来，静态私有属性的实现是使用私有作用域引入函数原型的副产物，主要的目的还是为了实现函数复用。
## 模块模式



