



## 前言

2021 年 8 月 16 号，也就是今天，感谢国家为我们新兴农民工正名，在新兴性前端农民工日益增加的时代，为了更好的搬砖，`next` 成了 `React` 项目在做服务端渲染不得不学的一个框架，前端为什么要做服务端渲染，两个大点，一是搜索引擎 `SEO` 优化，二是减少首屏渲染时间 



## Next.js  的优势

> `Next.js` 是一个轻量级的 `React` 服务端渲染框架

- 搭建方便
- 数据同步策略
- 插件丰富
- 灵活配置



## Next 项目的搭建

### 手动搭建

1. 新建一个文件夹命名如` NextDemo`

2. 在这个文件夹下面执行 `npm init` 把文件夹初始化成一个可以管理的项目，文件夹下面就多了一个 `package.json` 的文件

3. 下载项目中需要用到的插件

   ```
   yarn add react react-dom next 
   ```

4. 用 vscode 打开这个项目并手动在 `pakage.json` 里面添加项目运行和打包等的增加快捷命令

   ```json
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1",
       "dev": "next",
       "build": "next build",
       "start": "next start"
     },
   ```

5. 在根目录下添加 `pages` 文件夹（`Next` 规定的） ，然后在这个文件夹里写一个组件如 `index.js`

   ```js
   function Index () {
       return (
           <div>Hello Next.js</div>
       )
   }
   
   export default Index
   ```

6. 打开命令行执行 `yarn dev`，打开 http://localhost:3000 就能看见 我们写的程序运行结果了

   <img src="/Users/admin/Library/Application Support/typora-user-images/image-20210815113955064.png" alt="image-20210815113955064" style="zoom:50%;" />



### 自动搭建

使用 `create-next-app` 也就是 `Next`  的脚手架来搭建

1. 全局安装 `create-next-app`

   ```
   npm install -g creat-next-app
   ```

2. 用脚手架搭建项目

   ```
   npx create-next-app next-creat
   ```

3. 运行 `Next` 项目

   ```
   yarn dev
   ```





## Next 中的 Pages

`pages`  文件夹里用来放置一些页面，`Next`会根据页面放置的层级生成相应的路由，这样我们就不用自己配置页面的路由了

`componen`t 文件夹里写一些不包含页面的公用或专用组件

比如在 `pages` 文件夹下面建立一个` index.js` 的文件并导出，在页面路由` / `下就能访问到这个页面

<img src="/Users/admin/Library/Application Support/typora-user-images/image-20210815124425197.png" alt="image-20210815124425197" style="zoom:50%;" />

​                                                       <img src="/Users/admin/Library/Application Support/typora-user-images/image-20210815124516257.png" alt="image-20210815124516257" style="zoom:50%;" />

还可以看到 `pages/blog/myBlog.js `这个文件，访问的路径对于就应该是 `/blog/myBlog`

<img src="/Users/admin/Library/Application Support/typora-user-images/image-20210815124852078.png" alt="image-20210815124852078" style="zoom:50%;" />



默认在写组件的时候是可以不引入 `react` 和 `next` 的





## Next.js 中的路由

### 标签跳转

标签跳转是指使用 `a` 标签 或 `Link` 实现跳转的方式

```
import Link from 'next/link'
```

可以做一个简单的跳转案例

```js
import Link from 'next/link'

const Index = () => (
    <>
        <div>早上好，欢迎你</div>
        <Link href='/login'>退出</Link>
    </>
)

export default Index
```

```js
import Link from 'next/link'

const Login = () => (
    <>
        <div>你好，请登录</div>
        <Link href='/'>立即登录</Link>
    </>
)

export default Login
```

<img src="/Users/admin/Library/Application Support/typora-user-images/image-20210815131648236.png" alt="image-20210815131648236" style="zoom:50%;" /><img src="/Users/admin/Library/Application Support/typora-user-images/image-20210815131729881.png" alt="image-20210815131729881" style="zoom:50%;" />



注意： 当我们在`Link`标签里写了多个子标签就会报错

