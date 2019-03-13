 ## 一、什么是面向对象 ##

把客观对象抽象成属性数据({})和数据的相关操作，把内部细节和不相关的信息隐藏起来，把同一个类型的客观对象的属性数据和操作绑定在一起， 封装成类，并且允许分成不同层次进行抽象，通过继承实现属性和操作的共享。
* **1、基本概念**
    * **对象**：对象是人们要进行研究的任何事物，从最简单的整数到复杂的飞机等均可看作对象，它不仅能表示具体的事物，还能表示抽象的规则、计划或事件。
    * **类**：具有相同特性（数据元素）和行为（功能）的对象的抽象就是类。因此，对象的抽象是类，类的具体化就是对象，也可以说类的实例是对象，类实际上就是一种数据类型。
    * **类的结构**：在客观世界中有若干类，这些类之间有一定的结构关系。通常有两种主要的结构关系，即一般--具体结构关系，整体--部分结构关系。
    * **消息和方法**：
        * 对象之间进行通信的结构叫做消息。
        * 类中操作的实现过程叫做方法，一个方法有方法名、返回值、参数、方法体。
* **2、特征**
    * 对象唯一性
    * 抽象性
    * 封装(基本)
    * 继承性(基本)
    * 多态性(基本)
##  二、设计原则  ##    
* **1、五大设计原则**
    * 单一职责原则：对象仅具有单一功能的概念。一个程序仅做好一件事，如果功能特别复杂就拆分。
    * 开放封闭原则：对于扩展是开放的，对于修改是封闭的。增加需求是扩展新代码，而不是修改已有代码（这是软件设计的最终目标）。
    * 里氏替换原则：在不改变程序正确性的前提下，其中的对象是可以被子类所替换的。子类能覆盖父类，父类能出现的地方子类就可以出现，js用的少。
    * 接口隔离原则：多个特定客户端接口要好于一个广泛用途的接口。类似单一职责，更关注接口，js用的少。
    * 依赖反转原则：一个方法应该依赖于抽象而不是一个实例。面上接口编程，仅仅关注接口而不关注类的实现，js中没有接口用的少。
## 三、23种设计模式 ##
* 简介： 每种设计模式的出现都是为了弥补语言在某方面的不足，解决特定环境下的问题。思想是相通的。只不过不同的设计语言有其特定的实现。对javascript这种动态语言来说，弱类型的特性，与生俱来的多态性，导致某些设计模式不自觉的我们都在使用。只不过没有对应起来罢了。
* 　重复而言，使用设计模式的目的是为了提高我们解决问题的效率，不同的设计模式也是针对不同环境的特定方案，不仅仅是单独的某一种设计模式，大多数情况下都是多个模式共存的。切勿为了使用设计模式而强行引入，也切勿不同情况坚持使用某一种设计模式。不要被形式总之快速高效的解决问题才是我们的唯一目的。

* **1、创建型**
      
     * 工厂模式
     * 建造者模式
     * 单例模式
     * 原型模式
* **2、结构型模式**
  
  * 适配器模式
  * 装饰器模式
  * 代理模式
  * 外观模式
  * 桥接模式
  * 组合模式
  * 享元模式
  
* **3、行为模式**

    * 策略模式
    * 模板方法模式
    * 观察者模式
    * 迭代器模式
    * 职责模式
    * 命令模式
    * 备忘录模式
    * 状态模式
    * 访问者模式
    * 中介者模式
    * 解释器模式
    
## 四、工厂模式(factory) ##
 *  简单工厂模式 : 是由一个工厂对象决定创建出哪一种产品类的实例
 
 ````
class Plant{
    constructor(name) {
        this.name=name;
    }
    grow() {
        console.log('growing~~~~~~');    
    }
}
class Apple extends Plant{
    constructor(name) {
        super(name);
        this.taste='甜';
    }
}
class Orange extends Plant{
    constructor(name) {
        super(name);
        this.taste='酸';
    }
}
class Factory{
    static create(name) {
        switch (name) {
            case 'apple':
                return new Apple('苹果');
            case 'orange':
                return new Orange('橘子');
        }
    }
}
let apple=Factory.create('apple');
console.log(apple);
let orange=Factory.create('orange');
console.log(orange);
````

* 工厂方法模式（多态性工厂模式）
    *  核心的工厂类不再负责所有产品的创建，而是将具体的工作交个子类去做。
    
```
class Plant{
    constructor(name) {
        this.name=name;
    }
    grow() {
        console.log('growing~~~~~~');    
    }
}
class Apple extends Plant{
    constructor(name) {
        super(name);
        this.taste='甜';
    }
}
class Orange extends Plant{
    constructor(name) {
        super(name);
        this.taste='酸';
    }
}
class AppleFactory{
    create() {
        return new Apple();
    }
}
class OrangeFactory{
    create() {
        return new Orange();
    }
}
const settings={
    'apple': AppleFactory,
    'orange':OrangeFactory
}
let apple=new settings['apple']().create();
console.log(apple);
let orange=new settings['orange']().create();
console.log(orange);

```

* 抽象工厂模式是指当有多个抽象角色时，使用的一种工厂模式。它向客户端提供一个接口，使客户端在不必指定产品的具体情况下，创建多个产品族中的产品对象。

 ```
 class Button{
     render() {

     }
 }
 class AppleButton{
     render() {
        console.log('苹果按钮');
     }
 }
 class WindowButton{
     render() {
        console.log('Windows按钮');
     }
 }

 class Icon{
     render() {

     }
 }
 class AppleIcon{
     render() {
        console.log('苹果图标');
     }
 }
 class WindowIcon{
     render() {
        console.log('Windows图标');
     }
 }
 class Factory{
     createButton() {}
     createIcon() {}
 }
 class AppleFactory{
     createButton() {
         return new AppleButton();
     }
     createIcon() {
         return new AppleButton();
     }
 }
 class WindowsFactory{
     createButton() {
         return new WindowButton();
     }
     createIcon() {
         return new WindowIcon();
     }
 }
 const settings={
     'apple': AppleFactory,
     'windows':WindowsFactory
 }
 let appleFactory=new settings['apple']();
 appleFactory.createButton().render();
 appleFactory.createIcon().render();

 let windowsFactory=new settings['windows']();
 windowsFactory.createButton().render();
 windowsFactory.createIcon().render();

 ```

* 单例模式（单子模式/单体模式）
  * 含义： 软件中较为简单最常用的设计模式，保证‘一个类仅有一个实例’，并提供一个访问它的全局访问点（getInstance）。
  * 原理思路：用一个变量来标识当前是否已经为某个类创建过对象，如果是，则在下一次获取该类的实例时，直接返回之前创建的对象。
  * 优点：声明一个命名空间生成一个唯一的全局变量可以解决命名冲突问题；可以快速定位bug;比new对象更占优势；
  * 缺点：扩展性不好；灵活性不好；储存同一个位置，修改属性值慎重。
  
```
// typescript 
class Window {
    constructor(name) {
        this.name = name;
    }
    static getInstance(name) {
        if (!this.instance) {
            this.instance = new Window(name);
        }
        return this.instance;
    }
}

var w1 = Window.getInstance();
var w2 = Window.getInstance();
console.log(w1 === w2);

```

