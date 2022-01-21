- 存在的问题
- `Hooks`的目的

- `Hooks`使用的注意事项
- 常见的`Hooks`
  - `useState`
  - `useRender`
  - `useContext`
  - `useEffct`
  - `useCallback`
  - `useMemo`
  - `useRef`

- 自定义`Hook`





## 类组件存在的问题

计数器 `class `写法

```jsx
 class Example extends Component{
     constructor(props){
        super();
        this.state={
            count:0
        } 
     }
    addNum = () =>{
        this.setState({
            count:this.state.count + 1
        })
    }
     render(){
         return(
             <div>
                    <p>this num is {this.state.count}</p>
                    <button onClick={this.addNum}>click me</button>
             </div>
         )
     }
 }
```

存在问题：

- 使用`bind`或者箭头函数去约束我们函数中`this`的作用域

- 状态逻辑的难以复用以及复杂组件变得难以理解



`React `也支持函数组件

```jsx
function App(props) {
  return <h1>Hello, {props}</h1>;
}
```

但是有一个限制，这种纯函数，没有状态，没有生命周期，所以也不能代替类





## Hooks 的目的

`React Hooks `的设计目的，就是加强版函数组件，完全不使用"类"，就能写出一个全功能的组件



`hook`翻译过来是钩子的意思，可以把`state`和其他的`React`的其他特性钩进来

你需要什么功能，就使用什么钩子。`React` 默认提供了一些常用钩子，你也可以封装自己的钩子。

所有的钩子都是为函数引入外部功能，所以` React `约定，钩子一律使用`use`前缀命名，便于识别。



计数器`  hooks `写法

```jsx
function Example (){
    const [count,setCount] = useState(0);
    return(
        <>
            <p>the num is {count}</p>
            <button onClick={()=>{setCount(count+1)}}>click me </button>
        </>
    )
}
```





## Hooks使用的注意事项

- 只能在函数内部的最外层调用`Hook`，不能在循环、判断或子函数中调用

- 只能在`React`的函数组件里调用`Hook`，不能在`JavaScript`函数中调用

  

Q: 为什么只能在函数内部最外层调用呢？

原因：

单个组件中编写多个`hook`，它们的每次调用顺序是由书写的位置决定的，它的内部实现其实是一个链表式的调用。如果不能保证每次的渲染的位置一致就不能保证它能正常的工作。





## 常见的Hook

### 项目中的实例

```jsx
import React, { useState，useEffect, useCallback } from 'react';

function APP() {
   const [refreshCount, setRefreshCount] = useState<number>(0)
   const [list, setList] = useState<ClusterDO[] | null>([])
   
    const handleUpdate = useCallback(
    () => setRefreshCount((prevState) => prevState + 1),
    [])
    
   useEffect(() => {
    const fetchData = async () => {
      const result = await clusterInfo(uniqueId)

      if (result.success) {
        setList(result.data)
      } else {
        message.error(result.message)
        setList(null)
      }
    }

    fetchData()
  }, [uniqueId, refreshCount])

  return (
    <>
     <Table dataSource={list} />
      <button onClick={handleUpdate}>
        Refresh
      </button>
   </>
  );
}
```





### uesState

```jsx
// 1. 引入React 和 useState
import React, { useState } from 'react';

function APP() {
  // 2. 声明 state  变量
   const [refreshCount, setRefreshCount] = useState<number>(0)
   const [list, setList] = useState<ClusterDO[] | null>([])
   
   //3. 更新变量 
    const handleUpdate = () => setRefreshCount((prevState) => prevState + 1)

  return (
   <>
   //4. 使用定义的变量
    <p>{refreshCount}</p>
      <button onClick={handleUpdate}>
        Refresh
      </button>
    </>
  );
```

1. 为什么要调用`useState`？
2. `useState()`的返回值是什么？有什么用？
3. 调用了多个`useState`，`React`如果知道每个`useState`对应的是那个值？





**为什么要调用useState？**

- 调用`useState`的时候定义了一个`state`状态可以当做变量使用，这是*函数保存变量的方式*，其他变量在函数退出之后都会消失，但是`state`会保存下来

