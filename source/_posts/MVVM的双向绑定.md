---
title: MVVM的双向绑定
date: 2018-01-02 16:31:09
categories:
  - Vue
  - MVVM
tags: 
  - Vue
  - MVVM
---
> 什么是MVVM？MVVM是Model-View-ViewModel的缩写。MVVM最早由微软提出来，它借鉴了桌面应用程序的MVC思想，在前端页面中，把Model用纯JavaScript对象表示，View负责显示，两者做到了最大限度的分离。

把Model和View关联起来的就是ViewModel。ViewModel负责把Model的数据同步到View显示出来，还负责把View的修改同步回Model。MVVM的设计思想：关注Model的变化，让MVVM框架去自动更新DOM的状态，从而把开发者从操作DOM的繁琐步骤中解脱出来！

> 理解MVVM的原理之前，我们先来回顾下数据绑定，数据绑定是指页面UI布局与数据源建立连接的过程。

单向数据绑定：指的是我们先把Template(模板)写好，然后把Template(模板)和Model(数据）整合到一起形成HTML代码，然后把这段HTML代码插入到文档流里面,形成完整的View(视图)。如下图所示：

![image](https://s1.ax1x.com/2017/12/29/zykLT.png)

单向数据绑定缺点：HTML代码一旦生成完以后，就没有办法再变了，如果有新的数据来了，那就必须把之前的HTML代码去掉，再重新把新的数据和模板一起整合后插入到文档流中。

双向数据绑定：数据模型（Module）和视图（View）之间的双向绑定。

![image](https://s1.ax1x.com/2017/12/29/zyEeU.png)

用户在视图上的修改会自动同步到数据模型中去，同样的，如果数据模型中的值发生了变化，也会立刻同步到视图中去。双向数据绑定的优点是无需进行和单向数据绑定的那些CRUD（Create，Retrieve，Update，Delete）操作，到这里，我们不难发现，MVVM的核心原理就是双向数据绑定。

> 目前比较流行的MVVM框架就是[VueJS](https://cn.vuejs.org/) ，VueJS 是使用 ES5 提供的 Object.defineProperty() 方法，来实现双向数据绑定的。

我们先来详细了解一下，Object.defineProperty这个方法的使用。[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)给出的解释是：

> Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。

语法如下：

~~~~
Object.defineProperty(obj, prop, descriptor)
~~~~

参数


    obj         要在其上定义属性的对象。

    prop        要定义或修改的属性的名称。

    descriptor  将被定义或修改的属性描述符。
    

返回值

    被传递给函数的对象。
    
一个简单例子来看下怎么使用
```

var obj = {}
Object.defineProperty(obj,"name",{
  value:"web"
})
console.log(obj.name);//web

```

前两个参数就不用过多解释了，一看就明白了，我们重点分析一下第三个参数  descriptor


它有以下取值：

```
value：该属性对应的值可以是任何有效的 JavaScript 值（数值，对象，函数等）。默认为 undefined。

writable：当且仅当该属性的writable为true时，value才能被赋值运算符改变。默认为 false。

configurable：总开关，当且仅当该属性的 configurable 为 true 时，该属性描述符才能够被改变，同时该属性也能从对应的对象上被删除。默认为 false。

enumerable：当且仅当该属性的enumerable为true时，该属性才能够出现在对象的枚举属性中。默认为 false。

get：一个给属性提供 getter 的方法，如果没有 getter 则为 undefined。该方法返回值被用作属性值。默认为 undefined。

set：一个给属性提供 setter 的方法，如果没有 setter 则为 undefined。该方法将接受唯一参数，并将该参数的新值分配给该属性。默认为 undefined。

```

根据以上的理解，这时候，我们再来看刚才的例子

```
var obj = {}
Object.defineProperty(obj,"name",{
  value:"web"
})
console.log(obj.name);//web
```

 第一次设置的时候，我们只设置了 value，别的并没有设置，但是descriptor这个参数默认帮我们把writable，configurable，enumerable都设为false。也就是说，上面的代码和下面的是等价的，特别注意，**这仅仅是第一次设置的时候**。

```
var obj= {}
Object.defineProperty(obj,"name",{
  value:"web",
  writable:false,
  enumerable:false,
  configurable:false
})
console.log(obj.name);//web
```
注意我们上面讲的默认值，如果第一次不设置，它会默认帮你设置为false。但是，你第二次再设置他的时候，就会出现不同的情况，我们来逐个分析：

configurable 总开关，第一次设置默认为 false 之后，第二次再设置就会报错，比如说：

```
var obj= {}
Object.defineProperty(obj,"name",{
  value:"web"
})
console.log(obj.name);//web
Object.defineProperty(obj,"name",{
  value:"web1",
  configurable:true
})
console.log(obj.name);//TypeError: Cannot redefine property: name
```

writable  如果设置为fasle，就变成只读了。
```
var new_obj = {};
Object.defineProperty(new_obj,"name",{
  value:"web"
})
console.log(new_obj.name); //web

默认设为只读，接下来，我们尝试修改name属性的值

第一种方式：
Object.defineProperty(new_obj,"name",{
  value:"web1"
})
console.log(new_obj.name);//TypeError: Cannot redefine property: name
第二种方式：
new_obj.name = "javascript"；//没有错误抛出（在严格模式下会抛出，即使之前已经有相同的值）

console.log(new_obj.name); //web， 赋值不起作用。
```

enumerable
属性定义了对象的属性是否可以在 for...in 循环和 Object.keys() 中被枚举。

```
var obj = {}
Object.defineProperty(obj,"age",{
  value:18,
  enumerable:true
})
console.log(Object.keys(obj));// 打印["age"]
改为false

var obj = {}
Object.defineProperty(obj,"age",{
  value:20,
  enumerable:false //注意 这里改了
})
console.log(Object.keys(obj));// 打印[]
for...in 类似，不赘述了

```
接下来我们来分析下get和set，关于get和set，MDN上有这样一段话：

> 如果一个描述符同时设置了value,writable,get和set关键字，那么它将被认为是一个数据描述符。如果一个描述符同时有value或writable和get或set关键字，将会产生一个异常。

通俗的说，在 descriptor 中不能同时设置访问器（get 和 set）和 wriable 或 value，否则会错，就是说想用 get 和 set，就不能用 writable 或 value 中的任何一个。


```
var new_obj = {}
Object.defineProperty(new_obj,"num",{
  set:function(newValue){
    console.log("开始赋值");
    console.log("你要赋值给我,我的新值是"+ newValue)
  },
  get:function(){
    console.log("开始取值");
    return 100; //注意这里，我直接返回100
  }
})
new_obj.num = 10   //开始赋值    你要赋值给我,我的新值是10
console.log(new_obj.num)    //开始取值  100 注意这里，和我直接返回的值是相同的

```

简单来说，这个 “num” 属性，赋值或者取值的时候会分别触发 set 和 get 对应的函数。

> Vue 就是通过设定对象属性的 setter/getter 方法来监听数据的变化，通过getter进行依赖收集，而每个setter方法就是一个观察者，在数据变更的时候通知订阅者更新视图。

因此需要我们执行以下3个步骤，实现数据的双向绑定：

1、实现一个数据监听器Observer，能够对数据对象的所有属性进行监听，如有变动可拿到最新值并通知订阅者，[查看代码，observer.js](https://github.com/liutaochange/MVVM/blob/master/js/observer.js)

2、实现一个指令解析器Compile，对每个元素节点的指令进行扫描和解析，根据指令模板替换数据，以及绑定相应的更新函数，[查看代码，compile.js](https://github.com/liutaochange/MVVM/blob/master/js/compile.js)

3、实现一个Watcher，作为连接Observer和Compile的桥梁，能够订阅并收到每个属性变动的通知，执行指令绑定的相应回调函数，从而更新视图，[查看代码，watcher.js](https://github.com/liutaochange/MVVM/blob/master/js/watcher.js)


实现mvvm的主入口：

> MVVM作为数据绑定的入口，整合Observer、Compile和Watcher三者，通过Observer来监听自己的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据model变更的双向绑定效果。


```
function MVVM(options) {
    this.$options = options || {};
    var data = this._data = this.$options.data;
    var me = this;

    // 数据代理
    // 实现 vm.xxx -> vm._data.xxx
    Object.keys(data).forEach(function(key) {
        me._proxyData(key);
    });

    this._initComputed();

    observe(data, this);

    this.$compile = new Compile(options.el || document.body, this)
}

MVVM.prototype = {
    $watch: function(key, cb, options) {
        new Watcher(this, key, cb);
    },

    _proxyData: function(key, setter, getter) {
        var me = this;
        setter = setter ||
            Object.defineProperty(me, key, {
                configurable: false,
                enumerable: true,
                get: function proxyGetter() {
                    return me._data[key];
                },
                set: function proxySetter(newVal) {
                    me._data[key] = newVal;
                }
            });
    },

    _initComputed: function() {
        var me = this;
        var computed = this.$options.computed;
        if (typeof computed === 'object') {
            Object.keys(computed).forEach(function(key) {
                Object.defineProperty(me, key, {
                    get: typeof computed[key] === 'function'
                        ? computed[key]
                        : computed[key].get,
                    set: function() {}
                });
            });
        }
    }
};
```
[查看mvvm入口代码，mvvm.js](https://github.com/liutaochange/MVVM/blob/master/js/mvvm.js)

最后，在页面里引用如下代码，运行试试效果


```
<div id="app">
    <input type="text" v-model="word">
    <p>{{word}}</p>
    <button v-on:click="sayHi">change model</button>
</div>

<script src="./js/observer.js"></script>
<script src="./js/watcher.js"></script>
<script src="./js/compile.js"></script>
<script src="./js/mvvm.js"></script>
<script>
    var vm = new MVVM({
        el: '#app',
        data: {
            word: 'Hello World!'
        },
        methods: {
            reverseMessage: function () {
                this.word = this.word.split('').reverse().join('')
            }
        }
    });
</script>
```