```
//ES5单例模式
let  Window = function(name) {
    this.name=name;
}
Window.prototype.getName=function () {
    console.log(this.name);
}
Window.getInstance=(function () {
    let window=null;
    return function (name) {
        if (!window)
           window=new Window(name);
        return window;
    }
})();
let window=Window.getInstance('zfpx');
window.getName();

```


```
//透明单例
let Window=(function () {
    let window;
    let Window=function (name) {
        if (window) {
            return window;
        } else {
            this.name=name;
            return (window=this);
        }
    }
    Window.prototype.getName=function () {
        console.log(this.name);
    }
    return Window;
})();

let window1=new Window('zfpx');
let window2=new Window('zfpx');
window1.getName();
console.log(window1 === window2)

```

```
//单例与构建分离
function Window(name) {
    this.name=name;
}
Window.prototype.getName=function () {
    console.log(this.name);
}

let createSingle=(function () {
    let instance;
    return function (name) {
        if (!instance) {
            instance=new Window();
        }
        return instance;
    }
})();

let window1=new createSingle('zfpx');
let window2=new createSingle('zfpx');
window1.getName();
console.log(window1 === window2)

```

```
//封装变化
function Window(name) {
    this.name=name;
}
Window.prototype.getName=function () {
    console.log(this.name);
}

let createSingle=function (Constructor) {
    let instance;
    return function () {
        if (!instance) {
            Constructor.apply(this,arguments);
            Object.setPrototypeOf(this,Constructor.prototype)
            instance=this;
        }
        return instance;
    }
};
let CreateWindow=createSingle(Window);
let window1=new CreateWindow('zfpx');
let window2=new CreateWindow('zfpx');
window1.getName();
console.log(window1 === window2)

```

```
应用场景：
function createStore(reducer) {
    let state;
    let listeners=[];
    function getState() {
        return state;
    }
    function dispatch(action) {
        state=reducer(state,action);
        listeners.forEach(l=>l());
    }
    function subscribe(listener) {
        listeners.push(listener);
        return () => {
            listeners = listeners.filter(item => item!=listener);
            console.log(listeners);
        }
    }
    dispatch({});
    return {
        getState,
        dispatch,
        subscribe
    }
}
let store = createStore();

```

```
// 应用场景
class Login{
    constructor() {
        this.element=document.createElement('div');
        this.element.innerHTML=(
            `
            用户名 <input type="text"/>
            <button>登录</button>
            `
        );
        this.element.style.cssText='width: 100px; height: 100px; position: absolute; left: 50%; top: 50%; display: block;';
        /* this.element.style.width='100px';
        this.element.style.height='100px';
        this.element.style.position='absolute';
        this.element.style.left='50%';
        this.element.style.top='50%'; */
        document.body.appendChild(this.element);
    }
    show() {
        this.element.style.display='block';
    }
    hide() {
        this.element.style.display='none';
    }
}
Login.getInstance=(function () {
    let instance;
    return function () {
        if (!instance) {
            instance=new Login();
        }
        return instance;
    }
})();

document.getElementById('showBtn').addEventListener('click',function (event) {
    Login.getInstance().show();
});
document.getElementById('hideBtn').addEventListener('click',function (event) {
    Login.getInstance().hide();
});

```

## 五、适配器模式（adapter） ##
* 含义：将一个（A）接口（类的方法），通过一个类（适配器B）转换成客户需要的接口（方法）,而不需要修改（A）的代码。通过一个类过滤下改变了方法。
* 思路原理：A=>B(适配器)=>C（我需要的），但是A不变。
* 应用场景

```
// 插件适配 适配参数、后端接口数据
function ajax(options){
  let _default = {
      method:'GET',
      dataType:'json'
  }
  for(let attr in options){
    _default[attr] = options[attr]||_default[attr];
  }
}

function get(url){
  let options = {method:'GET',url};
  ajax(options);
}
```

```
//promisify
let fs=require('fs');
function promisify(readFile) {
    return function (filename,encoding) {
        return new Promise(function (resolve,reject) {
            readFile(filename,encoding,function (err,data) {
                if (err)
                    reject(err);
                else
                    resolve(data);
            })
        });
    }
}
let readFile=promisify(fs.readFile);
readFile('./1.txt','utf8').then(data => console.log(data));

```

```
//jquery
let $=require('jquery');
$.ajax({
    url,
    type: 'POST',
    dataType: 'json',
    data:{id:1}
});

let $={
    ajax(options) {
        fetch(options.url,{
            method: options.type,
            body:JSON.stringify(options.data)
        })
    }
}

```

```
// computed
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="https://cdn.bootcss.com/vue/2.5.17/vue.js"></script>
    <title>vue</title>
</head>
<body>
<div id="root">
<p>{{name}}</p>
<p>{{upperName}}</p>
</div>    
<script>
let vm=new Vue({
    el: '#root',
    data: {
        name:'zfpx'
    },
    computed: {
        upperName() {
            return this.name.toUpperCase();
        }
    }
});
</script>
</body>
</html>

```
## 六、装饰器（decorator）模式 ##

* 含义：装饰器是将一个对象嵌入另一个对象之中，实际上相当于这个对象被另一个对象包括起来，形成一条包装链条，请求随着这条链条依次传递到所有对象，每个对象都有处理这个请求的机会。
* 功能：es7@func。在不改变原有的结构和功能对象的前提下添加新功能
* 优点:装饰比继承更加灵活
* 缺点

```
// 基本代码
class Duck{
    eat(food) {
        console.log('吃${food}');
    }
}

class TangDuck{
    constructor() {
        this.duck=new Duck();
    }
    eat() {
        this.duck.eat();
        console.log('谢谢');
    }
}

```

```
// 包装器
class Coffee{
  make(water){
    return `${water}+咖啡`;
  }
  cost(){
      return 10;
  }
}

class MilkCoffee{
    constructor(parent){
        this.parent = parent;
    }
    make(water){
        return `${this.parent.make(water)}+牛奶`;
    }
    cost(){
        return this.parent.cost()+1;
    }
}

class SugerCoffee{
    constructor(parent){
        this.parent = parent;
    }
    make(water){
        return `${this.parent.make(water)}+糖`;
    }
    cost(){
        return this.parent.cost()+2;
    }
}
let coffee = new Coffee();
let milkCoffee = new MilkCoffee(coffee);
let milksugerCoffee = new SugerCoffee(milkCoffee);
console.log(milksugerCoffee.make('水')+'='+milksugerCoffee.cost());

```

```
//表单验证
<body>
    <form action="">
        用户名<input type="text" name="username" id="username">
        密码<input type="text" name="password" id="password">
        <button id="submit-btn" >提交</button>
    </form>
    <script>
         Function.prototype.before = function(beforeFn){
            let _this = this;
            return function(){
                let ret = beforeFn.apply(this,arguments);
                if(ret)
                _this.apply(this,arguments);
            }
        }
      function submit(){
          alert('提交表单');
      }
      submit= submit.before(function(){
          let username = document.getElementById('username').value;
          if(username.length<6){
              return alert('用户名不能少于6位');
          }
          return true;
      });
      submit = submit.before(function(){
          let username = document.getElementById('username').value;
          if(!username){
              return alert('用户名不能为空');
          }
          return true;
      });


      document.getElementById('submit-btn').addEventListener('click',submit);
```