**useState()的返回值是什么？有什么用？**

- 调用`useState` 会**返回一个数组**，数组项分别是：一个 `state`，一个更新` state `的函数 

- 在初始化渲染期间，返回的状态 (`state`) 与传入的第一个参数 (`initialState`) 值相同 
- 我们可以在事件处理函数中或其他一些地方调用这个更新函数，给*更新函数传递一个新的值就能改变`state`的值*，它会用新的 `state`把旧的 `state `直接替换

**调用了多个useState，React如果知道每个useState对应的是那个值？**

- 链式调用



**tips**

**使用多个state变量（分离独立的state）**

- `useState`也支持传递对象和数组，所以也可以把多个变量写在一个对象里，但是由于`state`更新之后是会替换它的值而不是合并值，所以更**推荐使用多个变量单独定义的方式让它们同时方式变化，这样写的好处还有就是之后两个变量分离在不同的组件能更好的抽取**

  ```js
    //不建议像这样把所有state都写在一个对象里
    const [state, setState] = React.useState({
      isFetching: false,
      startTime: null,
      endTime: null,
    })
    
    //更新其中一个的方法
    setState((prevState) => ({ ...prevState, isFetching: false }))
  ```



遇到的问题

![image-20210712235354343](/Users/admin/Library/Application Support/typora-user-images/image-20210712235354343.png)

1. 为什么`setState`函数执行之后立即输出` state` 相关的值没有变化

2. 下一次点击的` state.selectedRowKeys `的值还是空，为什么不是上一次设置进去的值？

3. 为什么` render state.selectedRowKeys `这个输出有最新的值？ 

   

   **为什么setState函数执行之后立即输出 state 相关的值没有变化**

   答：`useState` 返回的更新状态方法是“异步”的，要在下次重绘才能获取新值。不要试图在更改状态之后立马获取状态

   

   **下一次点击的 state.selectedRowKeys 的值还是空，为什么不是上一次设置进去的值？**

   答： 在`useCallback`的依赖项里没有添加`state.selectedRowKeys `这个值，所以它会一直都是第一次的值，但是第一次渲染的值是空的

   

   **为什么 render state.selectedRowKeys 这个输出有最新的值？** 

   答：` log`是同步的在重新渲染的时候都能拿到最新的值





经典的面试题

