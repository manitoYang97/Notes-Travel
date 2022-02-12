### 基础使用

```jsx


```

### 常用 hooks

`useRouteMatch` 获取当前路径

```js
const {url, path} = useRouteMatch()

```

`useHistory` 路由跳转

```js
const history = useHistory()
history.push('/')
history.geBack()
```