## 七、代理（proxy）模式 ##
 * 含义：一个对象不可以引用另一个对象，所以通过一个代理对象，在两者之间起个中介作用。可以在使用者和目标使用者之间加一个代理对象实现控制，控制对它的访问，
 * 个人理解：代理对象内部有真实对象的引用，从而可以操作真实对象；代理对象提供和真实对象相同名字的接口。
 * 使用场景：当我们需要使用的对象很复杂或者需要很长时间去构造，这时就可以使用代理模式(Proxy)。例如：如果构建一个对象很耗费时间和计算机资源，代理模式(Proxy)允许我们控制这种情况，直到我们需要使用实际的对象。一个代理(Proxy)通常包含和将要使用的对象同样的方法，一旦开始使用这个对象，这些方法将通过代理(Proxy)传递给实际的对象。

        
    * 保护代理。一个对象只有有限的访问权限，代理对象可以验证用户的权限
    * 虚拟代理。虚拟代理是将调用本体方法的请求进行管理，等到本体适合执行时，再执行。将开销很大的对象，延迟到真正需要它的时候再执行。比如：利用虚拟代理实现图片预加载功能
    * 缓存代理。缓存代理可以为开销大的一些运算结果提供暂时性的存储，如果再次传进相同的参数是，直接返回结果，避免大量重复计算。
    
    * 防抖代理、图片懒加载代理、跨域代理
    
* 优点: 对外部提供统一的接口方法,如果需有修改真实角色操作的，不用修改它，而是在外部‘包’的一层进行附加行为。
* 代理模式 VS 适配器模式： 适配器提供不同接口，代理模式提供一模一样的接口。
* 代理模式 VS 装饰器模式：装饰器模式原来的功能不变还可以使用，代理模式改变了原来的功能。
* es7代理：Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”

`````
// 基本代理模式
class Goole{
    constructor() {    }
    get() {
        return 'google';
    }
}

class Proxy {
    constructor() {
        this.google=new Goole();
    }
    get() {
        return this.google.get();
    }
}
let proxy = new Proxy();
let ret = proxy.get();
console.log(ret);
`````

````
// 案例一事件委托（自上而下）
<body>
    <ul id="list">
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>
<script>
  let list = document.querySelector('#list');
  list.addEventListener('click',event=>{
       alert(event.target.innerHTML);
  });     
</script>    
</body>
````

`````
// 防抖代理
<body>
    <ul id="todos">
    </ul>
<script>
    let todos = document.querySelector('#todos');
    window.onload = function(){
        fetch('/todos').then(res=>res.json()).then(response=>{
            todos.innerHTML = response.map(item=>`<li "><input value="${item.id}" type="checkbox" ${item.completed?"checked":""}/>${item.text}</li>`).join('');
        });
    }
    function toggle(id){
       fetch(`/toggle/${id}`).then(res=>res.json()).then(response=>{
            console.log('response',response);
        });
    }
    let LazyToggle = (function(id){
        let ids = [];
        let timer;
        return function(id){
            ids.push(id);
            if(!timer){
              timer = setTimeout(function(){
                  toggle(ids.join(','));
                  ids = null;
                  clearTimeout(timer);
                  timer = null;
              },2000);
            }
        }
    })();
    todos.addEventListener('click',function(event){
        let checkbox = event.target;
        let id = checkbox.value;
        LazyToggle(id);
    });
</script>
`````

```
//es7api
let wang={
    name: 'wangy',
    age: 29,
    height:165
}
let wangMama=new Proxy(wang,{
    get(target,key) {
        if (key == 'age') {
            return wang.age-1;
        } else if (key == 'height') {
            return wang.height+5;
        }
        return target[key];
    },
    set(target,key,val) {
        if (key == 'boyfriend') {
            let boyfriend=val;
            if (boyfriend.age>40) {
                throw new Error('太老');
            } else if (boyfriend.salary<20000) {
                throw new Error('太穷');
            } else {
                target[key]=val;
                return true;
            }
        }
    }
});
console.log(wangMama.age);
console.log(wangMama.height);
wangMama.boyfriend={
    age: 41,
    salary:3000
}

```

## 八、外观者（appearance）模式 ##
 * 含义：该模式就是把一些复杂的流程封装成一个接口供给外部用户更方便的使用，核心是“门面角色”，也就是常用的封装暴露出接口。
 * 场景：
    * 为复杂的模块或子系统提供外界访问的模块
    * 子系统相互独立
    
```
// 基本代码
class CPU{
    start() {console.log('打开CPU');}
}
class Memory{
    start() {console.log('打开内存');}
}
class Disk{
    start() {console.log('打开硬盘');}
}
class Computer{
    constructor() {
        this.cpu=new CPU();
        this.memory=new Memory();
        this.disk=new Disk();
    }
    start() {
        this.cpu.start();
        this.memory.start();
        this.disk.start();
    }
}
new Computer().start();
```

    
## 九、观察者（observer）模式 ##
 * 含义：又叫（发布-订阅模式），比如生活中：微信公众号订阅，多个读者订阅一个微信公众号，一旦公众号更新，多个读者就会收到更新。js中的常见的绑定各种事件绑定（addEventListener）就是观察者模式的实现。它有两个角色组成。
        
    * 目标对象（一对多关系中的一，管理观察者的增加和删除）
    * 观察者（一对多关系中的多，通知的对象以及反映）
* 关键点：
    * 目标对象提供一系列方法 
    * 目标对象发生变化时候，调用观察者‘注册’的方法
    * 观察者提供更新接口、并把自己‘注册’到目标对象中，等待‘目标对象’通知
* 其它：    
    *  但是观察者模式是由被观察者调度的，而发布/订阅模式是统一由调度中心调的？
    *  所以观察者模式的订阅者与发布者之间是存在依赖的，而发布/订阅模式则不会？
    
```
//基本代码
class Star{
    constructor(name) {
        this.name=name;
        this.state='';
        this.observers=[];
    }
    getState() {
        return this.state;
    }
    setState(state) {
        this.state=state;
        this.notifyAllObservers();
    }
    attach(observer) {
        this.observers.push(observer);
    }
    notifyAllObservers() {
        this.observers.forEach(observer=>observer.update());
    }
}
class Fan{
    constructor(name,subject) {
        this.name=name;
        this.subject=subject;
        this.subject.attach(this);
    }
    update() {
        console.log(`${this.subject.name}有新的状态-${this.subject.getState()},${this.name}正在更新`);    
    }
}
let star=new Star('赵丽颖');
let fan1=new Fan('姜老师',star);
star.setState('结婚');
```

```
// 自定义事件
class EventEmitter{
    constructor() {
        this._events={};
    }
    on(type,listener) {
        let listeners=this._events[type];
        if (listeners) {
            listeners.push(listener);
        } else {
            this._events[type]=[listener];
        }
    }
    emit(type) {
        let listeners=this._events[type];
        let args=Array.from(arguments).slice(1);
        listeners.forEach(listener => listener(...args));
    }
}
module.exports = EventEmitter;
const EventEmitter=require('./events');
let subject=new EventEmitter();
subject.on('click',function (name) {
    console.log(1,name);
});
subject.on('click',function (name) {
    console.log(2,name);
});
subject.emit('click','zfpx'); 
// 1,zfpx
// 2,zfpx
```

```
//promise 
class Promise{
    constructor(fn) {
        this.callbacks=[];
        let resolve=() => {
            this.callbacks.forEach(callback => callback())
        };
        fn(resolve);
    }
    then(callback) {
        this.callbacks.push(callback);
    }
}
let promise=new Promise(function (resolve,reject) {
    setTimeout(function () {
        resolve(100);
    },1000);
});
promise.then(() => console.log(1));
promise.then(() => console.log(2));
```

## 十、迭代器(iterator)模式 ##
* 含义：顺序访问一个集合[array/object]，调用者无需知道集合内部具体实现。
* 场景：对于集合内部结果常常变化各异，我们不想暴露其内部结构的话，但又想让客户代码透明地访问其中的元素，这种情况下我们可以使用迭代器模式。
* es6 Iterator属性：
    原生部署了遍历接口的数据结构有：Array、Map、Set、String、TypedArray、NodeList、函数的 arguments 对象
* yield*:  yield*后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口(Iterator)。
    * 例子：
```
//yield*例子：
    let generator = function* () {
    yield 1;
    yield* [2,3,4];
    yield 5;
  };

