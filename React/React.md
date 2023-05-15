# React学习笔记

## React基础

### 渲染函数

React提供render函数把节点渲染到DOM标签上。



### 组件的应用与复用

React组件像纯函数一样运行。

```react
component = (props) =>element
//纯函数
// 1.对于相同的入参，返回同样的结果
// 2.无副作用
function sum(a,b){
    return a+b
}
//非纯函数
function withdraw(account,amount){
    account.total -= amount
}
```



### 组件类型

```react
//函数组件
function Test (props){
    return (<div>{props.name}</div>)
}

//类组件
class Test extends React.Component {
    render(){
        return (<div>{this.props.name}</div>)
    }
}
```



### 状态管理state

state只能在类组件中使用。

```react
class Test extends React.Component {
    //挂载前调用
    constructor (props){
        super(props)
	    this.state = { date: new Date() }	
    }
    //挂载后调用  处理数据请求和 DOM 操作等操作
    componentDidMount(){
        this.timeId = setInterval(()=>this.Ticker(),1000)
    }
    //组件状态更新之后调用，可以在这里进行 DOM 操作和数据请求等操作
    componentWillAmount(){
        clearInterval(this.timeId)
    }
    
    Ticker(){
    	this.state:({
            date:new Date()
        }); 
    }
    
    render(){
        return (
            <div>this.state.date.toLocalTimeString</div>
        )
    }
}
```

下面是一些关于函数式组件和类组件的比较：

1. **性能**

函数式组件相对于类组件来说，更加轻量级，因为函数式组件不需要维护实例化对象的状态和生命周期方法等，只需要处理 props 和渲染结果。因此，函数式组件在渲染时的性能比类组件更高效。

2. **可读性**

函数式组件相对于类组件来说，更加简洁明了，因为函数式组件只需要编写函数体即可，而类组件需要定义一个类，包含一些生命周期方法、构造函数和状态等。在一些简单的场景下，使用函数式组件能够让代码更加易读。

3. **Hooks 支持**

React Hooks 是 React 16.8 版本引入的新特性，允许在函数式组件中使用状态、副作用等 React 功能，使得函数式组件的使用更加灵活和强大。而类组件的状态管理则需要使用 this.setState() 方法，代码相对于函数式组件来说更加冗长。

因此，总体来说，函数式组件在 React 中的使用更加普遍和推荐。当然，在一些复杂的场景下，类组件也有它的优势，比如需要使用 React 生命周期、Refs、继承等特性时。但是随着 React 的不断发展，函数式组件也在不断增强，未来会更加强大和灵活。



### 列表渲染

```jsx
const products = [
  { title: 'Cabbage', isFruit: false, id: 1 },
  { title: 'Garlic', isFruit: false, id: 2 },
  { title: 'Apple', isFruit: true, id: 3 },
];

return (
	<Component>
        {
            products.map(product,index)=>
                <li key = {product.id}>{product.title}</li>
        }
    </Component>
)
```



### 组件数据共享

把要可改变的属性和方法传给子组件。

```jsx
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}
```



### 生命周期

React 组件生命周期是指组件在挂载、更新和卸载等过程中所经历的一系列事件和状态变化。React 组件生命周期可以帮助我们更好地管理组件状态和行为，包括处理数据请求、更新 DOM 元素、处理事件等。在 React 16.8 版本中引入的 Hooks 也提供了类似生命周期的功能，可以在函数式组件中使用。

下面是 React 类组件的生命周期方法：

1. `constructor(props)`：组件的构造函数，在组件被挂载之前调用，可以在这里初始化组件的状态和绑定事件等操作。
2. `componentDidMount()`：组件挂载之后调用，可以在这里处理数据请求和 DOM 操作等操作。
3. `shouldComponentUpdate(nextProps, nextState)`：组件状态更新之前调用，可以在这里根据 nextProps 和 nextState 来决定是否需要更新组件，优化组件性能。
4. `componentWillUpdate(nextProps, nextState)`：组件状态更新之前调用，可以在这里处理组件状态和 DOM 元素的更新操作。
5. `componentDidUpdate(prevProps, prevState)`：组件状态更新之后调用，可以在这里进行 DOM 操作和数据请求等操作。
6. `componentWillUnmount()`：组件卸载之前调用，可以在这里清理定时器、取消事件监听等操作。

