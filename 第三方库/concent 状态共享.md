### concent 状态共享

在concent中提供一个全局唯一的store，store是由多个模块组成，每个模块又分别由state、reducer、computed、watch、lifecycle组成

### 模块

可以把每个部分都写成js文件放在一个模块文件夹里面，组成一个完整的model定义（reducer独立定义之后内部的函数的dispatch调用就能直接基于引用而非字符串了），在model文件中再添加一个index.js在里面合并并暴露配置接口。

**默认模块**：$$default 需要显示的配置，否则是一个空模块，如果不配置默认模块实例state和模块state是相互独立的，实例的setState不会触发模块数据的更改。

**全局模块**：$$global，也需要显示的配置



#### state模块

定义：把需要共享的属性放在state里并设置默认值，可以实现全局共享

```js
const state= {
  token: '',
  selectedOrgId: null,
  selectedProjId: null,
}
```

获取：

- ctx .state 获取state

- ctx.moduleState 获取属于模块的state

  ```js
  const { moduleState } = useConcent<AnyObject, Ctx>({module: 'user'})
  
  {moduleState.user.selectedOrgId}
  ```

- ctx.globalState获取内置的全局state

- ctx.connectedState {meduleState} 获取连接（请求)上的state

  ```js
    const { cr, moduleState, connectedState } = useConcent<AnyObject, Ctx>({
      module: 'user',
      connect: ['cluster'],
    })
  
  const currentDbOptions = connectedState.cluster.dbOptions[dbType]
  ```

  

#### reducer模块

定义：reducer对象是一个个片段状态生成的函数的集合，key是函数名，value是片段状态生成函数，负责生成并返回新的部分状态。

触发：

- 实例的ctx.dispatch触发

  - 内部触发 actionCtx.dispatch来触发其他reducer函数

    ```js
    const result = await actionCtx.dispatch(login, params)
    ```

- 实例的ctx.settings去呼叫预先的函数触发()

  setup在组件首次渲染之前被触发执行一次，其返回结果收集在ctx.settings里。可以在setup函数体内定义实例的钩子函数，让组件省去重复渲染的时间。
  
- ```jsx
  //setup先定义函数
    handleJoinSubmit: async (data: UserParams): Promise<void> => {
        setState({ message: '' })
  
        const state = await moduleReducer.join(data)
        const isLogin = Boolean(state.token)
  
        if (isLogin) {
          history.push('/account/setup')
        }
  ```
  
  ```jsx
//导入settings
  const { settings} = useConcent<InstProps, InstCtx>({
      setup,
      module: 'user',
      props: { history, from },
    })
  //绑定在DOM上
    <JoinForm onSubmit={settings.handleJoinSubmit} />
  ```
  
- ctx.reducer.{moduleName} {functionName}

  ```js
  const { moduleState, moduleReducer } = useConcent({ module: 'user' })
  
   const handleOrgChange = React.useCallback(
      (value) => {
        moduleReducer.orgChange(value)
      },
      [moduleReducer]
    )
  ```



#### computed模块

comouted可以定义多个计算结果键retKey，每个reKey对应一个计算函数，首次载入模块时按定义的顺序依次执行所有的计算函数并将其结果缓存。

```js
export const isLogin = {
  fn: ({ token, isTokenVerifyEnd }: UserSt): boolean => isTokenVerifyEnd && Boolean(token),
  depKeys: ['token', 'isTokenVerifyEnd'],
}
```

收集依赖：依赖列表depKeys里面的值发生变化时才会触发计算函数。



#### lifecycle模块

提供initState、initStateDone、loaded、mountd、willUnmount五个可选生命周期函数

- initState异步初识化状态

- initStateDone initState执行结束后的业务逻辑

- 模块载入完毕时执行的工作，可使用loaded替代initState

  ```js
  // 模块载入完毕时触发执行
  export function loaded(dispatch: D): void {
    dispatch(rd.initState)
  }
  ```

- mounted组件实例挂载完毕驱动触发，默认只触发一次，如果需要反复触发需要返回false
- willUnmount当最后一个组件卸载时需要触发的函数，通常用于清理工作

### 全局API

> - 知道使用`run`中心化的配置模块，`configure`去中心化的配置模块
> - 知道使用`register`、`registerDumb`、`useConcent`注册组件
> - 知道使用`setState`、`dispatch`修改状态

