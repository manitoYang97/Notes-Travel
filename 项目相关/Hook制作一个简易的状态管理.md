通过useContext和useReduce如何制作一个简单的状态管理



Hello，大家好，我是知秋前天给大家的分享一个轻量实用的状态管理`Concent`，但是第三方库还是会占一定的空间，在一个小型的或者需要快速加载的的项目里，并且想要全局共享的状态并不多，我们可以利用Reac中的两个Hooks来定制一个简单的状态管理。



对Hook不熟悉的同学可以看我之前分享的文章



用这两个 Hook 做一个状态管理的工具可以先有这样的思路

- 找到要放在全局的状态并初始化

- 使用context来传递共享的状态和更新状态的方法

- 使用useReduce来初始化和更新状态

- 最后用useContext实例化context并返回状态和更新的方法

  

### 找到需要放在全局的状态

以一个登陆为例，把登陆的信息在很多地方都会用到，所以现在把用户登陆的信息都放在全局状态里面，所以全局的状态就有

```js
const initState = {
  usename= '', // 用户名
  loding= false,  //登陆请求的加载状态
  message = '',  //登陆请求返回的信息
  isTokenverifyEnd = false //凭证是否过期
}
```



### 通过`useReduce`初始化和更新状态

先看更新状态的方法 dispatch，里面只有一个action 就是更新操作 NewState

```js
const reducer = (state, action) => {
  switch (action.type) {
    case actionType.NewState:
      return {
        // 合并更新state
        ...mergeWith(state, action.payload, customizer),
      }
    default:
      return { ...state }
  }
}
```

把useReduce定义在提供组件StoreProvider里面，在每次更新状态之后都会调用useReduce，使消费组件能拿到最新的状态值

```js
const StoreProvider: React.FC = ({ children }) => {
  const [state, dispatch] = React.useReducer(reducer, initState)
  ……
}
```



### 使用Context来传递状态和更新方法

先创建一个Context并以状态和更新方法作为初始值

```js
export const Store = React.createContext<GlobalContext>({
  state: initState,
  dispatch: () => initState,
})
```

传递数据组件里使用Provide包裹着子组件，并且在用value属性来传递数据

```jsx
const StoreProvider: React.FC = ({ children }) => {
  const [state, dispatch] = React.useReducer(reducer, initState)
  …… //这里可以做一些请求获取用户信息接口的操作
  
  return <Store.Provider value={{ state, dispatch }}>{children}</Store.Provider>
}
```



#### useContext实例化context并返回状态和更新的方法

导入Context，可以把dispatch方法指定Action， 在更新的时候只需要传递更新之后的值作为payload就可以了

```js
export const useStore = (): UseStoreHookType => {
  const { state, dispatch } = React.useContext(Store)
  
  const setState = React.useCallback(
    (newState: PartialState) => {
      dispatch({
        type: NewState,
        payload: newState,
      })
    },
    [dispatch]
  )

  return [state, setState]
}
```



### 如何使用自定义的状态管理

在根组件里用StoreProvider包裹住返回的组件

```jsx
export const App = ({ Component, pageProps }: AppProps): JSX.Element => (
    <StoreProvider>
       <Component {...pageProps} />
    </StoreProvider>
)

```

在消费组件里引入useStore解构出state和setState，就能获取和更新全局中的状态

```jsx
const Header = () => {
  const [state, setState] = useStore()
  
  const handleChange = () => {
    setState({usernams: 'yang'})
  }
  
  return (
  	<span>{state.username}</span>
  )
}
```



### 规范化文件格式

建立相关的的文件

![image.png](https://s2.loli.net/2022/01/24/BtImuEGS473zlaj.png)



-  `store.tex`初始化状态，渲染`Provider`提供者组件
- `reducer.ts`存放抽离`useReducer`的`Active`函数
- `useStore.ts`导出更新的钩子
- `constants.ts`存放的是一些通用的常量
- `index.ts`统一导出文件
- `type.js`类型文件



如果觉得对你有帮助，可以点赞收藏，有问题可以评论探讨~