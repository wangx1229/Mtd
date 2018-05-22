## 这是一个置顶的栗子，我们来创建一个函数，其实它就是JS中的类

``` javascript
function foo(name) {
  this.name = name;
}
foo.prototype.say = function() {
  return this.name
}
const obj = new foo('Yi');
```
至于为什么不把say属性也写到foo中，而是写在foo.prototype上。是因为，每次构造foo的时候都会重新生成一次say属性，而这并不是我们想要的，我们只需要重新生成name属性就可以了，所以foo中的方法通常写在foo.prototype上面，以避免不必要的工作。
## 概念解析
首先让我们来看看一些书中的解释
1. 通过new调用foo函数，会生成一个全新的对象。
2. 如果函数没有返回其他对象，那么函数就会返回这个新对象。
3. 这个新对象会绑定到函数中的this。
4. 这个新对象会被执行会被执行prototype链接。

如果你也是第一次看到这些，相信绝大多数情况和我当时的反应一样，“这**说的什么东西” 。确实，单看这些也许你并不能得到你想要的结果，所以接下来我们拿上面的栗子进行分析一哈。
## 实例分析
首先我们创建了一个对象。这个时候刚巧我们的函数并没有返回任何对象，因此默认返回了我们创建的这个新对象，并将这个对象命名为了obj。

接下来，让我们看看foo函数还做了些什么。

当我们调用这个foo函数的时候，第三点告诉我们，我们创建的这个新对象，会把它绑定到this上。因此函数中的this.name其实就是obj.name,被设置成我们函数的参数'Yi'

这个时候我们得到的obj对象就成了obj = {name: 'Yi'}。

等等，这还没完，别忘了我们还有一个foo.prototype.say的属性。最后一步，我们创建的这个obj他的原型对象会关联到foo.prototype上，因此obj.__proto__就指向了foo.prototype，所以obj就拥有了say属性，不过他其实是从原型链中使用的，你可以通过Object.getOwnPropertyNames()来查看obj自身的属性和obj的原型对象的属性
```javascript
Object.getOwnPropertyNames(obj)
// 你会得到["name"]
Object.getOwnPropertyNames(obj.__proto__)
// 你会得到["constructor", "say"]
```
如果你仔细观察会发现obj.__proto__上除了say之外还有一个constructor属性，他其实是使用new foo('Yi')时，默认创建的一个属性，值就是foo。因此，obj.__proto__.constructor === 'foo'。而obj.constructor其实是通过obj.__proto__使用的constructor属性。接下来我们对比一下ES6中的类的声明。

## ES6声明类
``` javascript
// ES6的写法                                            // ES5的写法
class foo {                                            function foo(name) {
  constructor(name) {                                     this.name = name;
    this.name = name;                                  }
  }                                                    foo.prototype.say = function() { 
  say() {                                                 return this.name
    return this.name;                                  }
  }                                                    const obj = new foo('Yi');
}
const obj = new foo('Yi');                             
```
为什么new foo('Yi')叫做构造函数，其实从ES6类的声明中更容易理解，或许这有些本末倒置。

new foo('Yi')其实是调用了ES6中foo类的constructor的方法，而我们在ES5中对this.name的赋值，其实都是在ES6中foo类的constructor函数里面实现的。

ES5中通过foo.prototype添加的属性，可以直接在ES6的foo类中声明，简洁了很多。
