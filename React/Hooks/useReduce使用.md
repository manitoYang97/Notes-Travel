## useReducer



## 前置知识

#### reducer是什么？

reducer是是伴随redux出现而流行起来的，我们先不用了解redux。单说reducer，简单来说它是一个形如(state,action) => newState的函数，接受一个state和改变状态的动作action，返回一个新的state。

举个例子🌰

```js
  function countReducer(state, action) {
        switch(action.type) {
            case 'add':
                return state + 1;
            case 'sub':
                return state - 1;
            default: 
                return state;
        }
    }
```

上面这个函数就是一个reducer，传入一个state，更改它的动作就是加和减，返回改变后的state



#### reducer的特性

幂等性：传出同样的值，结果永远都一样

```
    expect(countReducer(1, { type: 'add' })).equal(2); // 成功
    expect(countReducer(1, { type: 'add' })).equal(2); // 成功
```



#### reducer接收对象作为state

注意📢

- reducer处理的state对象必须是不可变的，也就是我们不能直接去改变对象的值，因为React中新旧state的比较是通过Object.is()来进行的浅比较（对象是只比较地址，直接改变对象的话指向的地址还是同一个，所以即使对象的值发生改变了，它还是会被认为是同一个值），同一个值就不会触发改变。
- 我们可以通过解构的方法返回一个新的对象

```jsx
 // 返回一个 newState (newObject)
    function countReducer(state, action) {
        switch(action.type) {
            case 'add':
                return { ...state, count: state.count + 1; }
            case 'sub':
                return { ...state, count: state.count - 1; }
            default: 
                return count;
        }
    }
```



#### action有哪些组成

Action表示动作，它的属性type表示具体动作的类型，是reducer执行的判断条件。payload可选属性表示要附加携带的数据 







## 进入useReducer

useReducer是进阶版的useState，当state变得繁多复杂的时候可以时候useReducer来替代useState，实现状态的易于维护和管理，复杂逻辑更好复用

```jsx
const [state,dispatch] = useReducer(reducder,initState)
```

- 参数：第一个是上面将的reducer函数，第二个是state的初始值
- 返回值： 第一个是返回的新的state，第二个是用于调用reducer函数的dispatch方法，它接受一个action



官网小例子🌰

```jsx
    // 第一个参数：应用的初始化
    const initialState = {count: 0};

    // 第二个参数：state的reducer处理函数
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
        // 返回值：最新的state和dispatch函数
        const [state, dispatch] = useReducer(reducer, initialState);
        return (
            <>
                // useReducer会根据dispatch的action，返回最终的state，并触发rerender
                Count: {state.count}
                // dispatch 用来接收一个 action参数「reducer中的action」，用来触发reducer函数，更新最新的状态
                <button onClick={() => dispatch({type: 'increment'})}>+</button>
                <button onClick={() => dispatch({type: 'decrement'})}>-</button>
            </>
        );
    }
```





改造项目中的例子🌰

```jsx
    function LoginPage() {
        const [name, setName] = useState(''); // 用户名
        const [pwd, setPwd] = useState(''); // 密码
        const [isLoading, setIsLoading] = useState(false); // 是否展示loading，发送请求中
        const [error, setError] = useState(''); // 错误信息
        const [isLoggedIn, setIsLoggedIn] = useState(false); // 是否登录

        const login = (event) => {
            event.preventDefault();
            setError('');
            setIsLoading(true);
            login({ name, pwd })
                .then(() => {
                    setIsLoggedIn(true);
                    setIsLoading(false);
                })
                .catch((error) => {
                    // 登录失败: 显示错误信息、清空输入框用户名、密码、清除loading标识
                    setError(error.message);
                    setName('');
                    setPwd('');
                    setIsLoading(false);
                });
        }
        return ( 
            //  返回页面JSX Element
        )
    }

```

问题：一系列的复杂state，分散难以维护



使用useReducer重写之后

```jsx
    const initState = {
        name: '',
        pwd: '',
        isLoading: false,
        error: '',
        isLoggedIn: false,
    }
    function loginReducer(state, action) {
        switch(action.type) {
            case 'login':
                return {
                    ...state,
                    isLoading: true,
                    error: '',
                }
            case 'success':
                return {
                    ...state,
                    isLoggedIn: true,
                    isLoading: false,
                }
            case 'error':
                return {
                    ...state,
                    error: action.payload.error,
                    name: '',
                    pwd: '',
                    isLoading: false,
                }
            default: 
                return state;
        }
    }
    function LoginPage() {
        const [state, dispatch] = useReducer(loginReducer, initState);
        const { name, pwd, isLoading, error, isLoggedIn } = state;
        const login = (event) => {
            event.preventDefault();
            dispatch({ type: 'login' });
            login({ name, pwd })
                .then(() => {
                    dispatch({ type: 'success' });
                })
                .catch((error) => {
                    dispatch({
                        type: 'error'
                        payload: { error: error.message }
                    });
                });
        }
        return ( 
            //  返回页面JSX Element
        )
    }

```

改善：代码量虽然变多了，但是整体逻辑更清晰，能实现逻辑复用，统一管理状态易于维护。

how和what分离，reducer实现how，组件只用管what，如登陆是组件只需要调用dispatch({ type: 'login' });





useReducer+useContext 实现login例子🌰

```jsx
    // 定义初始化值
    const initState = {
        name: '',
        pwd: '',
        isLoading: false,
        error: '',
        isLoggedIn: false,
    }
    // 定义state[业务]处理逻辑 reducer函数
    function loginReducer(state, action) {
        switch(action.type) {
            case 'login':
                return {
                    ...state,
                    isLoading: true,
                    error: '',
                }
            case 'success':
                return {
                    ...state,
                    isLoggedIn: true,
                    isLoading: false,
                }
            case 'error':
                return {
                    ...state,
                    error: action.payload.error,
                    name: '',
                    pwd: '',
                    isLoading: false,
                }
            default: 
                return state;
        }
    }
    // 定义 context函数
    const LoginContext = React.createContext();
    function LoginPage() {
        const [state, dispatch] = useReducer(loginReducer, initState);
        const { name, pwd, isLoading, error, isLoggedIn } = state;
        const login = (event) => {
            event.preventDefault();
            dispatch({ type: 'login' });
            login({ name, pwd })
                .then(() => {
                    dispatch({ type: 'success' });
                })
                .catch((error) => {
                    dispatch({
                        type: 'error'
                        payload: { error: error.message }
                    });
                });
        }
        // 利用 context 共享dispatch
        return ( 
            <LoginContext.Provider value={dispatch}>
                <...>
                <LoginButton />
            </LoginContext.Provider>
        )
    }
            
            
    function LoginButton() {
        // 子组件中直接通过context拿到dispatch，出发reducer操作state
        const dispatch = useContext(LoginContext);
        const click = () => {
            if (error) {
                // 子组件可以直接 dispatch action
                dispatch({
                    type: 'error'
                    payload: { error: error.message }
                });
            }
        }
    }

```

优点：使用useReducer代替复杂的state，如果组件层级比较深，需要子组件触发state，可以同时使用useContext传递dispatch