在 React 16.8 版本之后，Hooks 提供了一种新的方式来管理组件状态和行为，因此函数式组件也可以实现类似的生命周期功能。使用 Hooks 可以使用 `useEffect` 方法，它包含 `componentDidMount`、`componentDidUpdate` 和 `componentWillUnmount` 的功能。

函数式组件相比于类组件，不需要显式地定义生命周期方法，因为函数式组件只处理输入的 props 和渲染输出。但是，在函数式组件中也可以使用 `useEffect` 来实现类似生命周期的功能，例如在组件渲染完成后执行某个操作，或者在组件卸载之前执行清理操作等。



## React路由

### 基础配置

- **Router**
  - 
- **Route**
- **Link**

```react
import React from 'react'
import { Router, Route, Link, IndexRoute } from 'react-router'

import { Test } from './test'
const App = () =>{
    return (
    	<Router>
        	<Route path = "/test" component = { Test }/>
        </Router>
    )
}
```

**路由匹配原理**

**History**

**react-router 和 react-router-dom**



# React Hooks

管理组件状态和行为，避免了类组件中一些繁琐的操作。



## useState

A React Hook that lets you add a state variable to your component. 

```react
import { useState } from 'react';
//current state in the first render  ,  the set function
const [name,setName] = useState("")
```

**Caveats 注意事项**

The set function only updates the state variable for the next render. If you read the state variable after calling the state, you still get the previous old value that was on the screen before you call. 

set函数仅更新下一次渲染的状态变量。如果在调用状态之后读取状态变量，那么仍然会得到调用之前屏幕上的旧值。

```react
//Eg 1  
function handleClick(){
    setAge(age+1)  // setAge(42 + 1)
    setAge(age+2)  // setAge(42 + 2)
    setAge(age+3)  // setAge(42 + 3)
    //最后age = 45
}
//To solve this problem
//You may pass an updater function to setAge instead of the next state:
//Updater function takes the pending state and calculates the next state from it.
function handleClick() {
  setAge(a => a + 1); // setAge(42 => 43)
  setAge(a => a + 1); // setAge(43 => 44)
  setAge(a => a + 1); // setAge(44 => 45)
}

//Eg 2
function handleClick() {
  console.log(count);  // 0
    
  setCount(count + 1); // Request a re-render with 1
  console.log(count);  // Still 0!

  setTimeout(() => {
    console.log(count); // Also 0!
  }, 5000);
}

```

**嵌套对象** **Objects in state**

 The `{ ...form }` spread syntax ensures that the state object is replaced rather than mutated.

{ ...form } 传播语法确保状态对象被替换而不是变异。

```react
//Eg 1
const [form, setForm] = useState({
    firstName: 'Barbara',
    lastName: 'Hepworth',
    email: 'bhepworth@sculpture.com',
  });

setForm({
    ...form,
    firstName: e.target.value
});

//Eg 2
const [person, setPerson] = useState({
    name: 'Niki de Saint Phalle',
    artwork: {
      title: 'Blue Nana',
      city: 'Hamburg',
      image: 'https://i.imgur.com/Sd1AgUOm.jpg',
    }
});

setPerson({
    ...person,
    artwork: {
        ...person.artwork,
        title: e.target.value
    }
});
```

**Avoiding recreating the initial state 避免重新创建初始状态**

React saves the initial state once and ignores it on the next renders.

```react
//每次渲染时都会调用一次函数
const [todos, setTodos] = useState(createInitialTodos());
//createInitialTodos为第一次渲染时函数返回结果，后面不会再调用
const [todos, setTodos] = useState(createInitialTodos);
```