```jsx
const Login = () => (
    <>
        <div>你好，请登录</div>
        <Link href='/'>
            <span>立即登录</span>
            <span>》</span>
        </Link>
    </>
)
```

<img src="/Users/admin/Library/Application Support/typora-user-images/image-20210815132417921.png" alt="image-20210815132417921" style="zoom:50%;" />

`Link` 标签里面有多个标签的时候必须有一个父标签

```jsx
const Login = () => (
    <>
        <div>你好，请登录</div>
        <Link href='/'>
            <a>
            <span>立即登录</span>
            <span>》</span>
            </a>
        </Link>
    </>
)
```



#### 编程式跳转

编程式跳转可以使用 `router` 来进行，这样就可以把跳转任务写在业务方法里面，也能实现逻辑的复用

```
import Router from 'next/router'
```

现在就可以用编程式跳转改变一个上面的 Link 跳转了

```jsx
const Index = () =>{
    const handleLogout = () => {
        Router.push('/login')
    }

return (
    <>
        <div>早上好，欢迎你</div>
        <button onClick={handleLogout}>退出</button>
    </>
    )
}
```



### 动态路由跳转

动态路由跳转是指的在进行页面跳转的时候要携带一些参数进行跳转，比如我们点击一个列表进入详情肯定要知道这是这是列表的第几项这样一个 `id` ，登陆也会也携带用户的信息

Next 中的动态跳转参数只支持 `query` 的形式，也就是 `?id=1` 这种，不支持` path:id` 这种

获取 `query` 中的参数需要用到 `router` 中的高级组件 `withRouter`

```
import {witRouter} from 'next/router'
```

下面来改写一下登陆，让登陆的时候能都携带参数，并且在首页能获取到显示出来

```jsx
const Login = () => (
    <>
        <div>你好，请登录</div>
        <Link href='/?name=yang'>
            <a>
            <span>立即登录</span>
            <span>》</span>
            </a>
        </Link>
    </>
)
```

```jsx
import Router, {withRouter} from 'next/router'

const Index = ({router}) =>{
    const handleLogout = () => {
        Router.push('/login')
        console.log('Router:',router);
    }

return (
    <>
        <div>早上好，欢迎你{router.query.name}</div>
        <button onClick={handleLogout}>退出</button>
    </>
    )
}

export default withRouter(Index)
```

<img src="/Users/admin/Library/Application Support/typora-user-images/image-20210815140459891.png" alt="image-20210815140459891" style="zoom:50%;" />



### 路由的钩子事件

- `routeChangeStart` 路由发生变化时触发
- `routeChangeComplete` 路由结束时触发
- `beforeHistoryChange`  改变浏览器 `history `触发，`Next `默认用的是 `history` 所以每次改变路由都会触发
- `routeChangeError`  路由发生错误时触发
- `hashChangeStart`    `hash` 路由发生改变时触发
- `hashChangeComplete`  `hash` 路由结束时触发

```jsx
import Router, {withRouter} from 'next/router'
import Link from 'next/link'

const Index = ({router}) =>{
    const handleLogout = () => {
        Router.push({
            pathname: '/login',
            query: 'yang'
        })
    }

    Router.events.on('routeChangeStart',(...args)=>{
        console.log('1.routeChangeStart->路由开始变化,参数为:',...args)
      })
    
      Router.events.on('routeChangeComplete',(...args)=>{
        console.log('2.routeChangeComplete->路由结束变化,参数为:',...args)
      })
    
      Router.events.on('beforeHistoryChange',(...args)=>{
        console.log('3,beforeHistoryChange->在改变浏览器 history之前触发,参数为:',...args)
      })
    
      Router.events.on('routeChangeError',(...args)=>{
        console.log('4,routeChangeError->跳转发生错误,参数为:',...args)
      })
    
      Router.events.on('hashChangeStart',(...args)=>{
        console.log('5,hashChangeStart->hash跳转开始时执行,参数为:',...args)
      })
    
      Router.events.on('hashChangeComplete',(...args)=>{
        console.log('6,hashChangeComplete->hash跳转完成时,参数为:',...args)
      })

return (
    <>
        <div>早上好，欢迎你{router.query.name}</div>
        <button onClick={handleLogout}>退出</button>
        <div>
         <Link href="/?name=yang#hash">hash跳转</Link>
      </div>
    </>
    )
}

export default withRouter(Index)

```