var iterator = generator();
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next());// { value: 3, done: false }
console.log(iterator.next()); // { value: 4, done: false }
console.log(iterator.next()); // { value: 5, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

```
//基本代码1
function createIterator(arr) {
    let index=0;
    return {
        next() {
            return index<arr.length?
                {value: arr[index++],done: false}:
                {done:true}
        }
    }
}
let it=createIterator([1,2]);
console.log(it.next());
console.log(it.next());
console.log(it.next());
```

```
//基本代码二
$.each( [1, 2, 3], function( i, n ){
    console.log( '当前下标为： '+ i,'当前值为:' + n );
});
//原码实现
var each = function( ary, callback ) {
    for ( var i = 0, l = ary.length; i < l; i++ ){
        callback.call( ary[i], i, ary[ i ] ); 
        // 把下标和元素当作参数传给callback 函数
    }
};

each( [ "a", "b", "c" ], function( i, n ){
    console.log( '自定义下标为： '+ i,'自定义值为:' + n );
});

// 自定义下标为： 0 自定义值为:a  
// 自定义下标为： 1 自定义值为:b 
// 自定义下标为： 2 自定义值为:c
```

```
// es6中的原码
// array中的属性 [Symbol.Iterator]
Array.prototype[Symbol.iterator]=function () {
  let index=0;
  return {
      next:()=> {
          return index<this.length?
              {value: this[index++],done: false}:
              {done:true}
      }
  }
}
let arr=[1,2];
let it=arr[Symbol.iterator]();
console.log(it.next());
console.log(it.next());
console.log(it.next());
```

```
// 常用的上传文件功能，不同的上传方式是不一样的目前有三种上传方式，要兼容不同的浏览器。
// IE 上传控件
var getActiveUploadObj = function() {
    try {
        return new ActiveXObject("TXFTNActiveX.FTNUpload"); 
    } catch (e) {
        return false;
    }
};

// Flash 上传
var getFlashUploadObj = function() {
    if (supportFlash()) { // supportFlash 函数未提供
        var str = '<object type="application/x-shockwave-flash"></object>';
        return $(str).appendTo($('body'));
    };
    return false;
}

// 表单上传
var getFormUpladObj = function() {
    var str = '<input name="file" type="file" class="ui-file"/>'; 
    return $(str).appendTo($('body'));
};

var iteratorUpload = function() {
    for (var i = 0, fn; fn = arguments[i++];) {
        var upload = fn();
        if (upload !== false) {
            return upload;
        }
    }
};

var uploadObj = iteratorUpload( getActiveUploadObj, getFlashUploadObj, getFormUpladObj )

//如果后期增加了Webkit控件或者HTML5上传，仅仅需要增加函数和调用方法即可。
比如：
var getWebkitUploadObj = function(){ 
 // 具体代码略
}

var getHtml5UploadObj = function(){ 
 // 具体代码略
}

// 依照优先级把它们添加进迭代器
var uploadObj = iteratorUploadObj( getActiveUploadObj, getWebkitUploadObj,
getFlashUploadObj, getHtml5UploadObj, getFormUpladObj );
```

## 十一、状态（state）模式 ##
* 含义：允许一个对象在其内部状态改变的时候改变它的行为，对象看起来似乎修改了它的类。
其实就是用一个对象或者数组记录一组状态，每个状态对应一个实现，实现的时候根据状态挨个去运行实现。
* 应用场景：可以少用if...else switch,
```
//基本代码
class SuperMarry {
  constructor() {
    this._currentState = []
    this.states = {
      jump() {console.log('跳跃!')},
      move() {console.log('移动!')},
      shoot() {console.log('射击!')},
      squat() {console.log('蹲下!')}
    }
  }
  
  change(arr) {  // 更改当前动作
    this._currentState = arr
    return this
  }
  
  go() {
    console.log('触发动作')
    this._currentState.forEach(T => this.states[T] && this.states[T]())
    return this
  }
}

new SuperMarry()
    .change(['jump', 'shoot'])
    .go()                    // 触发动作  跳跃!  射击!
    .go()                    // 触发动作  跳跃!  射击!
    .change(['squat'])
    .go()                    // 触发动作  蹲下!
````

```
//react 导航
const States = {
'show':function(){
  console.log("banner显示，点击可以关闭");
  this.setState({
    currentState: "hide"
  })
},
'hide':function(){
    console.log("banner隐藏，点击可以打开");
    this.setState({
      currentState: "show"
    })
}
}
class Banner extends React.Component{
state={currentState:'show'}
toggle = ()=>{
  let s = this.state.currentState;
  States[s] && States[s].apply(this);
}
render(){
  return (
    <div>
      {this.state.currentState=='show'&&<nav>导航</nav>}
      <button onClick={this.toggle}>隐藏</button>
    </div>
  )
}
}
ReactDOM.render(<Banner/>,document.getElementById('root'));
```

## 十二、策略(strategy)模式 ##
* 含义：定义一系列的算法，把它们一个个封装起来，并且使它们可以相互替换。
* 应用场景：避免使用大量if...else,swith,表单验证，
* 和状态模式的区别：
    * 策略模式中各个类是平等的，没有关系，客户端需要知道算法主动切换，状态模式中，状态的切换和行为被封装好了，客户不需要了解细节。
    * 策略模式和状态模式都有上下文，有策略或者状态类，上下文把这些请求委托给这些类来执行

```
//把算法封装在策略对象中，指定算法调用即可
class Customer{
    constructor() {
        this.kinds={
            normal: function (price) {
                return price;
            },
            member: function (price) {
                return price*.9;
            },
            vip: function (price) {
                return price*.8;
            }
        }
    }
    cost(kind,amount) {
        return this.kinds[kind](amount);
    }
}
let c=new Customer();
console.log(c.cost('normal',100));
console.log(c.cost('member',100));
console.log(c.cost('vip',100));
```

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <form  id="userform">
      用户名 <input type="text" name="username"/>
      密码 <input type="text" name="password"/>
      手机号 <input type="text" name="mobile"/>
      <input type="submit">
    </form>
<script>
let Validator = (function(){
    let rules = {
        notEmpty(val,msg){
            if(val == ''){
                return msg;
            }
        },
        minLength(val,length,msg){
            if(val==''||val.length <length){
                return msg;
            }
        },
        maxLength(val,length,msg){
            if(val==''||val.length > length){
                return msg;
            }
        },
        isMobile(val,msg){
            if(!/^1\d{10}/.test(val)){
                return msg;
            }
        }
    }
    function addRule(name,rule){
        rules[name] = rule;
    }
    let checks = [];
    function add(element,rule){
        checks.push(function(){
            let name = rule.shift();
            rule.unshift(element.value);
            return rules[name]&&rules[name].apply(element,rule);
        });
    }
    function start(){
        for(let i=0;i<checks.length;i++){
            let check = checks[i];
            let msg = check();
            if(msg){
                return msg;
            }
        }
    }
    return {
        add,
        start,
        addRule
    }
})();

let form = document.getElementById('userform');
form.onsubmit = function(){
    Validator.add(form.username,['notEmpty','用户名不能为空']);
    Validator.add(form.password,['minLength',6,'密码小于6位最少长度']);
    Validator.add(form.password,['maxLength',8,'密码大于8位最大长度']);
    Validator.add(form.mobile,['isMobile','手机号不合法']);
    let msg =   Validator.start();
    if(msg) {
        alert(msg);
        return false;
    }
    alert('校验通过');
    return true;
}
</script>

</body>
</html>
```

