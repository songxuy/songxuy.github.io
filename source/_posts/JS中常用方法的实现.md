title: JS中常用方法的实现
author: coolsong
tags:
  - JavaScript
categories:
  - JavaScript
date: 2019-11-22 23:22:00
---
&nbsp;&nbsp;&nbsp;对于一些我们在工作中经常使用的方法，我们可能已经对它的用法、参数、返回等非常清楚，甚至了如指掌。但是对于其具体的实现可能就只知道个大概，要自己完全实现还是会有一定的困难，但是这个思考的过程也是我们进步和提升很重要的一步，下面就和大家一起思考着来实现一些常用的方法：
<!--more-->
### bind，apply和call
>这三个函数的作用都是改变this的指向，不同点 call、apply 是直接调用函数，bind 是返回一个新的函数。call 跟 apply 就只有参数上不同。

#### bind
>bind的基本用法就是传递一个作用域及参数 返回一个函数
```JavaScript
Function.prototype.bind(context, args)
```
* 实现
>1.首先判断调用者是否为函数
>2.将传递的数据参数取出来
>3.创建一个干净的函数，用于保存原函数的原型
>4.构造绑定的函数 this instanceof nop, 判断是否使用 new 来调用 bound， 如果是 new 来调用的话，this的指向就是其实例，如果不是 new 调用的话，就改变 this 指向到指定的对象
>5.然后再判断是否为箭头函数，箭头函数没有 prototype，箭头函数this永远指向它所在的作用域
>6. 最后修改绑定函数的原型指向
```JavaScript
Function.prototype.mybind = function (context) {
    if (typeof this !== 'function'){
      throw TypeError("Bind must be called on a function");
    }
    let args = Array.prototype.slice.call(arguments, 1)
    let self = this
    let nop = function () {}
    let result = function () {
      return self.apply(this instanceof nop ? this : context, args.concat(Array.prototype.slice.call(arguments)))
    }
    if (this.prototype) {
      nop.prototype = this.prototype
    }
    result.prototype = new nop()
    return result
  }
```
#### call
>call的原理是将函数在给定作用域和给定参数上执行并返回相应的结果
```JavaScript
Function.prototype.call(context,arg1,arg2...)
```
*实现

>1. 取出数据参数部分
>2. 创建一个唯一标识，避免调用函数上存在同名函数导致报错
>3. 将调用作用域的标识函数赋值为传入的作用域（未传则默认window）
>4. 在传入作用域调用该函数
>5. 删除相应的变量并返回结果
```JavaScript
Function.prototype.mycall = function(context) {
    if (typeof this !== 'function'){
      throw TypeError("Bind must be called on a function");
    }
    let args = Array.prototype.slice.call(arguments, 1)
    let fn = Symbol('fn')
    context = context || window
    context[fn] = this
    const result = context[fn](...args)
    delete context[fn]
    return result
  }
```
#### apply
> 和call的实现基本上完全一样就不再赘述了，直接上代码吧

```JavaScript
Function.prototype.myapply = function(context) {
    if (typeof this !== 'function'){
      throw TypeError("Bind must be called on a function");
    }
    let fn = Symbol('fn')
    context = context || window
    context[fn] = this
    const result = context[fn](arguments[1])
    delete context[fn]
    return result
  }
```

### reduce的实现
>reduce是数组中的一个累加方法，通过传递一个函数和初始值（可不传），得到一个累加值返回。函数中又包含了4个参数，分别是当前累计值、下一个值、索引、数组
```JavaScript
array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
```
* 实现
```JavaScript
Array.prototype.myreduce = function(func) {
    let arr = this, len = this.length, k = 0, status
    let sum = undefined, initvalue = arguments.length > 1 ? arguments[1] : undefined
    if (typeof func !== 'function') {
      throw new TypeError(func + ' is not a function');
    }

    // 数组为空，并且有初始值，报错
    if (len === 0 && arguments.length < 2) {
      throw new TypeError('Reduce of empty array with no initial value');
    }
    if (initvalue !== undefined) {
      sum = initvalue
    } else {
      sum = arr[k]
      k++
    }
    while(k<len) {
      status = arr.hasOwnProperty(k)
      if (status) {
        const value = arr[k]
        sum = func.apply(undefined, [sum, value, k, arr])
      }
      k++
    }
    return sum
  }
```

