# 继承

在JavaScript中，继承是获取对象已有的方法和属性的一种方式。 
先抛开概念，举个例子，下面有一个函数对象：
```javascript
function superType(name,age){
    this.name = name;
    this.age = age;
    this.color = ['blue','green','black'];  //引用类型属性
};

superType.prototype.sayname = function(){
    console.log(this.name);
};
superType.prototype.saycolor = function(){
    console.log(this.color);
};

```
然后，我打算写一个新函数：
```javascript
function sub(){};
```
我希望函数`sub`能够拥有函数`superType`的所有**属性**和**方法**。我们就可以说新函数`sub`继承了已有函数`superType`的属性和方法。  
同时，对于新函数`sub`，还有两个要求：
+ 每个实例都有自己的属性
+ 所有实例共享方法

知道这两点非常重要，因为后面所有的方法都是围绕着这两个要求展开的，一点要抓着这两个目的去学习。下面我们一步步来实现这两个目标。

## 原型链
首先想到的是通过原型链实现继承
```javascript
function sub(){};
sub.prototype = new superType('jerry',27);  //关键一步
sub.prototype.constructor = sub;

var student3 = new sub();
student3.sayname();      //jerry
```
实现原型链的关键在于：使子类的原型是超类的一个实例。因此，原型链实现继承的特点是：**所有子类实例共享同一个父类实例**。  
但是，这种方式会导致两个问题：
+ 新建子类实例时没办法向父类构造函数传递参数
+ 拥有引用类型值的原型属性会被所有子类实例共享，可以被子类实例引用并修改，进而影响所有的子类实例

为了避开这两个问题，需要换一种实现继承的方法。
## 借用构造函数技术
*函数只不过是在特定环境中执行代码的对象  ————《JavaScript高级程序设计》*  
这句话非常精辟，因此这种技术的思路很好理解，在子类构造函数的内部直接调用父类构造函数
```javascript
function sub2(name,age,sex){
    superType.call(this,name,age);    //关键一步
    this.sex = sex; 
};

var student4 = new sub2('jerry',29,'man');
console.log(student4);
```
这种方式的优点在于：每个子类实例都有自己私有的属性和方法，不会相互影响，这就解决了引用类型值原型属性被共享的问题。同时，还可以向父类构造函数传递参数。缺点也很明显，每创建一个实例就新建一套属性和方法，函数不能复用，浪费内存。

这个时候，再看一下最初的要求，我们需要的是：
+ 私有的属性
+ 共享的方法

我们发现，借用构造函数技术可以实现属性私有，利用原型存放方法函数可以实现方法共享，不如把他们组合一下。于是，就有了组合式继承。
## 组合式继承
```javascript
function sub3(name,age,sex){
    //继承属性
    superType.call(this,name,age);   //第二次调用superType
    this.sex = sex;
};
//继承方法
sub3.prototype = new superType();    //第一次调用superType
sub3.prototype.constructor = sub3;
sub3.prototype.saysex = function(){
    console.log(this.sex);
};

var student5 = new sub3('jerry',30,'man');
student5.saysex();    //man
```
思路也很简单，通过借用构造函数继承属性，通过子类原型来继承父类方法。到这一步，两个目标都实现了。但是，程序还有没有优化的空间呢？  
再看一下，在执行代码的过程中，程序总共需要调用两次父类构造函数`superType`，同时会在子类原型中创建一套无用的属性，这会降低程序的性能。  
为了解决这个问题，我打算这么入手，让`sub3.prototype`指向一个空对象，空对象的原型指向父类的原型。围绕这个思路，可以把流程封装成下面两个函数：
```javascript
//创建空对象，其原型指向父类的原型
function creat(obj){
    function f(){};
    f.prototype = obj;
    return new f();
};

//搭建原型链
function inherit(sub,supers){
    var prototype = creat(supers.prototype); //创建对象
    prototype.constructor = sub;             //增强对象
    sub.prototype = prototype;               //指定对象为原型
};
```
然后更新一下组合式继承的程序
```javascript
function sub3(name,age,sex){
    superType.call(this,name,age);
    this.sex = sex;
};
inherit(sub3,superType);                    //被更新的位置
sub3.prototype.saysex = function(){
    console.log(this.sex);
};

var student5 = new sub3('jerry',30,'man');
student5.sayname();   //jerry
student5.saysex();    //man
```
现在，新程序与旧程序相比，优势在于有更高的执行效率，因为他只调用了一次父类构造函数。在《JavaScript高级程序设计》中，这种继承方式称为寄生组合式继承，它是目前引用类型最理想的继承范式。  
最后，要记录我的一次错误。事实上，最开始我解决调用两次构造函数问题的方案是：
```javascript
sub3.prototype = superType.prototype;
```
这么做的问题在于，当我想增强子类`sub3`的原型的时候，会对父类的原型造成破坏，特别是设置`constructor`属性时会重写父类原型的`constructor`属性。