- **react中`setState`是同步的还是异步？什么场景下是异步的，可不可能是同步，什么场景下又是同步的？**

  

  - `setState` 只在[合成事件](https://zh-hans.reactjs.org/docs/events.html)和钩子函数中是“异步”的，在原生事件和 `setTimeout` 中都是同步的。

    

  - `setState`的“异步”并不是说内部由异步代码实现，其实本身执行的过程和代码都是同步的，只是合成事件和钩子函数的调用顺序在更新之前，导致在合成事件和钩子函数中没法立马拿到更新后的值，形式了所谓的“异步”，当然可以通过第二个参数 setState(partialState, callback) 中的callback拿到更新后的结果。

  

  - `setState` 的批量更新优化也是建立在“异步”（合成事件、钩子函数）之上的，在原生事件和setTimeout 中不会批量更新，在“异步”中如果对同一个值进行多次 `setState` ， `setState` 的批量更新策略会对其进行覆盖，取最后一次的执行，如果是同时 `setState` 多个不同的值，在更新时会对其进行合并批量更新。

    

  

  **tips**   

  *合成事件*React模拟原生DOM事件功能的事件对象，即浏览器原生事件的跨浏览器包装器，它根据W3C标准定义，能兼容所有浏览器，还拥有和原生事件相同的接口

  

  React 中所有事件都是合成的，不是原生DOM事件，但是可以通过e.nativeEvent来获取原生DOM事件

  ```jsx
  // 原生DOM事件
  <button onclick="activateLasers()">Activate Lasers</button>
  
  //React 中的合成事件
  <button onClick={activateLasers}>Activate Lasers</button>
  ```

  合成事件的特点

  - 命名采用驼峰式，不是纯小写
  - 使用JSX语法时需要传入一个函数作为事件处理函数，不是一个字符串





### useReducer

它是`useState`的替代方案。它接收一个形如 `(state, action) => newState` 的` reducer`，并返回当前的 `state `以及与其配套的 `dispatch` 方法。

```js
const [state, dispatch] = useReducer(reducer, initialArg, init)
```



使用场景

在某些场景下，`useReducer` 会比 `useState` 更适用，例如 state 逻辑较复杂且包含多个子值，或者下一个 `state `依赖于之前的` state` 等。

并且，使用 `useReducer` 还能给那些会触发深更新的组件做性能优化，因为你可以向子组件传递 `dispatch` 而不是回调函数

重写`useState`计数器的例子

```jsx
const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```



**tips**

指定初始`state`：将初始值作为第二个参数传入

```jsx
 const [state, dispatch] = useReducer(
    reducer,
    {count: initialCount}
  )
```



*惰性初始化*：创建函数作为`useReducer`的第三个参数传入，初始的`state`就是函数的参数

这么做可以将用于计算` state `的逻辑提取到` reducer` 外部，这也为将来对重置 `state` 的 `action `做处理提供了便利

```jsx
function init(initialCount) {
  return {count: initialCount};
}

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    case 'reset':
      return init(action.payload);
    default:
      throw new Error();
  }
}

function Counter({initialCount}) {
  const [state, dispatch] = useReducer(reducer, initialCount, init);
  return (
    <>
      Count: {state.count}
      <button
        //重置按钮
        onClick={() => dispatch({type: 'reset', payload: initialCount})}>
        Reset
      </button>
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```



跳过`dispatch`  (`useReducer`的幂等性)

如果 `Reducer Hook` 的返回值与当前 `state `相同，`React` 将跳过子组件的渲染及副作用的执行。（`React `使用`Object.is` 比较算法 来比较 state。）





### useEffect  

- `useEffect `能在函数组件中执行副作用，最常见的就是获取后台数据

- 我们可以把 `useEffect Hook` 看做 `componentDidMount`，`componentDidUpdate` 和 `componentWillUnmount` 这三个生命周期函数的组合

  ```jsx
   useEffect(() => {
      const fetchData = async () => {
        const result = await clusterInfo(uniqueId)
  
        if (result.success) {
          setList(result.data)
        } else {
          message.error(result.message)
          setList(null)
        }
      }
  
      fetchData()
    }, [uniqueId, refreshCount])  //仅在页面初次渲染和uniqueId或者refreshCount改变的时候重新渲染
  ```

  一般是传递两个参数，第一个是函数可以放异步操作，第二个参数是数组，放的是依赖项，只要这个数组发生变化`useEffect()`就会执行

  

- `useEffect `分为*需要清除的*和*不需要清除的effect*

  **不需要清除的操作**：在`React`更新`DOM`之后会运行一些额外的代码，如发送网络请求、手动更新`DOM`、打印日志等，这些都是无需清除的操作

  

  **需要清除的操作**：订阅外部的数据源，清除可以防止内存泄漏

  **如何实现清除？**

  - 如果`effect`返回一个函数，`React`将会在执行清除的时候调用它

  ```js
    useEffect(() => {
      function handleStatusChange(status) {
        setIsOnline(status.isOnline);
      }
  
      ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
      //实现清除
      return () => {
        ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
      };
    },[props.friend.id]
  ```

  1. 调用`useEffect`的做了什么？
  2. `useEffect`每次都会渲染吗？
  3. 何时会执行清除`effect`？



**调用useEffect的做了什么？**

- 通过这个`hook`可以告诉`React `组件在渲染后要执行的操作，`React`会保存传递的函数，在`DOM`更新后调用。

**useEffect每次都会渲染吗？**

- `useEffect`在第一次渲染和每次更新的时候都会执行，控制它执行的是第二个参数列表放的变量更新之后会触发它执行

**何时会执行清除effect**

- 在组件卸载的时候`React`会执行清除操作



**tips**

**使用独立逻辑的Effect**

使用多个`Effect`把不同的逻辑分离到不同的`effect`中，可以实现关注点分离，`React `将按照` effect` 声明的顺序依次调用组件中的*每一个* `effect`。





### useCallback

```js
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

返回一个 `memoized` 回调函数。

把内联回调函数及依赖项数组作为参数传入 `useCallback`，它将返回该回调函数的版本，该回调函数仅在某个依赖项改变时才会更新。

当你把回调函数传递给经过优化的并使用引用相等性去避免非必要渲染

依赖项数组不会作为参数传给回调函数。所有回调函数中引用的值都应该出现在依赖项数组中。



`useCallback(fn, deps)` 相当于 `useMemo(() => fn, deps)`

 useMemo 与 useCallback 的区别，即 useMemo 是缓存了函数的返回值，useCallback 是缓存了函数自身。这两个 api 都是性能优化的方法



> 记忆（memoization）是主要用于加速程序计算的一种优化技术，它使得函数避免重复演算之前已被处理过的输入，而返回已缓存的结果





### useMemo

```js
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

返回一个memoized 值。

把“创建”函数和依赖项数组作为参数传入 `useMemo`，它仅会在某个依赖项改变时才重新计算 memoized 值。

这种优化有助于避免在每次渲染时都进行高开销的计算。

传入 `useMemo` 的函数会在渲染期间执行。

**你可以把 `useMemo` 作为性能优化的手段，但不要把它当成语义上的保证。**





### useRef

`useRef` 返回一个可变的` ref` 对象，其 `.current` 属性被初始化为传入的参数（`initialValue`）。

```js
const refContainer = useRef(initialValue)
```

返回的` ref` 对象在组件的整个生命周期内保持不变,也就是说每次重新渲染函数组件时，返回的`ref` 对象都是同一个（使用` React.createRef` ，每次重新渲染组件都会重新创建` ref`）



使用场景：

1、`dom ref  ` 引用  2、每次渲染都想保持同一个引用    3.`useRef`当成变量使用，修改它不会引发 `render`

```jsx
function TextInputWithFocusButton() {
  const echEL = React.useRef(null)
 
React.useEffect(() => {
    const myChart = echarts.init(echEL.current)
    ....
,[]})
  
  return (
    <div ref={echEL} className="echartsSty"></div>
  );
}
```





### useContext

**前置知识：Context**

`Context`是一个*无需为每层组件手动添加`Props`*，就能在组件树间进行*数据传递*的方法

作用：共享对于一个组件树而言的全局的数据。

主要应用场景：在于*很多*不同层级的组件需要访问同样一些的数据。请谨慎使用，因为这会使得组件的复用性变差。



`useContext`就是接收一个`context`对象，并返回该`context`的当前值。当前的`context`的当前值。当前的 `context` 值由上层组件中距离当前组件最近的 `<MyContext.Provider>` 的 `value prop` 决定。

当组件上层最近的 `<MyContext.Provider>` 更新时，该` Hook` 会触发重渲染，并使用最新传递给 ` provider` 的 `context  value` 值。

```jsx
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};

const ThemeContext = React.createContext(themes.light);

function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return (
    <button style={{ background: theme.background, color: theme.foreground }}>
      I am styled by theme context!
    </button>
  );
}
```





### 自定义Hooks

共享组件之间的状态逻辑可以使用自定义`hooks（`或者`render props`和高阶组件），自定义`Hook`是一个函数以`use`开头，在自定义 `Hook `中调用其他 `Hook`

```jsx
export function useClusterDetail(uniqueId: string): ClusterDetailResult {
  const [cluster, setCluster] = React.useState<ClusterDO | null>(null)
  const [refreshCount, setRefreshCount] = React.useState<number>(0)

  React.useEffect(() => {
    const fetchData = async () => {
      const result = await clusterInfo(uniqueId)

      if (result.success) {
        setCluster(result.data)
      } else {
        $notification.error(result.message)
        setCluster(null)
      }
    }

    fetchData()
  }, [uniqueId, refreshCount])

  return { cluster, setRefreshCount }
}

//使用 
  const { cluster, setRefreshCount } = useClusterDetail(uniqueId)
```