### new
> new之后的操作主要是
> 1. 创建一个新对象
> 2. 将构造函数的作用域赋给新对象
> 3. 执行构造函数中的代码，为新对象添加属性
> 4. 返回新对象

* 实现 ES5
```JavaScript
function myNew() {
  // 创建一个实例对象
  var obj = new Object();
  // 取得外部传入的构造器
  var Constructor = Array.prototype.shift.call(arguments);
  // 实现继承，实例可以访问构造器的属性
  obj.__proto__ = Constructor.prototype;
  // 调用构造器，并改变其 this 指向到实例
  var ret = Constructor.apply(obj, arguments);
  // 如果构造函数返回值是对象则返回这个对象，如果不是对象则返回新的实例对象
  return typeof ret === 'object' && ret !== null ? ret : obj;
}
```
* 实现ES6
```JavaScript
function muNew (fn,...args) {
    let obj = Object.create(fn.prototype)
    const res = fn.call(obj, args)
    return res instanceof Object ? res : obj
  }
```
### Object.create
>Object.create主要是创建一个新对象，使用现有的对象来提供新创建的对象的proto
```JavaScript
Object.create(proto, [propertiesObject])
/*
proto : 必须。表示新建对象的原型对象，即该参数会被赋值到目标对象(即新对象，或说是最后返回的对象)的原型上。该参数可以是null， 对象， 函数的prototype属性 （创建空的对象时需传null , 否则会抛出TypeError异常）。propertiesObject : 可选。 添加到新创建对象的可枚举属性（即其自身的属性，而不是原型链上的枚举属性）对象的属性描述符以及相应的属性名称。这些属性对应Object.defineProperties()的第二个参数。
*/
```
* 实现
```JavaScript
Object.create1 = function (prototype, properties) {
    if (typeof prototype !== "object") 
    { 
        throw TypeError();
    }
    function Func() {}
    Func.prototype = prototype;
    var o = new Func();
    if (prototype) { o.constructor = Func; }
    if (properties !== undefined) {
      if (properties !== Object(properties)) { throw TypeError(); }
      Object.defineProperties(o, properties);
    }
    return o;
  };
```

### instanceof
> 主要是测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性
> 和它一样经常使用的typeof 则是来判断数据是什么类型的（返回值主要是
"number"、"string"、"boolean"、"object"、"function" 和 "undefined"）
* 实现
```JavaScript
function instance_of (L , R) {
  let b = R.prototype
  let l = L.__proto__
  while(true) {
    if (l === null) {
      return false
    }
    if (l === b) {
      return true
    }
    l = l.__proto__
  }
}
```

### Array.isArray
>Array中的一个方法，判断传入值是否为一个数组 实现起来也比较的简单就是一个通用类型的判断
* 实现
```JavaScript
Array.myisarray = function (o) {
  return Object.prototype.toString.call(o) === '[object Array]'
}
```

### getOwnPropertyNames
>Object.getOwnPropertyNames()方法返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括Symbol值作为名称的属性）组成的数组。
>和它有类型作用的还有for in（会遍历自身及原型链上的可枚举属性）和Object.keys(对象自身的可枚举属性的key输出)

* 实现
```JavaScript
Object.getOwnPropertyNames1 = function (obj) {
  if (obj !== Object(obj)) {
    throw TypeError('Object.getOwnPropertyNames called on non-object');
  }
  let arr= [], p;
  for (p in obj) {
    if (Object.prototype.hasOwnProperty.call(o, p)) {
      arr.push(p)
    }
  }
  return arr
}
```

### Promise
>Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理且更强大。它最早由社区提出并实现，ES6将其写进了语言标准，统一了用法，并原生提供了Promise对象。

>Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由JavaScript引擎提供，不用自己部署。resolve作用是将Promise对象状态由“未完成”变为“成功”，也就是Pending -> Fulfilled，在异步操作成功时调用，并将异步操作的结果作为参数传递出去；而reject函数则是将Promise对象状态由“未完成”变为“失败”，也就是Pending -> Rejected，在异步操作失败时调用，并将异步操作的结果作为参数传递出去

>除此之外，其中还包括了很多的方法如Promise.all、Promise.race、Promise.resolve、Promise.reject等

>这个实现起来会比之前的复杂一点，我们可以先列出一些需要实现的点，并且实现使用ts

