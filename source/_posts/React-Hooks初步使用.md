title: React Hooks初步使用
author: coolsong
date: 2020-09-10 20:59:29
tags:
  - React
  - Hooks
categories:
  - React
  - Hooks
---
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Hooks这个特性也出来很久了，在React16.8中新增的。自己之前的话也只是简单的了解过，并没有实际的去使用过。最近重新来过了一遍，下面是对一些基本的hooks的介绍。
<!--more-->
#### 介绍

>React Hooks的作用主要是它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。
>
>它的出现，表明着react中不会再存在无状态组件了，只有类组件和函数组件。那为什么要提倡使用函数式组件呢，下面看一下简单的对比。

#### 对比
* 我们先来看看Class写法的一些缺点（网上总结的）以及解决方法：

| 问题 | 解决方案 | 缺点 |
| --- | --- | --- |
| 生命周期臃肿、逻辑耦合 | 无 |  |
| 逻辑难以复用 | 通过继承解决 | 不支持多继承 |
|  | 通过hoc解决 | 会增加额外的组件嵌套，也会有一些性能影响 |
|  | 渲染属性 | 同上层级臃肿、性能影响 |
| class this 指向问题 | 匿名函数  | 每次都创建新的函数，子组件重复不必要渲染 |
|  | bind | 需要写很多跟逻辑、状态无关的代码 |

* Hooks的解决方案

1. 没有了class就没有this的指向问题
2. 通过自定义useEffect来解决复用问题
3. 通过使用useEffect来细分逻辑，减少逻辑臃肿场景

#### 具体使用
> 下面通过介绍一些基本的hooks来了解其基本使用

##### useState

* useState的作用就是让我们能在函数组件中使用state，下面是官网给的例子：

###### 官网例子

```JavaScript
import React, { useState } from 'react';

function Example() {
  // 声明一个叫 "count" 的 state 变量
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

>useState调用后会返回一个数组，第一个就是state，第二个是一个函数用来修改state的。使用es6解构赋值名字可以自己随便起，但是为了规范和统一，第二个参数一般为set开头。

###### 修改相同值
* 当我们修改时传入相同值，组件会重新渲染么？答案肯定是否
```JavaScript
import React, { useState } from 'react';

function Example() {
  // 声明一个叫 "count" 的 state 变量
  const [count, setCount] = useState(0);
  console.log('hello, wolrd')
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count)}>
        Click me
      </button>
    </div>
  );
}
```
###### 关于默认值

* useState 支持我们在调用的时候直接传入一个值，来指定 state 的默认值，比如这样 useState(0), useState({ a: 1 }), useState([ 1, 2 ])，还支持我们传入一个函数，来通过逻辑计算出默认值，比如这样：

```JavaScript
import React, { useState } from 'react';

function Example(props) {
  // 声明一个叫 "count" 的 state 变量
  const [count, setCount] = useState(() => {
    return props.count || 0
  });
  console.log('hello, wolrd')
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
>同时不用担心useState多次执行的问题，只会执行一次

###### 多个 useState
* 当我们使用多个useState的时候，react是如何识别哪个是哪个的呢？其实就是根据第一次的执行顺序来的，也是因为这个原因不要在循环、条件或者嵌套函数中调用hooks。

##### useEffect
* Effect Hook 可以让你在函数组件中执行副作用操作，这里的副作用操主要是指的网络请求，监听事件，查找 dom等

###### 官网例子
```JavaScript
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
>运行之后我们可以发现，useEffect中的函数再开始执行的时候回执行一次，当我们setCount之后，又会执行。感觉有点像之前的componentDidMount和componentDidUpdate。下面我们就来看看useEffect生命周期

###### useEffect生命周期
* 总的来说useEffect Hook包含了componentDidMount、componentDidUpdate 和 componentWillUnmount 这三个函数。以往我们在绑定事件、解绑事件、设定定时器、查找 dom 的时候，都是通过 componentDidMount、componentDidUpdate、componentWillUnmount 生命周期来实现的，而 useEffect 会在组件每次 render 之后调用，就相当于这三个生命周期函数，只不过可以通过传参来决定是否调用其中注意的是，useEffect 会返回一个回调函数，作用于清除上一次副作用遗留下来的状态，如果该 useEffect 只调用一次，该回调函数相当于 componentWillUnmount 生命周期。

一个例子：
```JavaScript
import React, { useState, useEffect } from 'react';

function App() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    window.addEventListener('scroll', onScroll, false);
    return () => {
      window.removeEventListener('scroll', onScroll, false);
    }
  })
  const onScroll = () => {
    console.log('scroll');
  }
  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
