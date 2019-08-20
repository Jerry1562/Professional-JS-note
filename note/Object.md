# JS的对象 #
本文主要记述JS对象常用操作方法
## 对象属性类型 ##
对象的属性有两种类型：**数据属性**、**访问器属性**。

|   数据属性   |  默认值   |  访问器属性  |  默认值   |
| :----------: | :-------: | :----------: | :-------: |
| configurable |   true    | configurable |   true    |
|  enumerable  |   true    |  enumerable  |   true    |
|   writable   |   true    |     set      | undefined |
|    value     | undefined |     get      | undefined |

## 修改、创建属性 ##
#### `Object.defineProperty()` ####
用于创建一个属性。
```javascript
var Obj1 = {};
//创建一个名为year的属性
Object.defineProperty(Obj1, "year", {
    configurable: true,
    enumerable: true,
    writable: true,
    value: 1990
});
```
#### `Object.defineProperties()` ####
用于一次性创建多个属性。
```javascript
Obj1.defineProperties(Obj1, {
    year:{
        configurable: true,
        enumerable: true,
        writable: true,
        value: 2017
    },
    month:{
        configurable: true,
        enumerable: true,
        writable: true,
        value: 8
    }
});
```
通过上述两种方法创建的属性，如果不指定，`configurable`、`enumerable`、`writable`特征的默认值均为`false`。
#### `Object.getOwnPropertyDescriptor()` ####  
```javascript
var Obj1 = {};
Obj1.age = 30;
var b = Object.getOwnPropertyDescriptor(Obj1, "age");
/*
返回一个包含着特征-特征值键值对的对象
b = {
  configurable: true,
  enumerable: true,
  writable: true,
  value: 25
}
*/
```
## 枚举属性 ##
#### `for...in...` ####
返回实例和原型链上所有可以枚举的属性。
#### `Object.keys()` ####
返回一个数组，包含着实例中可以枚举的属性。
#### `Object.getOwnPropertyNames()` ####
返回一个数组，包含着实例中所有属性（包括不可以枚举的）。
### 原型操作 ####
#### `Object.getPrototypeOf()` ####
返回指定对象的原型。
#### `isPrototypeOf()` ####
用于测试一个对象是否存在于另一个对象的原型链上，返回Boolean。
```javascript
prototypeObj.isPrototypeOf(object)
```
#### [`Object.create()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create) ####
创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。
```javascript
Object.create(proto[, propertiesObject])
```
#### `hasOwnProperty()` ####
返回一个布尔值，指示对象自身（不包括原型链）属性中是否具有指定的属性。
```javascript
obj.hasOwnProperty(prop)
```