1. Although the result of `createInitialTodos()` is only used for the initial render, **you’re still calling this function on every render.** 
2. Notice that you’re passing `createInitialTodos`, which is the *function itself*, and not `createInitialTodos()`, which is the result of calling it. If you pass a function to `useState`, React will only call it during initialization.

**Resetting state with a key 一键重置状态**

```REACT
const [name, setName] = useState('Taylor');

<input
    value={name}
    onChange={e => setName(e.target.value)}
/>
```

**初始化程序或更新程序函数运行两次**

在开发Strict mood时React会调用函数两次，可以确保检查组件是否纯净。

```react
//不纯的
setTodos(prevTodos => {
  // 🚩 Mistake: mutating state
  prevTodos.push(createTodo());
});
//修正后
setTodos(prevTodos => {
  // ✅ Correct: replacing with new state
  return [...prevTodos, createTodo()];
});
```

**只有组件、初始化器和更新器函数需要是纯函数。**事件处理程序不需要是纯的，因此 React 永远不会调用您的事件处理程序两次。



## useEffect

```react
//Setup: function
//Dependencies: include props, state, and all the variables and functions declared directly inside your component body. 
useEffect(setup,dependencies)
```

### **Caveats 注意事项**

- 如果您的 Effect 不是由交互（如点击）引起的，React 将让浏览器**在运行您的 Effect 之前先绘制更新的屏幕。**如果您的 Effect 正在做一些可视化的事情（例如，定位工具提示），并且延迟很明显（例如，它闪烁），请替换`useEffect`为useLayoutEffect。
- 即使您的 Effect 是由交互（如点击）引起的，**浏览器也可能会在处理 Effect 内的状态更新之前重新绘制屏幕。**通常，这就是你想要的。但是，如果您必须阻止浏览器重新绘制屏幕，则需要替换useEffect为useLayoutEffect。



### **Usage**

**Connecting to an external system**

Some components need to stay connected to the network, some browser API, or a third-party library, while they are displayed on the page. These systems aren’t controlled by React, so they are called external.

链接到外部系统。有些组件在显示在页面上时需要保持与网络、某些浏览器 API 或第三方库的连接。这些系统不受 React 控制，因此它们被称为外部系统。

1. **Listening to a global browser event**

   监听浏览器全局事件。Effect 可以让你连接到`window`对象并监听它的事件。以下例子侦听`pointermove`事件可以让您跟踪光标（或手指）的位置并更新红点以随之移动。

   ```react
   import { useState, useEffect } from 'react';
   
   export default function App() {
     const [position, setPosition] = useState({ x: 0, y: 0 });
   
     useEffect(() => {
       function handleMove(e) {
         setPosition({ x: e.clientX, y: e.clientY });
       }
       //监听鼠标移动事件
       window.addEventListener('pointermove', handleMove);
       return () => {
         window.removeEventListener('pointermove', handleMove);
       };
     }, []);
   
     return (
       <div style={{
         position: 'absolute',
         backgroundColor: 'pink',
         borderRadius: '50%',
         opacity: 0.6,
         transform: `translate(${position.x}px, ${position.y}px)`,
         pointerEvents: 'none',
         left: -20,
         top: -20,
         width: 40,
         height: 40,
       }} />
     );
   }
   
   ```

   

2. **Connecting to chat server**

   连接到聊天服务器。

   ```react
   import createConnection from "./chat.js"
   const [serverUrl, setServerUrl] = useState('https://localhost:1234');
   
   useEffect(() => {
       //connect to external server
       const connection = createConnection(serverUrl, roomId);
       connection.connect();
       return () => {
         connection.disconnect();
       };
   }, [roomId, serverUrl]);
   ```

   

