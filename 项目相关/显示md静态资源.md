### 在页面显示md文件

使用第三方插件 vditor 并导入

```js
import VditorPreview from 'vditor/dist/method.min'
import 'vditor/dist/index.css'
import 'vditor/dist/css/content-theme/light.css'
```

获取div元素的的ref把md的内容显示到div里面

```jsx
const previewRef = useRef<HTMLDivElement>(null)

    <div className="markdown-review" ref={previewRef}>
      <Loading />
    </div>
```

在页面初始化时，使用fetch获取到静态文件，并且用preview方法把获取到的文件放在div元素上

```jsx
  useEffect(() => {
    const fetchData = async () => {
      let markdownString = 'Failed to read document'

      try {
        const res = await fetch(`/assets/docs/how-to-backup-${dbType.toLowerCase()}-to-s3.md`)

        if (res.status === ApiCode.OK) {
          const data = await res.text()

          markdownString = data
        }
      } catch (error) {
        console.error(error)
      }

      setTimeout(() => VditorPreview.preview(previewRef.current, markdownString), 200)
    }

    fetchData()
  }, [dbType])
```

问题描述：如果是弹框显示以上整个读md操作必须写在一个单独的组将引入