* 大概构成
```TypeScript
class selfPromise {
  constructor(handle: Function) {
    try  {
      handle(this._resolve, this._reject);
    } catch (err) {
      this._reject(err)
    }
  }
  private _status: string = 'pending'
  
  private _resolve = val => {

  }
  private _reject = val => {
  }
  then(onFulfilled?, onRejected?) {
  }
  catch () {}
  finally() {}
  static resolve () {}
  static reject () {
  }
  static all () {
  }
  static race () {}
}
```
* 具体实现(TS版)
```TypeScript
const isFunction = variable => typeof variable === 'function';

// 定义Promise的三种状态常量
const PENDING = 'pending';
const FULFILLED = 'fulfilled';
const REJECTED = 'rejected';

class MyPromise {
  // 构造函数，new 时触发
  constructor(handle: Function) {
    try {
      handle(this._resolve, this._reject);
    } catch (err) {
      this._reject(err);
    }
  }
  // 状态 pending fulfilled rejected
  private _status: string = PENDING;
  // 储存 value，用于 then 返回
  private _value: string | undefined = undefined;
  // 失败队列，在 then 时注入，resolve 时触发
  private _rejectedQueues: any = [];
  // 成功队列，在 then 时注入，resolve 时触发
  private _fulfilledQueues: any = [];
  // resovle 时执行的函数
  private _resolve = val => {
    const run = () => {
      if (this._status !== PENDING) return;
      this._status = FULFILLED;
      // 依次执行成功队列中的函数，并清空队列
      const runFulfilled = value => {
        let cb;
        while ((cb = this._fulfilledQueues.shift())) {
          cb(value);
        }
      };
      // 依次执行失败队列中的函数，并清空队列
      const runRejected = error => {
        let cb; 
        while ((cb = this._rejectedQueues.shift())) {
          cb(error);
        }
      };
      /*
       * 如果resolve的参数为Promise对象，
       * 则必须等待该Promise对象状态改变后当前Promsie的状态才会改变
       * 且状态取决于参数Promsie对象的状态
       */
      if (val instanceof MyPromise) {
        val.then(
          value => {
            this._value = value;
            runFulfilled(value);
          },
          err => {
            this._value = err;
            runRejected(err);
          }
        );
      } else {
        this._value = val;
        runFulfilled(val);
      }
    };
    // 异步调用
    setTimeout(run);
  };
  // reject 时执行的函数
  private _reject = err => {
    if (this._status !== PENDING) return;
    // 依次执行失败队列中的函数，并清空队列
    const run = () => {
      this._status = REJECTED;
      this._value = err;
      let cb;
      while ((cb = this._rejectedQueues.shift())) {
        cb(err);
      }
    };
    // 为了支持同步的Promise，这里采用异步调用
    setTimeout(run);
  };
  // then 方法
  then(onFulfilled?, onRejected?) {
    const { _value, _status } = this;
    // 返回一个新的Promise对象
    return new MyPromise((onFulfilledNext, onRejectedNext) => {
      // 封装一个成功时执行的函数
      const fulfilled = value => {
        try {
          if (!isFunction(onFulfilled)) {
            onFulfilledNext(value);
          } else {
            const res = onFulfilled(value);
            if (res instanceof MyPromise) {
              // 如果当前回调函数返回MyPromise对象，必须等待其状态改变后在执行下一个回调
              res.then(onFulfilledNext, onRejectedNext);
            } else {
              //否则会将返回结果直接作为参数，传入下一个then的回调函数，并立即执行下一个then的回调函数
              onFulfilledNext(res);
            }
          }
        } catch (err) {
          // 如果函数执行出错，新的Promise对象的状态为失败
          onRejectedNext(err);
        }
      };

      // 封装一个失败时执行的函数
      const rejected = error => {
        try {
          if (!isFunction(onRejected)) {
            onRejectedNext(error);
          } else {
            const res = onRejected(error);
            if (res instanceof MyPromise) {
              // 如果当前回调函数返回MyPromise对象，必须等待其状态改变后在执行下一个回调
              res.then(onFulfilledNext, onRejectedNext);
            } else {
              //否则会将返回结果直接作为参数，传入下一个then的回调函数，并立即执行下一个then的回调函数
              onFulfilledNext(res);
            }
          }
        } catch (err) {
          // 如果函数执行出错，新的Promise对象的状态为失败
          onRejectedNext(err);
        }
      };

      switch (_status) {
        // 当状态为pending时，将then方法回调函数加入执行队列等待执行
        case PENDING:
          this._fulfilledQueues.push(fulfilled);
          this._rejectedQueues.push(rejected);
          break;
        // 当状态已经改变时，立即执行对应的回调函数
        case FULFILLED:
          fulfilled(_value);
          break;
        case REJECTED:
          rejected(_value);
          break;
      }
    });
  }
  // catch 方法
  catch(onRejected) {
    return this.then(undefined, onRejected);
  }
  // finally 方法
  finally(cb) {
    return this.then(
      value => MyPromise.resolve(cb()).then(() => value),
      reason =>
        MyPromise.resolve(cb()).then(() => {
          throw reason;
        })
    );
  }
  // 静态 resolve 方法
  static resolve(value) {
    // 如果参数是MyPromise实例，直接返回这个实例
    if (value instanceof MyPromise) return value;
    return new MyPromise(resolve => resolve(value));
  }
  // 静态 reject 方法
  static reject(value) {
    return new MyPromise((resolve, reject) => reject(value));
  }
  // 静态 all 方法
  static all(list) {
    return new MyPromise((resolve, reject) => {
      // 返回值的集合
      let values = [];
      let count = 0;
      for (let [i, p] of list.entries()) {
        // 数组参数如果不是MyPromise实例，先调用MyPromise.resolve
        this.resolve(p).then(
          res => {
            values[i] = res;
            count++;
            // 所有状态都变成fulfilled时返回的MyPromise状态就变成fulfilled
            if (count === list.length) resolve(values);
          },
          err => {
            // 有一个被rejected时返回的MyPromise状态就变成rejected
            reject(err);
          }
        );
      }
    });
  }
  // 添加静态race方法
  static race(list) {
    return new MyPromise((resolve, reject) => {
      for (let p of list) {
        // 只要有一个实例率先改变状态，新的MyPromise的状态就跟着改变
        this.resolve(p).then(
          res => {
            resolve(res);
          },
          err => {
            reject(err);
          }
        );
      }
    });
  }
}
```
* 具体实现（JavaScript）
```JavaScript
var isFunction = function (variable) { return typeof variable === 'function'; };
// 定义Promise的三种状态常量
var PENDING = 'pending';
var FULFILLED = 'fulfilled';
var REJECTED = 'rejected';
var MyPromise = /** @class */ (function () {
    // 构造函数，new 时触发
    function MyPromise(handle) {
        var _this = this;
        // 状态 pending fulfilled rejected
        this._status = PENDING;
        // 储存 value，用于 then 返回
        this._value = undefined;
        // 失败队列，在 then 时注入，resolve 时触发
        this._rejectedQueues = [];
        // 成功队列，在 then 时注入，resolve 时触发
        this._fulfilledQueues = [];
        // resovle 时执行的函数
        this._resolve = function (val) {
            var run = function () {
                if (_this._status !== PENDING)
                    return;
                _this._status = FULFILLED;
                // 依次执行成功队列中的函数，并清空队列
                var runFulfilled = function (value) {
                    var cb;
                    while ((cb = _this._fulfilledQueues.shift())) {
                        cb(value);
                    }
                };
                // 依次执行失败队列中的函数，并清空队列
                var runRejected = function (error) {
                    var cb;
                    while ((cb = _this._rejectedQueues.shift())) {
                        cb(error);
                    }
                };
                /*
                 * 如果resolve的参数为Promise对象，
                 * 则必须等待该Promise对象状态改变后当前Promsie的状态才会改变
                 * 且状态取决于参数Promsie对象的状态
                 */
                if (val instanceof MyPromise) {
                    val.then(function (value) {
                        _this._value = value;
                        runFulfilled(value);
                    }, function (err) {
                        _this._value = err;
                        runRejected(err);
                    });
                }
                else {
                    _this._value = val;
                    runFulfilled(val);
                }
            };
            // 异步调用
            setTimeout(run);
        };
        // reject 时执行的函数
        this._reject = function (err) {
            if (_this._status !== PENDING)
                return;
            // 依次执行失败队列中的函数，并清空队列
            var run = function () {
                _this._status = REJECTED;
                _this._value = err;
                var cb;
                while ((cb = _this._rejectedQueues.shift())) {
                    cb(err);
                }
            };
            // 为了支持同步的Promise，这里采用异步调用
            setTimeout(run);
        };
        try {
            handle(this._resolve, this._reject);
        }
        catch (err) {
            this._reject(err);
        }
    }
    // then 方法
    MyPromise.prototype.then = function (onFulfilled, onRejected) {
        var _this = this;
        var _a = this, _value = _a._value, _status = _a._status;
        // 返回一个新的Promise对象
        return new MyPromise(function (onFulfilledNext, onRejectedNext) {
            // 封装一个成功时执行的函数
            var fulfilled = function (value) {
                try {
                    if (!isFunction(onFulfilled)) {
                        onFulfilledNext(value);
                    }
                    else {
                        var res = onFulfilled(value);
                        if (res instanceof MyPromise) {
                            // 如果当前回调函数返回MyPromise对象，必须等待其状态改变后在执行下一个回调
                            res.then(onFulfilledNext, onRejectedNext);
                        }
                        else {
                            //否则会将返回结果直接作为参数，传入下一个then的回调函数，并立即执行下一个then的回调函数
                            onFulfilledNext(res);
                        }
                    }
                }
                catch (err) {
                    // 如果函数执行出错，新的Promise对象的状态为失败
                    onRejectedNext(err);
                }
            };
            // 封装一个失败时执行的函数
            var rejected = function (error) {
                try {
                    if (!isFunction(onRejected)) {
                        onRejectedNext(error);
                    }
                    else {
                        var res = onRejected(error);
                        if (res instanceof MyPromise) {
                            // 如果当前回调函数返回MyPromise对象，必须等待其状态改变后在执行下一个回调
                            res.then(onFulfilledNext, onRejectedNext);
                        }
                        else {
                            //否则会将返回结果直接作为参数，传入下一个then的回调函数，并立即执行下一个then的回调函数
                            onFulfilledNext(res);
                        }
                    }
                }
                catch (err) {
                    // 如果函数执行出错，新的Promise对象的状态为失败
                    onRejectedNext(err);
                }
            };
            switch (_status) {
                // 当状态为pending时，将then方法回调函数加入执行队列等待执行
                case PENDING:
                    _this._fulfilledQueues.push(fulfilled);
                    _this._rejectedQueues.push(rejected);
                    break;
                // 当状态已经改变时，立即执行对应的回调函数
                case FULFILLED:
                    fulfilled(_value);
                    break;
                case REJECTED:
                    rejected(_value);
                    break;
            }
        });
    };
    // catch 方法
    MyPromise.prototype["catch"] = function (onRejected) {
        return this.then(undefined, onRejected);
    };
    // finally 方法
    MyPromise.prototype["finally"] = function (cb) {
        return this.then(function (value) { return MyPromise.resolve(cb()).then(function () { return value; }); }, function (reason) {
            return MyPromise.resolve(cb()).then(function () {
                throw reason;
            });
        });
    };
    // 静态 resolve 方法
    MyPromise.resolve = function (value) {
        // 如果参数是MyPromise实例，直接返回这个实例
        if (value instanceof MyPromise)
            return value;
        return new MyPromise(function (resolve) { return resolve(value); });
    };
    // 静态 reject 方法
    MyPromise.reject = function (value) {
        return new MyPromise(function (resolve, reject) { return reject(value); });
    };
    // 静态 all 方法
    MyPromise.all = function (list) {
        var _this = this;
        return new MyPromise(function (resolve, reject) {
            // 返回值的集合
            var values = [];
            var count = 0;
            var _loop_1 = function (i, p) {
                // 数组参数如果不是MyPromise实例，先调用MyPromise.resolve
                _this.resolve(p).then(function (res) {
                    values[i] = res;
                    count++;
                    // 所有状态都变成fulfilled时返回的MyPromise状态就变成fulfilled
                    if (count === list.length)
                        resolve(values);
                }, function (err) {
                    // 有一个被rejected时返回的MyPromise状态就变成rejected
                    reject(err);
                });
            };
            for (var _i = 0, _a = list.entries(); _i < _a.length; _i++) {
                var _b = _a[_i], i = _b[0], p = _b[1];
                _loop_1(i, p);
            }
        });
    };
    // 添加静态race方法
    MyPromise.race = function (list) {
        var _this = this;
        return new MyPromise(function (resolve, reject) {
            for (var _i = 0, list_1 = list; _i < list_1.length; _i++) {
                var p = list_1[_i];
                // 只要有一个实例率先改变状态，新的MyPromise的状态就跟着改变
                _this.resolve(p).then(function (res) {
                    resolve(res);
                }, function (err) {
                    reject(err);
                });
            }
        });
    };
    return MyPromise;
}());
```