3. **Triggering an animation**

   触发动画。

   This component [uses a ref](https://react.dev/learn/manipulating-the-dom-with-refs) to access the underlying DOM node. 

   The Effect reads the DOM node from the ref and automatically starts the animation for that node when the component appears.

   ```react
   import { useState, useEffect, useRef } from 'react';
   import { FadeInAnimation } from './animation.js';
   
   function Welcome() {
     const ref = useRef(null);
   
     useEffect(() => {
       const animation = new FadeInAnimation(ref.current);
       animation.start(1000);
       return () => {
         animation.stop();
       };
     }, []);
   
     return (
       <h1
         ref={ref}
         style={{
           opacity: 0,
           color: 'white',
           padding: 50,
           textAlign: 'center',
           fontSize: 50,
           backgroundImage: 'radial-gradient(circle, rgba(63,94,251,1) 0%, rgba(252,70,107,1) 100%)'
         }}
       >
         Welcome
       </h1>
     );
   }
   
   export default function App() {
     const [show, setShow] = useState(false);
     return (
       <>
         <button onClick={() => setShow(!show)}>
           {show ? 'Remove' : 'Show'}
         </button>
         <hr />
         {show && <Welcome />}
       </>
     );
   }
   ```

   

4. **Tracking element visibility**

   追踪元素可见性。

   在此示例中，外部系统又是浏览器 DOM。该`App`组件显示一个长列表，然后是一个`Box`组件，然后是另一个长列表。向下滚动列表。请注意，当`Box`组件出现在视口中时，背景颜色变为黑色。为了实现这一点，该`Box`组件使用 Effect 来管理一个[`IntersectionObserver`](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API). 当 DOM 元素在视口中可见时，此浏览器 API 会通知您。

   The Intersection Observer API provides a way to asynchronously observe changes in the intersection of a target element with an ancestor element or with a top-level document's [viewport](https://developer.mozilla.org/en-US/docs/Glossary/Viewport).

   ```react
   //App.js
   import Box from './Box.js';
   export default function App() {
     return (
       <>
         <LongSection />
         <Box />
         <LongSection />
         <Box />
         <LongSection />
       </>
     );
   }
   
   function LongSection() {
     const items = [];
     for (let i = 0; i < 50; i++) {
       items.push(<li key={i}>Item #{i} (keep scrolling)</li>);
     }
     return <ul>{items}</ul>
   }
   ```

   ```react
   import { useRef, useEffect } from 'react';
   
   export default function Box() {
     const ref = useRef(null);
   
     useEffect(() => {
       const div = ref.current;
       const observer = new IntersectionObserver(entries => {
         const entry = entries[0];
         if (entry.isIntersecting) {
           document.body.style.backgroundColor = 'black';
           document.body.style.color = 'white';
         } else {
           document.body.style.backgroundColor = 'white';
           document.body.style.color = 'black';
         }
       });
       observer.observe(div, {
         threshold: 1.0
       });
       return () => {
         observer.disconnect();
       }
     }, []);
   
     return (
       <div ref={ref} style={{
         margin: 20,
         height: 100,
         width: 100,
         border: '2px solid black',
         backgroundColor: 'blue'
       }} />
     );
   }
   ```

   

### Wrapping Effects in custom Hooks

在自定义hooks中包装Effect。

Escape Hatch: Some of your components may need to control and synchronize with systems outside of React. For example, you might need to focus an input using the browser API, play and pause a video player implemented without React, or connect and listen to messages from a remote server. 

例如，这个`useChatRoom`自定义 Hook 将您的 Effect 逻辑“隐藏”在更具声明性的 API 后面：

```react
function useChatRoom({ serverUrl, roomId }) {
  useEffect(() => {
    const options = {
      serverUrl: serverUrl,
      roomId: roomId
    };
    const connection = createConnection(options);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId, serverUrl]);
}
```

Then you can use it from any component like this:

```react
function ChatRoom({ roomId }) {
  const [serverUrl, setServerUrl] = useState('https://localhost:1234');

  useChatRoom({
    roomId: roomId,
    serverUrl: serverUrl
  });
  // ...
```

**More Usage**

1. #### Custom `useChatRoom` Hook 

   ```react
   //App.js
   import { useState } from 'react';
   import { useChatRoom } from './useChatRoom.js';
   
   function ChatRoom({ roomId }) {
     const [serverUrl, setServerUrl] = useState('https://localhost:1234');
   
     useChatRoom({
       roomId: roomId,
       serverUrl: serverUrl
     });
   
     return (
       <>
         <label>
           Server URL:{' '}
           <input
             value={serverUrl}
             onChange={e => setServerUrl(e.target.value)}
           />
         </label>
         <h1>Welcome to the {roomId} room!</h1>
       </>
     );
   }
   
   export default function App() {
     const [roomId, setRoomId] = useState('general');
     const [show, setShow] = useState(false);
     return (
       <>
         <label>
           Choose the chat room:{' '}
           <select
             value={roomId}
             onChange={e => setRoomId(e.target.value)}
           >
             <option value="general">general</option>
             <option value="travel">travel</option>
             <option value="music">music</option>
           </select>
         </label>
         <button onClick={() => setShow(!show)}>
           {show ? 'Close chat' : 'Open chat'}
         </button>
         {show && <hr />}
         {show && <ChatRoom roomId={roomId} />}
       </>
     );
   }
   ```

   ```react
   //useChatRoom.js
   import { useEffect } from 'react';
   import { createConnection } from './chat.js';
   
   export function useChatRoom({ serverUrl, roomId }) {
     useEffect(() => {
       const connection = createConnection(serverUrl, roomId);
       connection.connect();
       return () => {
         connection.disconnect();
       };
     }, [roomId, serverUrl]);
   }
   ```

   ```react
   //chat.js
   export function createConnection(serverUrl, roomId) {
     // A real implementation would actually connect to the server
     return {
       connect() {
         console.log('✅ Connecting to "' + roomId + '" room at ' + serverUrl + '...');
       },
       disconnect() {
         console.log('❌ Disconnected from "' + roomId + '" room at ' + serverUrl);
       }
     };
   }
   ```

   

   

2. #### Custom `useWindowListener` Hook

   

3. #### Custom `useIntersectionObserver` Hook 

   

### Fetching data with Effects 

使用 Effect 为您的组件获取数据。

```react
import { useState, useEffect } from 'react';
import { fetchBio } from './api.js';

export default function Page() {
  const [person, setPerson] = useState('Alice');
  const [bio, setBio] = useState(null);
    
  async function startFetching() {
      setBio(null);
      const result = await fetchBio(person);
      if (!ignore) {
        setBio(result);
      }
  }
    
  useEffect(() => {
    let ignore = false;
    startFetching();
    return () => {
      ignore = true;
    }
  }, [person]);

  return (
    <>
      <select value={person} onChange={e => {
        setPerson(e.target.value);
      }}>
        <option value="Alice">Alice</option>
        <option value="Bob">Bob</option>
        <option value="Taylor">Taylor</option>
      </select>
      <hr />
      <p><i>{bio ?? 'Loading...'}</i></p>
    </>
  );
}
```



### Updating state based on previous state from an Effect

根据 Effect 的先前状态更新状态。

**错误写法**

```react
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const intervalId = setInterval(() => {
      setCount(count + 1); // You want to increment the counter every second...
    }, 1000)
    return () => clearInterval(intervalId);
  }, [count]); // 🚩 ... but specifying `count` as a dependency always resets the interval.
  // ...
}
```

**正确写法**

```react
import { useState, useEffect } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const intervalId = setInterval(() => {
      setCount(c => c + 1); // ✅ Pass a state updater
    }, 1000);
    return () => clearInterval(intervalId);
  }, []); // ✅ Now count is not a dependency

  return <h1>{count}</h1>;
}
```



## useRef

- useRef is a React Hook that lets you reference a value that’s not needed for rendering.
- useRef in different components - canvas & Textarea & Input
- 



## useContext



- 



## DIFF