## Next 中获取数据

`Next` 中使用 `getInitialProps` 来获取远端的数据

```jsx
const Async = ({data}) =>{
    
return (
        <div>{data.data.username} 代号 {data.data.uid}</div>
    )
}

Async.getInitialProps = async() => {
    const promise = new Promise(resolve=>{
        setTimeout(()=>{
            return resolve({
              data: {
                data: {
                  uid: '007',
                  username: '小白鼠'
                }
              }
            })
          }, 100)
    })

    return await promise
}

export default Async
```



当然也可以用来获取页面中的数据，把 `async` 和 `awiat` 去掉就可以了



## Next 中的样式

### 使用 Style JSX 编写样式

`Next` 中支持用 `style jsx` 来编写样式

```jsx
const Async = ({ data }) => {
  return (
    <>
      <div className="name">
        {data.data.username} 代号 {data.data.uid}
      </div>
      <style jsx>
        {`
          .name {
            color: red;
          }
        `}
      </style>
    </>
  );
};
```

<img src="/Users/admin/Library/Application Support/typora-user-images/image-20210815175920278.png" alt="image-20210815175920278" style="zoom:50%;" />



### 支持 CSS 文件

默认是不支持直接引入 `css` 文件的，需要先下载  `@zeit/next-css`

```
yarn add @zeit/next-css
```

然后再进行配置，项目根目录建一个 `next.config.js`

```js
const withCss = require('@zeit/next-css')

if(typeof require !== 'undefined'){
    require.extensions['.css']=file=>{}
}

module.exports = withCss({})
```



## Next 实现懒加载

使用 Next 实现懒加载 LazyLoading 让我们切换到对应的路由才会去加载相应的 js 模块

LazyLoading 可以分为加载第三方模块和自定义组件

加载第三方模块是一个异步的操作所以要用到 getInitial

```jsx
const Index = ({ router, Hour }) => {
  const handleLogout = () => {
    Router.push("/login");
  };

  const getHour = () => {
    switch (true) {
      case Hour >= 6 && Hour <= 9:
        return "上午好";
      case Hour >= 10 && Hour <= 14:
        return "中午好";
      case Hour >= 15 && Hour <= 19:
        return "下午好";
      case Hour >= 20 && Hour <= 23:
        return "晚上好";
      default:
        return "夜深了";
    }
  };

  return (
    <>
      <div>
        {getHour()}，欢迎你{router.query.name}
      </div>
      <button onClick={handleLogout}>退出</button>
    </>
  );
};

Index.getInitialProps = async () => {
  const dayjs = await import("dayjs");
  const Hour = dayjs.default().hour();
  return { Hour };
};
```



对于自定义的组件进行懒加载可以用  `dynamic` 

```
import dynamic from "next/dynamic"

const MyButton = dynamic(import("../component/MyButton"))
```



## 自定义 Head 优化 SEO

```
import Head from "next/head";

      <Head>
        <title>next.js 基础 </title>
        <meta charSet="utf-8" />
      </Head>
```





## 按需加载 Antd 

需要安装和配置 `babel-plugin-import`

```
yarn add babel-plugin-import
```

安装完成之后再项目的根目录下新建`.babelrc`文件，写入配置

```
{
    "presets":["next/babel"],  //Next.js的总配置文件，相当于继承了它本身的所有配置
    "plugins":[     //增加新的插件，这个插件就是让antd可以按需引入，包括CSS
        [
            "import",
            {
                "libraryName":"antd",
            }
        ]
    ]
}
```

之后引入 `antd` 的组件就能实现按需加载了

```
import {Button} from 'antd'

//解析成
import Button from 'antd/lib/button'
```