## 十三、原型(prototype)模式 ##
含义：通俗点讲就是创建一个共享的原型，并通过拷贝这些原型创建新的对象。用于创建重复的对象，这种类型的设计模式属于创建型模式，它提供了一种创建对象的不错选择，
关键点：
 * 每个函数都有一个prototype属性，此属性指向原型对象，我们可以用‘.’的方式定义原型对象的属性和方法。
 * 每个对象都有一个隐藏的—proto—属性,这个属性引用了创建这个对象的函数的prototype。
  
函数和对象的关系：函数是一种对象,对象都是通过函数创建的。

原型链：访问一个对象的属性时，先在基本属性中查找，如果没有，再沿着proto这条链向上找。可以通过hasOwnProperty区分一个属性是来自于自己还是原型。
优点： 可以随意扩展、可以重写继承的方法。多个实例可以共享原型上的属性和方法
缺点: 修改原型上的一些引用属性，所有实例对应的属性也将被改变，这样可能带来一些问题
```
//基本代码
function Person(){
    this.name = name;
}
Person.prototype.getName = funciton(){
     console.log(this.name);
 }
let p1 = new Person('张三');
let p2=new Person('李四');
console.log(p1.getName===p2.getName);
 ```
 
```
// 场景：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        * {
            margin:0;
            padding:0;
        }
        canvas{
            border:1px solid #000;
        }
    </style>
</head>
<body>
    <canvas id="canvas" width="1000" height="600"></canvas>
    <script>
    //随机颜色，十六进制方法；
        function getRandomColor( ) {
          var rand = Math.floor(Math.random( ) * 0xFFFFFF).toString(16);
          if(rand.length == 6){        
            return '#'+rand;
          }else{
            return getRandomColor();
          }
        }
        let canvas = document.getElementById('canvas');
        let ctx = canvas.getContext('2d');
        let circles = [];
        function Circle(x,y,radius){
                this.x=x;
                this.y=y;
                this.radius= radius;
                circles.push(this);
        }
        Circle.prototype.update = function() {
                this.radius--;
                if(this.radius>0){
                    return true;
                }
        }
        Circle.prototype.render = function(){
            ctx.beginPath();
            ctx.arc(this.x,this.y,this.radius,0,2*Math.PI);
            ctx.fillStyle = getRandomColor();
            ctx.fill();
        }

        let circle = new Circle(0,0,30);
        canvas.onmousemove = function(event){
            circles.push(new Circle(event.clientX,event.clientY,30));
        }
        setInterval(function(){
            ctx.clearRect(0,0,1000,600);
            circles.forEach(item=>{
                item.update()&&item.render(); 
            });
            circles = circles.filter(item=>item.radius>0);
        },20);
    </script>
</body>
</html>

```

## 十四、桥接(bridging)模式 ##
含义1：将抽象部分和实现功能部分分离（解耦,弱化耦合程度），使他们可以独立变化，
含义2：需要将事物的对象和具体的行为、特征分离开来，使其可以各自独立的变化，桥接函数在中间起类似于总调控的作用。
优点：优化了代码之间的耦合，将抽象和其实现功能分离，便于二者的独立变化，让api更加健壮，有利于分层，提高组件化的模块化程度，提高可拓性。
缺点:增加开发成本，在性能方便可能会有所降低。
应用场景：事件监听，

```
//基本代码
//这个桥接模式为了让class1、class2能够独立于bridgeClass而发生改变。
var Class1 = function( a, b, c ){
    this.a = a;
    this.bc = b*c;
}
var Class2 = function( d ){
    this.d = d;
}

function BridgeClass(a, b, c, d){
    this.class1 = new Class1(a, b, c);
    this.class2 = new Class2(d);
}
```

```
//事件监听
//从逻辑上分析，把id传给getBeerById函数式合情理的，且回应结果总是通过一个毁掉函数返回。这么理解，我们现在做的是针对接口而不是实现进行编程，用桥梁模式把抽象隔离开来。
// 函数通过一个函数过滤下
 function getBeerById(id,callback){
  asyncRequest('GET','beer.url?id='+id,function(resp){
  callback(resp.responseText)
});
 }
 addEvent(element,'click',getBeerByIdBridge);
 function getBeerByIdBridge(e){
getBeerById(this.id,function(beer){　console.log('Requested Beer: '+ beer);
});
}
// express
let express=require('express');
let path=require('path');
let app=express();
app.get('/',(req,res) => {
    res.sendFile(path.join(__dirname,'4.html'));
});
app.get('/user/:id',function (req,res) {
    let id = req.params.id;
    res.json({id,name:`${id}_name`});
});
app.listen(8888);
```

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <ul>
      <li data-id="1" >1</li>
      <li data-id="2" >2</li>
    </ul>
    <p id="content">

    </p>
<script>
  let lis = document.querySelectorAll('li');
  for(let i=0;i<lis.length;i++){
      lis[i].addEventListener('click',addBridage);
  }
// 通过此桥接函数过滤下
  function addBridage(){
      let id = this.dataset.id;
      getUserById(id,function(user){
         document.getElementById('content').innerHTML = user.name;
      });
  }

  function getUserById(id,callback){
      let xhr = new XMLHttpRequest;
      xhr.open('GET',`/user/${id}`,true);
      xhr.responseType = 'json';
      xhr.onreadystatechange = function(){
          if(xhr.readyState ===4 && xhr.status == 200){
            let user = xhr.response;
            callback(user);
          }
      }
      xhr.send();
  }
</script>    
</body>
</html>
```

```
//分离多维变化
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>html</title>
    <style>
        * {
            margin:0;
            padding:0;
        }
        canvas{
            border:1px solid #000;
        }
    </style>
</head>
<body>
<canvas id="canvas" width="600" height="600"></canvas>
<script>
//形状 颜色 坐标
function Position(x,y) {
  this.x=x;
  this.y=y;
}

function Color(color) {
    this.color=color;
}

function Ball(x,y,color) {
    this.position=new Position(x,y);
    this.color=new Color(color);
}