export default App;
```
> 现在我们知道了useEffect每次render之后都会执行，那要是我们不需要每次都执行，或者只需要执行一次的时候要怎么办呢？这是就要用到useEffect的第二个参数。

###### useEffect 的第二个参数
* 一共有三种情况
1. 什么都不传，即每次render之后都会执行相当于 componentDidMount 和 componentDidUpdate
2. 传一个空数组[]，只会在初次render的时候执行
3. 传入一个数组，其中包括变量，只有这些变量变动时，useEffect 才会执行

一个例子：
```JavaScript
import React, { useState, useEffect } from 'react';
function App() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    window.addEventListener('scroll', onScroll, false);
    return () => {
      window.removeEventListener('scroll', onScroll, false);
    }
  }, [])
  const onScroll = () => {
    console.log('scroll');
  }
  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });
  useEffect(() => {
    console.log('count-change')
  }, [count])
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
export default App;
```

##### useContext
* 官网的描述接收一个 context 对象（React.createContext 的返回值）并返回该 context 的当前值。当前的 context 值由上层组件中距离当前组件最近的 <MyContext.Provider> 的 value prop 决定。当组件上层最近的 <MyContext.Provider> 更新时，该 Hook 会触发重渲染，并使用最新传递给 MyContext provider 的 context value 值。即使祖先使用 React.memo 或 shouldComponentUpdate，也会在组件本身使用 useContext 时重新渲染。useContext 的参数必须是 context 对象本身。

一个例子：
```JavaScript
// context.js
import { createContext } from 'react'
export const Context = createContext(0)
```
```JavaScript
//child.js
import React from 'react'
import DepChild from './deepChild'
function Child () {
  return (
    <DepChild/>
  )
}
export default Child
```
```JavaScript
// deepChild
import React, { useContext } from 'react'
import { Context } from './context'
function DepChild () {
  const count = useContext(Context);
  return (
    <div>{ count }</div>
  )
}
export default DepChild
```
```JavaScript
//App.js
import React, { useState} from 'react';
import Child from './child'
import { Context } from './context'
function App() {
  const [ count, setCount ] = useState(0)
  return (
    <div>
      点击次数: { count } 
      <button onClick={() => { setCount(count + 1)}}>点我</button>
      <Context.Provider value={count}>
        <Child></Child>
      </Context.Provider>
    </div>
    )
}
export default App;
```
>效果和使用（provider+consumer）、contextType的写法类似

##### useReducer
* 从它的名字可以猜到，它其实就是类似redux中的功能，相较于useState它更适合一些逻辑较复杂且包含多个子值，或者下一个state依赖于之前state等的特定场景。它总共有三个参数：
>1.第一个参数是一个reducer，就是一个函数类似(state, action) => newState的函数，传入上一个state和本次的action
>2.第二个参数就是初始值state，也就是默认值
>3.第三个参数是惰性初始化，可以将用于计算 state 的逻辑提取到 reducer 外部，这也为将来对重置 state 的 action 做处理提供了便利

* 官网例子

```JavaScript
import React, { useReducer } from 'react';
function init(initialCount) {
  return {count: initialCount + 1};
}
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    case 'reset':
      return init(action.payload);
    default:
      throw new Error();
  }
}
function App() {
    const [state, dispatch] = useReducer(reducer, 0, init);
  return (
    <>
      Count: {state.count}
      <button
        onClick={() => dispatch({type: 'reset', payload: 0})}>
        Reset
      </button>
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
export default App;
```

##### useCallback
* useCallball主要用来优化性能，避免对函数的非必要渲染。把内联回调函数及依赖项数组作为参数传入 useCallback，它将返回该回调函数的 memoized 版本，该回调函数仅在某个依赖项改变时才会更新。

一个例子
```JavaScript
import React, { useReducer, useState, useCallback } from 'react';
function App() {
  const [name, setName] = useState('sxy');
  const [age, setAge] = useState(20)
  const changeAge = useCallback((val) => {
    setAge(val)
  }, [age])
  const changeName = (val) => {
    setName(val)
  }
  return (
    <div>
      age: {age}
      <button onClick={() => changeAge(25)}>change age</button>
      <button onClick={() => changeName('syq')}>change name</button>
    </div>
  )
}
export default App;
```
> 第二个参数的传法和useEffect类似

##### useMemo
* 和useCallback类似，useMemo也是用来优化性能的。不过一般来说它是用来避免每次渲染时都进行高开小的计算。

一个例子
```JavaScript
import React, { useReducer, useState, useCallback, useMemo } from 'react';
function App() {
  const [ count, setCount ] = useState(0)
  const [ age, setAge ] = useState(0)
  const add = useMemo(() => {
    return count + 1
  }, [count])
  return (
    <div>
      点击次数: { count }
      age; { age }
      <br/>
      次数加一: { add }
      <button onClick={() => { setCount(count + 1)}}>点我count</button>
      <button onClick={() => { setAge(age + 1)}}>点我age</button>
    </div>
    )
}
export default App;
```
> 可以看到只有当count变化时，age参会重新计算，第二个参数的传法同上。

##### useRef
* 总的来说useRef有两种用法：
>1.获取子组件或者Dom元素的实例（只有类组件可以使用）
>2.在函数组件中的一个全局变量，不会因为重复 render 重复申明， 类似于类组件的 this.xxx

###### 获取子组件实例
* 官网例子
```JavaScript
import React, { useReducer, useState, useCallback, useMemo, useRef } from 'react';
function App() {
    const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` 指向已挂载到 DOM 上的文本输入元素
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
export default App;
```

###### 类组件属性
* 有时候我们需要保证函数组件每次render之后，某些变量是不会被重复申明，比如Dom几点，定时器的id等。在类组件中，我们可以通过添加属性比如this.xxx，但是函数组件没有this。我们就可以通过useRef来实现。

一个例子
```JavaScript

import React, { useReducer, useState, useCallback, useMemo, useRef, useEffect } from 'react';
function App() {
  const [ count, setCount ] = useState(0)
  const timer = useRef(null)
  let timer2 
  
  useEffect(() => {
    let id = setInterval(() => {
      setCount(count => count + 1)
    }, 500)
    timer.current = id
    timer2 = id
    return () => {
      clearInterval(timer.current)
    }
  }, [])
  const onClickRef = () => {
    clearInterval(timer.current)
  }
  const onClick = () => {
    clearInterval(timer2)
  }
  return (
    <div>
      点击次数: { count }
      <button onClick={onClick}>普通</button>
      <button onClick={onClickRef}>useRef</button>
    </div>
    )
}
export default App;
```
>我们可以看到点击普通的click是无法clear的，因为组件每次render的时候都会给timer2重新声明赋值，导致最开始的id丢失无法清除。

##### 其它Hooks
* 可以在官方文档中找到
>1.[useImperativeHandle](https://zh-hans.reactjs.org/docs/hooks-reference.html#useimperativehandle)
>2.[useLayoutEffect](https://zh-hans.reactjs.org/docs/hooks-reference.html#uselayouteffect)
>3.[useDebugValue](https://zh-hans.reactjs.org/docs/hooks-reference.html#usedebugvalue)

##### 自定义Hooks
* 通过使用自定义 Hook，可以将组件逻辑提取到可重用的函数中。来解决逻辑难以复用问题

* 获取屏幕宽度变化hook
```JavaScript
import React, { useState, useCallback, useEffect } from 'react'
function useWidth (defaultWidth) {
  const [width, setWidth] = useState(defaultWidth)
  const onChange = useCallback (() => {
    setWidth(document.body.clientWidth)
  }, [])
  useEffect(() => {
    window.addEventListener('resize', onChange, false)
    return () => {
      window.removeEventListener('resize', onChange, false)
    }
  }, [onChange])
  return width
}
export default useWidth;
```
* 使用
```JavaScript
import useWidth from './useWidth'
function App() {
  const width = useWidth(document.body.clientWidth)
  return (
    <div>
      页面宽度：{ width }
    </div>
    )
}
export default App;
```

#### 推荐的Hook库

>[ahooks](https://ahooks.js.org/zh-CN)


#### 参考文章

> [烤透 React Hook](https://juejin.im/post/6867745889184972814)
> [官方文档](https://zh-hans.reactjs.org/docs/hooks-intro.html)
> [呕心沥血，一文看懂 react hooks](https://juejin.im/post/6844903957534507021)