#### run函数

- run函数负责载入用户定义的模块配置并启动concent,其他顶层的函数调用必须在调用run之后才能
- 调用可以把`import {run} from 'concent'`放在入口函数index里.
- run函数的第一个参数作为启动concent的store配置项，concent启动后会维护这个全局唯一的ConcentContext对象

```js
import { run } from 'concent'
import * as models from '@/models'

run(models, { log: __DEV__ })
```



#### register函数（使用较少）

注册类组件的模块信息，concent实例化组件会读取注册信息生成相应的实例上下文。

#### useConcent函数

为函数组件注册模块信息，concent实例化组件时会读取注册信息生成响应的实例上下文

- 使用方法：1.从concent模块直接导出，2.从默认导出里调用

  ```js
  import {useConcent} from 'concent'
  useConcent()
  
  import cc from 'concent'
  cc.useConcent()
  ```

- 注册所属模块时，会返回实例上下文ctx

  ```js
  const ctx = useConcent('foo')
  
  const ctx = usConcent({module:'foo'})
  ```

- 可以读取模块中的数据state，也可以用setState修改模块中的数据

  ```js
  const {state} = useConcent({module:'foo'})
  
  const {setState} = useConcent({module:'foo'})
  const changeName = e =>setState({name: e.target.value})
  ```

- 如果reducer里提供了对应的方法，可以使用mr、dispatch去处罚修改

  ```js
  const {mr,dispatch} = useConcent({module:'foo'})
  const changeName = e =>mr.changeName(e.target.value)
  const changeName = e => dispatch('changeName',e.target.value)
  ```

- 当组件需要消费多个模块的数据时，可以使用connect参数来声明要连接的多个模块，ctx.connectedState获取目标模块的数据

  ```js
  const {connectedState:{bar,baz}} = useConcent({connect:['bar','baz']})
  ```

- 如果reducer里以提供对应的方法，可以使用cr、dispatch去触发模块的reducer方法修改数据

  ```js
  const {cr,dispatch} = useConcent({connect:['bar','baz']})
  const changeName = e => cr.bar.changeName(e.target.value)
  const changeName = e => dispatch('bar/changeName',e.target.value)
  ```

- 同时配置module和connect来实现既属于某个模块又连接多个模块的场景

  ```js
  const {state,connectedState} = useConcent({module:'foo',connect:['bar','baz']})
  ```



### 实例

#### 实例上下文

concent组件实例化之后会创建一个RefContext实例上下文对象，所有的实例接口都有RefContext提供。

每个组件实例都有一个引用ctx，指向concent为其构建的实例上下文对象，在实例初始化阶段完成构建，被保管在全局上下文中，实例销毁就从全局上下文删除。

ctx上携带的模块信息可以让concent知道怎样将提交的状态精确的分发到其他实例上，所有的实例api、相关状态、计算结果等都挂载在ctx对象上。

获取上下文：在函数组件里可以直接获取到ctx

```js
const ctx = useConcent('counter')
```



#### 实例setup

setup在组件首次渲染之前被触发执行一次，其返回结果收集在ctx.settings里。可以在setup函数体内定义实例的钩子函数，让组件省去重复渲染的时间。



#### 实例effect

`setup`函数能够被类组件和实例组件初次渲染前都能够被执行，且仅执行一次。setup函数定义的副作用函数可以达到统一函数组件生命周期的效果。

```js
const setup = (ctx) =>{
const {effect} = ctx
  effect(() => {
    if (moduleComputed.isLogin) {
      history.push(from)
    }
  }, ['token'])
}
```

- 只有一个函数，每一轮都会执行，等效于didMount+didUpdata
- 设置第三位参数为null，表示无任何依赖，每一轮都执行。设置第四位参数为false表示初次渲染时不执行。等效于didUpdate
- 设置第三位参数依赖数组为[ ]，表示仅首次渲染执行
- 第三位参数['count']只有当count值发生变化时才会执行，改函数会在首次渲染的完毕后执行。（注意这里传递的是stateKey名称，concent会自动比较上一刻的count值和此刻是否不同 来决定是否触发副作用函数）
- effectProps 和 effect等效，只不过依赖数组传递的是propsKey名称