Ball.prototype.draw=function () {
  let canvas = document.getElementById('canvas');
  let ctx = canvas.getContext('2d');
  ctx.beginPath();
  ctx.arc(this.position.x,this.position.y,100,0,2*Math.PI);
  ctx.fillStyle = this.color.color;
  ctx.fill();
}
new Ball(300,300,'red').draw();
</script>    
</body>
</html>
```
 
## 十五、组合（combination）模式 ##
* 含义：又称整体-部分模式，将一个复杂的对象组合成树形结构以表示部分-整体的层次结构，用统一的方法操作（整体和部分）。

* 其它理解：主要解决的是单一对象和组合对象在使用方式上的一致性问题
组合模式主要有三个角色：
    * 抽象组件（Component）：抽象类，主要定义了参与组合的对象的公共接口
    * 子对象（Leaf）：组成组合对象的最基本对象
    * 组合对象（Composite）：由子对象组合起来的复杂对象
* 优点：
    * 可以用同一个方法操作父对象和子对象，
    * 不必编写大量手工遍历数组或其他数据结构的粘合代码.
    * 各个对象之间非常松散,促进了代码的重用.
    * 形成了一个出色的层次体系,每当顶层组合对象执行一个操作时，实际上是在对整个结构进行深度优先的搜索以查找节点.这样查找,添加和删除都是很容易的.
* 缺点：
    * 树状结构都可以遍历（递归）.
    * 由于组合对象调用的任何操作都会被传递到它的所有子对象,如果这个层次体系很大的话,系统的性能将会受到影响.
    * 在使用组合模式时，其叶子和树枝的声明都是实现类，而不是接口，违反了依赖倒置原则
* 使用场景：
    * 具体例子：dom节点、文件夹（文件）、订单（子订单）,树形菜单，部分、整体的场景。
    * 存在一批组织成某种层次体系的对象(具体的结构在开发期间可能无法得知)
    * 希望这批对象或其中的一部分对象实施一个操作.组合模式擅长于对大批对象进行操作.它专为组织这类对象并把操作从一个层次向下一个层次传递而设计.

```
// react 基本实现
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title></title>
    <style>
    .red{
        color:red;
    }
    </style>
</head>
<body>
<div id="root"></div>    
<script>
function ReactElement(type,props) {
    this.type = type;
    this.props = props;
}
let React = {
    createElement(type,props={},...childrens){
        childrens.length===1?childrens = childrens[0]:void 0
        return new ReactElement(type,{...props,children:childrens})
    }
};
let render = (eleObj,container)=>{
    // 先取出第一层 进行创建真实dom
    let {type,props} = eleObj;
    let elementNode = document.createElement(type); // 创建第一个元素
    for(let attr in props){ // 循环所有属性
        if(attr === 'children'){ // 如果是children表示有嵌套关系
            if(typeof props[attr] == 'object'){ // 看是否是只有一个文本节点
                props[attr].forEach(item=>{ // 多个的话循环判断 如果是对象再次调用render方法
                    if(typeof item === 'object'){
                        render(item,elementNode)
                    }else{ //是文本节点 直接创建即可
                        elementNode.appendChild(document.createTextNode(item));
                    }
                })
            }else{ // 只有一个文本节点直接创建即可
                elementNode.appendChild(document.createTextNode(props[attr]));
            }
        }else if(attr === 'className'){ // 是不是class属性 class 属性特殊处理
            elementNode.setAttribute('class',props[attr]);
        }else{
            elementNode.setAttribute(attr,props[attr]);
        }
    }
    container.appendChild(elementNode)
};
//ReactDOM.render(<div>hello<span>world</span></div>);
//ReactDOM.render(React.createElement("div",null,"hello,",React.createElement("span",null,"world")));
render(React.createElement("div",null,"hello,",React.createElement("span",{class:"red"},"world")),document.getElementById('root'));

</script>
</body>
</html>
```

```
//文件夹和文件
function Folder(name) {
    this.name=name;
    this.children=[];
    this.parent=null;
    this.level=0;
}
Folder.prototype.add=function (child) {
    child.level=this.level+1;
    child.parent=this;
    this.children.push(child);
}
Folder.prototype.show=function () {
    console.log(' '.repeat(this.level)+'文件夹'+this.name);
    for (let i=0;i<this.children.length;i++){
        this.children[i].show();
    }
}
Folder.prototype.remove=function () {
    if (!this.parent) return;
    for (let i=0;i<this.parent.children.length;i++){
        let current=this.parent.children[i];
        if (current === this) {
            return this.parent.children.splice(i,1);
        }
    }
}

function File(name) {
    this.name=name;
}
File.prototype.add=function () {
    throw new Error(`文件下面不能再添加文件`);
}
File.prototype.show=function () {
    console.log(' '.repeat(this.level)+'文件'+this.name);
}

let folder=new Folder('视频');
let vueFolder=new Folder('Vue视频');
let reactFolder=new Folder('React视频');
let vueFile=new File('Vue从入门到精通');
let reactFile=new File('React从入门到精通');
folder.add(vueFolder);
folder.add(reactFolder);
vueFolder.add(vueFile);
reactFolder.add(reactFile);

folder.show();
vueFolder.remove();
folder.show();
```
## 十六、命令（command）模式 ##
* 含义：执行命令时候将发布者和执行者分开，中间加入命令对象作为中间站。有个三个角色
    * receiver(接受者角色)：干活角色，执行具体功能
    * command（命令角色）：声明所有的命令。中介者。
    * invoker(调用者角色)：发起者，接受命令并执行命令。
* 优点：行为请求者”与“行为实现者”解耦， 可以容易地实现对请求的撤销和重做
* 缺点：改模式使得我们简单的调用方法变得非常复杂和难以理解，
* 使用场景：添加、返回（撤销、重做），

```
//基本代码
class Cooker{
    cook() {
        console.log(`做饭`);    
    }
}
class Cleaner{
    clean() {
        console.log(`清洁`);    
    }
}


class CookCommand{
    constructor(cooker) {
        this.cooker=cooker;
    }
    execute() {
        this.cooker.cook();
    }
}
class CleanCommand{
    constructor(cleaner) {
        this.cleaner=cleaner;
    }
    execute() {
        this.cleaner.clean();
    }
}

class Customer{
    constructor(command) {
        this.command=command;
    }
    cook() {
        this.command.execute();
    }
    clean() {
        this.command.execute();
    }
}
let cooker=new Cooker();
let command=new CookCommand(cooker);
let c=new Customer(command);
c.cook();
let cleaner=new Cleaner();
c.command = new CleanCommand(cleaner);
c.clean();
```

```
//计数器
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>计数器</title>
</head>
<body>
    <p id="number">0</p>
    <button id="addBtn">点击按钮</button>
<script>
 let addBtn = document.getElementById('addBtn');
 let number = document.getElementById('number');

 let worker = {
  add(){
   number.innerHTML = parseInt(number.innerHTML)+1;
  }
 }

 class AddCommand{
     constructor(receiver){
         this.receiver = receiver;
     }
     execute(){
         this.receiver.add();
     }
 }

let addCommand = new AddCommand(worker);
addBtn.onclick = ()=>addCommand.execute();
</script>    
</body>
</html>
```

```
// 多步撤销和重做
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>弹出菜单</title>
</head>
<body>
    <p id="number">0</p>
    <button id="addBtn">+</button>
    <button id="undoBtn">undo</button>
    <button id="redoBtn">undo</button>
