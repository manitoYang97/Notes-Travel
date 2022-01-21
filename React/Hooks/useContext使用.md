useContext 



## 前置知识

context

什么场景下使用context？

我们知道React 是单向数据流，如果组件之间有嵌套引用的关系如A引用B，B引用C，像套娃一样的关系， 在没有状态管理的情况下，只能通过props一层一层的进行传递，当组件间的调用过多的时候，维护会变得十分复杂，React自带API context可以使这个局面得到缓和，实现A组件直接到C组件的值传递，不需要经过中间组件。



talk is cheap， show your code

举个小栗子🌰

A>B>C三个组件逐层嵌套，每个组件里都包含半句话，在A组件里包含我的个人信息，需要在C组件里显示，要求信息数据不通过B组件传递。

<img src="/Users/admin/Library/Application Support/typora-user-images/image-20210716231943618.png" style="zoom:50%;" />



下面是使用context进行实现。

```jsx
//组件A
import React from "react";
import B from "./B";

export const nameContext = React.createContext("");
export default function App() {
  return (
    <nameContext.Provider value={"ys"}>
      大家好，
      <B />
    </nameContext.Provider>
  );
}
```

```jsx
//组件B
import C from "./C";

export default function B() {
  return (
    <>
      我是今天的分享者，
      <C />
    </>
  );
}
```

```jsx
//组件C
import React from "react";
import { nameContext } from "./App";

export default function C() {
  return (
    <nameContext.Consumer>
      {(name) => <span>我叫{name}</span>}
    </nameContext.Consumer>
  );
}
```



实现一个像这样的context跨组件值传递只需要记住三步（跟把大象放进冰箱一样🤦🏻‍♀️

1. 在要传递数据的组件里使用createContext建立一个context，并导出
2. 传递数据组件里使用Provide包裹着子组件，并且在用value属性来传递数据
3. 接受数据的组件导入定义的context使用Consumer来接收,可接收到的是一个函数。（消费组件)



值得了解的知识点

- 当创建了一个Context对象，在React渲染出了订阅这个对象的组件（这里是组件B），它能获取到组件树中距离最近Provider中value的值。当没有匹配到Provider的时候创建时传递的初始值会生效。如果value的值是undefined初始值不生效。
- Provider 及其内部 consumer 组件都不受制于 `shouldComponentUpdate` 函数，因此当 consumer 组件在其祖先组件退出更新的情况下也能更新。
- 通过新旧值检测来确定变化，使用了与 Object.is 相同的算法。
- 一个 Provider 可以和多个消费组件有对应关系。多个 Provider 也可以嵌套使用，里层的会覆盖外层的数据



多个Context

现在不不光好要传递我的个人信息，还要把我分享的主题在C组件里展示，所以在A组件里又定义了一个context，在C组件里接收到

<img src="/Users/admin/Library/Application Support/typora-user-images/image-20210717214729055.png" alt="image-20210717214729055" style="zoom:50%;" />

```jsx
//组件A
import React from "react";
import B from "./B";

export const nameContext = React.createContext("");
export const titleContext = React.createContext("");
export default function App() {
  return (
    <nameContext.Provider value={"我是ys，"}>
      <titleContext.Provider value={"今天的主题是Hooks使用"}>
        大家好，
        <B />
      </titleContext.Provider>
    </nameContext.Provider>
  );
}
```

```jsx
//组件C
import React from "react";
import { nameContext, titleContext } from "./App";

export default function C() {
  return (
    <nameContext.Consumer>
      {(name) => (
        <>
          <span>{name}</span>
          <titleContext.Consumer>
            {(title) => <span>{title}</span>}
          </titleContext.Consumer>
        </>
      )}
    </nameContext.Consumer>
  );
}
```



问题：当要传递多个context的时候，就会出现层层嵌套，又复杂麻烦了起来。这时候我们就可以使用useContext这个Hooks来改造一下上面的代码





## useContext

改造代码，我们只需要改变消费组件（也就是C组件）获取值的方式，使用useContext获取A组件传递的值

<img src="/Users/admin/Library/Application Support/typora-user-images/image-20210717221833501.png" alt="image-20210717221833501" style="zoom:50%;" />

```jsx
import React, { useContext } from "react"
import { nameContext, titleContext } from "./App"

export default function C() {
  const name = useContext(nameContext)
  const title = useContext(titleContext)

  return (
    <span>
      {name}
      {title}
      （useContext方式）
    </span>
  )
}
```



使用 uesContext 又是祖传的三个步骤

1. import useContext
2. import context对象
3. 调用 useContext 并传入 context 对象，它的返回值就是共享的数据



值得记住的知识点

- `useContext`就是接收一个`context`对象，并返回该`context`的当前值。当前的`context`的当前值。当前的 `context` 值由上层组件中距离当前组件最近的 `<MyContext.Provider>` 的 `value prop` 决定。

- 当组件上层最近的 `<MyContext.Provider>` 更新时，该` Hook` 会触发重渲染，并使用最新传递给 ` provider` 的 `context  value` 值。