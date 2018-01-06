---
title: es6 class详解
date: 2017-12-25 22:47:17
tags: es6 javascript
categories:
  - javascript
---

> ES6 引入了 Class（类）这个概念，作为对象的模板。通过class关键字，可以定义类，事实上，ES6 的class可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。本文通过对比ES5与ES6的用法，来加深对ES6 class的理解。

- 先来看一个es5通过构造函数来生成实例对象的例子

~~~~
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.intro = function () {
  console.log("I am "+this.name+",I am "+this.age)
};

var p = new Person("Tony", 20);
~~~~

- 我们这里先直接来看用es6的class怎么实现同样的功能

~~~~
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  intro() {
    console.log("I am "+this.name+",I am "+this.age)
  }
}
~~~~
> 上面这段ES6的代码定义了一个“类”，里面有一个constructor方法，这就是构造方法，而this关键字则代表实例对象。也就是说，ES5 的构造函数Person，对应 ES6 的Person类的构造方法。

Person类除了构造方法，还定义了一个intro方法。注意，定义“类”的方法的时候，不需要加上function这个关键字，直接把函数定义放进去了就可以了。另外，方法之间不需要逗号分隔，加了会报错。

> ES6 的类，完全可以看作构造函数的另一种写法，类的数据类型就是函数，类本身就指向构造函数，使用的时候，也是直接对类使用new命令，跟构造函数的用法完全一致。

~~~~
typeof Person // "function"
Person === Person.prototype.constructor // true
var p = new Person();
p.intro()  // I am Tony,I am 20
~~~~

> 构造函数的prototype属性，在 ES6 的“类”上面继续存在。事实上，类的所有方法都定义在类的prototype属性上面。

~~~~
class Person {
  constructor() {
    // ...
  }
  intro() {
    // ...
  }
}

// 等同于

Person.prototype = {
  constructor() {},
  intro() {}
};
~~~~

> 由于类的方法都定义在prototype对象上面，所以类的新方法可以添加在prototype对象上面。Object.assign方法可以很方便地一次向类添加多个方法。

~~~~
class Person {
  constructor(){
    // ...
  }
}

Object.assign(Person.prototype, {
  intro(){},
  show(){}
});
~~~~
> prototype对象的constructor属性，直接指向“类”的本身,类的内部所有定义的方法，都是不可枚举的，而ES5里边构造函数的prototype绑定的方法是可枚举的。

~~~~
class Person {
  constructor(x, y) {
    // ...
  }

  intro() {
    // ...
  }
}

Object.keys(Person.prototype)
// []
Object.getOwnPropertyNames(Person.prototype)
// ["constructor","intro"]

ES5 写法

var Person = function (x, y) {
  // ...
};

Person.prototype.intro = function() {
  // ...
};

Object.keys(Person.prototype)
// ["intro"]
Object.getOwnPropertyNames(Person.prototype)
// ["constructor","intro"]
~~~~

> 类的属性名，可以采用表达式。如下，类的方法名getAge，是从表达式得到的。

~~~~
let methodName = 'getAge';

class Person {
  constructor(length) {
    // ...
  }

  [methodName]() {
    // ...
  }
}
~~~~

> 与函数一样，类也可以使用表达式的形式定义。

~~~~
const Person = class P {
  getClassName() {
    return p.name;
  }
};
~~~~

上面代码使用表达式定义了一个类。需要注意的是，这个类的名字是Person而不是P，P只在 Class 的内部可用，指代当前类。类的内部没用到的话，可以省略。因为可以使用表达式，可以试着写一个立即执行的class，如下，Show就是一个立即执行的类。

~~~~
let Show = new class {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}('张三');

Show.sayName(); // "张三"
~~~~

> 类不存在变量提升（hoist），这一点与 ES5 完全不同。ES6 不会把类的声明提升到代码头部。所以，必须先声明，在调用，这样做与继承有关，必须保证子类在父类之后定义。类可以通过extends关键字实现继承。子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。子类实例的构建，是基于对父类实例加工，只有super方法才能返回父类实例。

~~~~
new Show()  // ReferenceError
class Show{
    
}
~~~~

> 类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。静态方法可以与非静态方法重名,并且父类的静态方法，可以被子类继承。

~~~~

class Show {
  constructor(x, y){
      this.x = x;
      this.y = y;
  }
  static print() {
    return 'hello';
  }
}

Show.print()                    // 'hello'  

var foo = new Show();
foo.print()                     // TypeError: foo.print is not a function

class Start extends Show {
    constructor(x, y, color) {
        super(x, y); // 调用父类的constructor(x, y)
        this.color = color;
    }
}

Start.print()                    // 'hello'


~~~~

> Object.getPrototypeOf方法可以用来从子类上获取父类。

~~~~
Object.getPrototypeOf(Start) === Show   // true
~~~~

> super这个关键字，既可以当作函数使用，也可以当作对象使用。在这两种情况下，它的用法完全不同。

第一种情况，super作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次super函数。子类构造函数之中的super()，代表调用父类的构造函数。这是必须的，否则 JavaScript引擎会报错。super虽然代表了父类的构造函数，但是返回的是子类的实例，即super内部的this指的是子类，因此super()在这里相当于

~~~~
parent.prototype.constructor.call(this)。
~~~~

特别注意，作为函数时，super()只能用在子类的构造函数之中，用在其他地方就会报错。

第二种情况，super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。

~~~~

class Show {
  print() {
    return 2;
  }
}

class Start extends Show {
  constructor() {
    super();
    console.log(super.print()); // 2
  }
}

let b = new Start();

~~~~

上面代码中，子类Start当中的super.print()，就是将super当作一个对象使用。这时，super在普通方法之中，指向Show.prototype，所以super.print()Show.prototype.print()。但是需要注意，由于super指向父类的原型对象，所以定义在父类实例上的方法或属性，是无法通过super调用的。反之，如果属性定义在父类的原型对象上，super就可以取到。

ES6 规定，通过super调用父类的方法时，方法内部的this指向子类。由于this指向子类，所以如果通过super对某个属性赋值，这时super就是this，赋值的属性会变成子类实例的属性。

如果super作为对象，用在静态方法之中，这时super将指向父类，而不是父类的原型对象。也就是说super在静态方法之中指向父类，在普通方法之中指向父类的原型对象。

除了以上之外，想了解更多class的特性，请参考[阮一峰老师的es6入门class教程](http://es6.ruanyifeng.com/#docs/class)