<script>
 let addBtn = document.getElementById('addBtn');
 let undoBtn = document.getElementById('undoBtn');
 let redoBtn = document.getElementById('redoBtn');
 let number = document.getElementById('number');

 let worker = {
  history:[],
  index:-1,
  add(){
   let oldVal = parseInt(number.innerHTML);     
   let newVal = oldVal + 1;
   worker.history.push(newVal);
   worker.index = worker.history.length-1;
   number.innerHTML = newVal;
   console.log(worker);
  },
  undo(){
      if(worker.index-1>=0){
          worker.index--;
        number.innerHTML = worker.history[worker.index];
        console.log(worker);
      }
  },
  redo(){
      if(worker.index+1<worker.history.length){
        worker.index++;  
        number.innerHTML = worker.history[worker.index];
        console.log(worker);
      }
  }
 }

 class AddCommand{
     constructor(receiver){
         this.receiver = receiver;
     }
     execute(){
         this.receiver.add();
     }
 }

 let addCommand = new AddCommand(worker);
 class UndoCommand{
     constructor(receiver){
         this.receiver = receiver;
     }
     execute(){
         this.receiver.undo();
     }
 }

 let undoCommand = new UndoCommand(worker);
 class RedoCommand{
     constructor(receiver){
         this.receiver = receiver;
     }
     execute(){
         this.receiver.redo();
     }
 }

let redoCommand = new RedoCommand(worker);
addBtn.onclick = ()=>addCommand.execute();
undoBtn.onclick = ()=>undoCommand.execute();
redoBtn.onclick = ()=>redoCommand.execute();
</script>    
</body>
</html>
```

## 十七、享元（flyweight）模式 ##
* 含义：共享内存、共享相同的数据，对数据和方法分离成内部数据、内部方法（可以共享）和外部数据、外部方法（不可共享）0。
* 优点：节约内存空间。
* 缺点：无
* 应用场景：共享一个对象、共享一个方法、

```
// 共享对象，传进不同的参数
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>

<input type="radio" value="red" name="color" checked/>红 
<input type="radio" value="yellow" name="color"/>黄 
<input type="radio" value="blue" name="color"/>蓝

<button onclick="draw()">绘制</button>

<div id="container">

</div>

<script>
let container = document.getElementById('container');
class MyDiv{
    constructor(){
      this.element = document.createElement('div');
    }
    setColor(color){
        this.element.style = `width:100px;height:100px;background-color:${color}`; 
    }
}
let div = new MyDiv();
function draw(){
  let color = Array.from(document.getElementsByName('color')).find(item=>item.checked).value||'red';
  div.setColor(color);
  container.appendChild(div.element);
}
</script>
</body>
</html>
```

## 十八、模板（template）模式 ##
* 含义：父类定义一个算法的骨架、将方法（步骤）的实现延迟到子类中，子类有使用权重新定义权，没有控制权
* 优点：
    * 1、封装不变部分，扩展可变部分。
    * 2、提取公共代码，便于维护。 
    * 3、行为由父类控制，子类实现。
* 缺点：
    * 每一个不同的实现都需要一个子类来实现，导致类的个数增加，使得系统更加庞大
* 使用场景:
    * 1、有多个子类共有的方法，且逻辑相同。 重要的、复杂的方法，可以考虑作为模板方法。 
    
```
//基本代码
class Person{
    dinner() {
        this.buy();
        this.cook();
        this.eat();
    }
    buy() {}
    cook() {}
    eat() {}
}
class Jiangwen extends Person{
    buy() {console.log('买黄瓜');}
    cook() {console.log('拍黄瓜');}
    eat() {console.log('吃黄瓜');}
}
let j=new Jiangwen();
j.dinner();
```

```
// 公共模板弹框

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <link rel="stylesheet" href="3.css">
</head>
<body>

<script src="3.js"></script>    
<script>
let dialog = new ConfirmDialog({
    title:'标题',
    content:'内容'
});
</script>
</body>
</html>
class Dialog{
    constructor(options) {
        this.title = options.title||'标题';
        this.content = options.content||'内容';
        this.onConfirm = options.onConfirm||this.hide;
        this.onCancel = options.onCancel||this.hide;
        this.init();
        this.initEventHandler();
    }
    init() {
        this.panel=document.createElement('div');
        this.panel.className='dialog';

        this.titleP=document.createElement('p');
        this.titleP.innerHTML=this.title;
        this.panel.appendChild(this.titleP);

        this.contentP=document.createElement('p');
        this.contentP.innerHTML=this.content;
        this.panel.appendChild(this.contentP);

        this.confirmBtn=document.createElement('button');
        this.confirmBtn.className='button confirm-button';
        this.confirmBtn.innerHTML='确定';
        this.panel.appendChild(this.confirmBtn);

        this.cancelBtn=document.createElement('button');
        this.cancelBtn.className='button cancel-button';
        this.cancelBtn.innerHTML='取消';
        this.panel.appendChild(this.cancelBtn);

        document.body.appendChild(this.panel);
    }
    inithttp://image.bubuko.com/info/201807/20180712173511381297.pngntListener('click',() => {
            this.onConfirm();
            this.hide();
        });
        this.cancelBtn.addEventListener('click',() => {
            this.onCancel();
            this.hide();
        });
    }
    show() {
        this.panel.style.display='block';
    }
    hide() {
        this.panel.style.display='none';
    }
}

class ContentDialog extends Dialog{
    init() {
        super.init();
        this.titleP.parentNode.removeChild(this.titleP);
    }
}

class ConfirmDialog extends Dialog{
    init() {
        super.init();
        this.cancelBtn.parentNode.removeChild(this.cancelBtn);
    }
}

```

## 十九、职责链（responsibility chain）模式 ##
* 含义：使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系，将这些对象连成一条链，并沿着这条链传递改请求，直到有一个对象处理它为止。
* 含义2： 一个操作可能分为多个职责角色来完成，把这些角色都分开，然后用一个链串起来，将发送者和各个处理者进行隔离。
* 优点：
    * 1、 解耦了请求发送者和接收者之间的复杂关系、只需把请求传递给第一个节点就可以。
    * 2、 链中的节点对象可以灵活的拆分重组。
    * 3、可以手动指定起始节点，
* 缺点：
    * 1、不可以保证某个请求一定会被链中的节点处理（链尾离开，抛出一个异常）
    * 2、有时候一些节点没有起到实质性的作用，仅仅是传递下了，过长得职责链带来了性能损耗。
* 应用场景：电商订单、审批流程
```
class Employee{
    constructor(next) {
        this.next=next;
    }
}
class GroupLeader extends Employee{
    audit() {
        console.log(`组长已经审批!`);
        this.next&&this.next.audit();
    }
}
class Manager extends Employee{
    audit() {
        console.log(`经理已经审批!`);
        this.next&&this.next.audit();
    }
}
class Boss extends Employee{
    audit() {
        console.log(`老板已经审批!`);
        this.next&&this.next.audit();
    }
}
let boss=new Boss();
let manager=new Manager(boss);
let groupLeader=new GroupLeader(manager);
groupLeader.audit();
// 组长已经审批
// 经理已经审批
// 老板已经审批
```

## 二十、备忘录（memorandum）模式 ##
* 含义：保存一个对象的某个状态、以便在适当的时候恢复对象，
* 意图：在不破坏封装的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，可以恢复到原先保存的状态。
* 优点：
    * 1、可以恢复的状态机制、可以回到历史状态
    * 2、信息的封装、不需要关心状态的保存细节。
* 缺点：
    * 1、消耗资源、消耗内存。 
* 应用场景：后悔了、存档、Ctrl+z、IE后退、回滚、
* 注意事项：需要增加一个管理备忘录的类，为了节约内存、可使用原型模式+备忘录模式。

```
    <input type="text" id="content">
    <button id="save-btn">保存</button>
    <button id="prev-btn">上一步</button>
    <button id="next-btn">下一步</button>
    <script>
      let content = document.getElementById('content');
      let saveBtn = document.getElementById('save-btn');
      let prevBtn = document.getElementById('prev-btn');
      let nextBtn = document.getElementById('next-btn');
      class Memo{
          constructor(content){
              this.content = content;
          }
      }
      class Memos {
          constructor(){
              this.index = 0;
              this.list = [new Memo('')];
          }
          add(content){
              this.list[++this.index] = new Memo(content);
          }
          get(){
              return this.list[this.index];
          }
          prev(){
              if(this.index ==0) return alert('没有上一步');
              return this.list[--this.index];
          }
          next(){
               if(this.index ==this.list.length-1) return alert('没有下一步');
              return this.list[++this.index];
          }
      }
      let memos = new Memos();
      saveBtn.addEventListener('click',function(){
          memos.add(content.value);
      });
      prevBtn.addEventListener('click',function(){
          let memo = memos.prev();
          content.value = memo.content;
      });
      nextBtn.addEventListener('click',function(){
          let memo = memos.next();
          content.value = memo.content;
      });
    </script>
