---
title: apply和call的用法和区别
date: 2018-3-24 22:47:17
tags: [javascript]
categories:
  - javascript
---


##### 每个函数都包含三个非继承而来的方法：apply()方法和call()方法

> 都是在特定的作用域中调用函数，等于设置函数体内this对象的值，以扩充函数赖以运行的作用域。

#####  Function.prototype.apply() 

>apply() 方法调用一个函数，其具有一个指定的this值，以及作为一个数组（或类似数组的对象）提供的参数。

使用语法：


```
func.apply(thisArg, [argsArray])

```

##### 参数说明

1.  thisArg

可选的。在 func 函数运行时使用的 this 值。需要注意的是，使用的 this 值并不一定是该函数执行时真正的 this 值，如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象（浏览器中就是window对象），同时值为原始值（数字，字符串，布尔值）的 this 会指向该原始值的包装对象。
    
2. argsArray

可选的。一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 func 函数。如果该参数的值为null 或 undefined，则表示不需要传入任何参数。从ECMAScript 5 开始可以使用类数组对象。

##### 返回值

调用有指定this值和参数的函数的结果。

##### 例子：


```
window.name = 'css';
document.name = 'javascript';

var obj = {
    name: 'html5' 
};
function changeName(){
    console.log(this.name);
}

changeName()        // css 当前this指向window
changeName.apply()  // css 参数为空，默认指向当前环境的this
changeName.apply(document)  // javascript 当前的this指向document对象
changeName.apply(obj)  // html5 当前的this指向obj对象

function beforeWork(work){
    this.work = work;
    this.skills = function () {
        console.log(this.work)
    }
}
function newWork(work){
   // beforeWork.call(this, work);
   beforeWork.apply(this, arguments); //  arguments 是一个函数的局部变量。 它可以被用作被调用对象的所有未指定的参数。 
}
let getWork = new newWork('web');
getWork.skills(); // web newWork继承了beforeWork，实例化之后，getWork就继承了newWork的属性和方法

```
#####  Function.prototype.call()

> call() 方法调用一个函数, 其具有一个指定的this值和分别地提供的参数(参数的列表)。

<font color=red>该方法的作用和 apply() 方法类似，只有一个区别，就是call()方法接受的是若干个参数的列表，而apply()方法接受的是一个包含多个参数的数组或者类数组。</font>

使用语法：


```
fun.call(thisArg, arg1, arg2, ...)

```

##### 参数说明

1.  thisArg

在fun函数运行时指定的this值。需要注意的是，指定的this值并不一定是该函数执行时真正的this值，如果这个函数处于非严格模式下，则指定为null和undefined的this值会自动指向全局对象(浏览器中就是window对象)，同时值为原始值(数字，字符串，布尔值)的this会指向该原始值的自动包装对象。
    
2. arg1, arg2, ...

指定的参数列表。

##### 返回值

返回值是你调用的方法的返回值，若该方法没有返回值，则返回undefined。

##### 例子：


```
window.name = 'css';
document.name = 'javascript';

var obj = {
    name: 'html5' 
};
function changeName(){
    console.log(this.name);
}

changeName()        // css 当前this指向window
changeName.call()  // css 参数为空，默认指向当前环境的this
changeName.call(document)  // javascript 当前的this指向document对象
changeName.call(obj)  // html5 当前的this指向obj对象

function getInfo(work) {
    console.log(this.name + "," + this.age + "," + work)
}

var Info = {
    name: 'javascript',
    age: 18
}
getInfo.call(Info, 'web'); // javascript,18,web 当调用getInfo方法的时候，该方法的this值会绑定到Info对象

```



---