```

## 二十一、中介者(intermediary)模式 ##
* 含义：用一个中介对象来封装系列的对象交互，中介者使各个对象不需要显示的互相引用，从而使其耦合松散，而且可以独立的改变它们之间的交互。目的是解耦。
* 优点：
* 缺点：会增加一个巨大的对象，中介者中回包含大量的逻辑、维护难度大，
* 和观察者模式的区别：
    * 1、中介者不是简单的分发、包括维护这些约束的职责。
    * 2、观察者和具体类是一起配合来维护约束的，沟通是双方交互的，互相包含。
* 和外观者模式的区别：
    * 1、 中介者所做的是在模块之间进行的通信、是多向的。
    * 2、对外暴露简单的接口（不添加额外的功能）
* 应用场景：
* 其它：中介者模式把交互复杂性变成了中介者本身的复杂性，中介者对象会比其它任何对象都复杂。

```
class Boy{
    constructor(mediator) {
        this.mediator=mediator;
        this.salary=1000;
    }
    learnGirl() {
        this.mediator.learnGirl();
    }
}
class Girl{
    constructor(mediator) {
        this.mediator=mediator;
        this.age=28;
    }
    learnBoy() {
        this.mediator.learnBoy();
    }
}
class Mediator{
    constructor(boy,girl) {
        this.boy=boy;
        this.girl=girl;
    }
    learnBoy() {
        console.log(`这个男孩月薪${this.boy.salary*10}`);
    }
    learnGirl() {
        console.log(`这个女孩年龄${this.girl.age-10}`);
    }
}
let boy=new Boy();
let girl=new Girl();
let mediator=new Mediator(boy,girl);
boy.mediator=mediator;
girl.mediator=mediator;
boy.learnGirl(); // 这个女孩年龄18
girl.learnBoy(); // 这个男孩月薪10000



```
## 二十二、访问者（visitor）模式 ##
* 含义：将数据操作和数据结构进行分离，有某个操作作用于一些元素之上，而这些元素属于某一个对象结构
* 组成部分：
    * ObjectStructure：定义当中所说的对象结构，对象结构是一个抽象表述，它内部管理了元素集合，并且可以迭代这些元素供访问者访问。
    * Element：元素接口或者抽象类，它定义了一个接受访问者的方法（Accept），其意义是指每一个元素都要可以被访问者访问。
    * ConcreteElementA、ConcreteElementB：具体的元素类，它提供接受访问方法的具体实现，而这个具体的实现，通常情况下是使用访问者提供的访问该元素类的方法。
    * Visitor：接口或者抽象类，它定义了对每一个元素（Element）访问的行为，它的参数就是可以访问的元素，它的方法数理论上来讲与元素个数是一样的，因此，访问者模式要求元素的类族要稳定，如果经常添加、移除元素类，必然会导致频繁地修改Visitor接口，如果这样则不适合使用访问者模式。
    * 
* 优点：符合单一职责原则、扩展性良好。
* 缺点：
* 应用场景：
* 其它注意事项：

```
let babel = require('babel-core');
let t=require('babel-types');
let preCalculator={
    visitor: {
        BinaryExpression(path) {
            let node=path.node;
            console.log(node.operator);
        }
    }
}


const result = babel.transform('const sum = 1+2',{
    plugins:[
        preCalculator
    ]
});
//console.log(result.code);

```
```
class Father{
    constructor(name,wealth) {
        this.name=name;
        this.wealth=wealth;
    }
    accept(viewer) {
        viewer.viewFather(this);
    }
}
class Mother{
    constructor(name,character) {
        this.name=name;
        this.character=character;
    }
    accept(viewer) {
        viewer.viewMother(this);
    }
}
class Son{
    constructor(name,look) {
        this.name=name;
        this.look=look;
    }
    accept(viewer) {
        viewer.viewSon(this);
    }
}
class Family{
    constructor(father,mother,son) {
        this.father=father;
        this.mother=mother
        this.son=son;
    }
    view(viewer) {
        this.father.accept(viewer);
        this.mother.accept(viewer);
        this.son.accept(viewer);
    }
}

class Girl{
    constructor(name) {
        this.name=name;
    }
    viewFather(father) {
        console.log(`${this.name} ${father.name} 的财富 ${father.wealth}`);        
    }
    viewMother(mother) {
        console.log(`${this.name} ${mother.name} 的性格 ${mother.character}`);        
    }
    viewSon(son) {
        console.log(`${this.name} ${son.name} 的相貌 ${son.look}`);        
    }
}
let father=new Father('冯爸爸',999999);
let mother=new Mother('冯妈妈','温柔');
let son=new Son('冯绍峰','帅');
let family=new Family(father,mother,son);
let zhaoliying=new Girl('赵丽颖');
family.view(zhaoliying);
```

## 二十三、解释器（interpreter）模式 ##
* 含义：是定义语言的文法，并且建立一个解释器来解释该语言中的句子。解释器模式描述了如何构成一个简单的语言解释器，主要应用在使用面向对象语言开发的编译器中。它描述了如何为简单的语言定义一个文法，如何在该语言中表示一个句子，以及如何解释这些句子。在解释器模式中除了能够使用文法规则来定义一个语言，还有通过一个更加直观的方法来表示——使用抽象语法树。抽象语法树能够更好地，更直观地表示一个语言的构成，每一颗抽象语法树对应一个语言实例。
* 优点：
    *  1、 可扩展性比较好，灵活。

    *  2、 增加了新的解释表达式的方式。

    *  3、 易于实现文法。
* 缺点：
    * 1、执行效率比较低，可利用场景比较少。
    * 2、对于复杂的文法比较难维护
* 使用场景：
    * 1、可以将一个需要解释执行的语言中的句子表示为一个抽象语法树
    * 2、一些重复出现的问题可以用一种简单的语言来进行表达。
    * 3、文法较为简单
* 其它： 
    * 1、在解释器模式中由于语法是由很多类表示的，所以可扩展性强。 
    * 2、虽然解释器的可扩展性强，但是如果语法规则的数目太大的时候，该模式可能就会变得异常复杂。所以解释器模式适用于文法较为简单的。
    * 3、 解释器模式可以处理脚本语言和编程语言。常用于解决某一特定类型的问题频繁发生情况。
