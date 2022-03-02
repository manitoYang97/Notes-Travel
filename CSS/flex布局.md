### 前言

你有没有这种困惑，页面写了很久，但是对于布局都是有一种拼凑的感觉，很多布局都是靠margin，padding弄出来的，适配小屏就用媒体查询，flex就只是用来模块居中的，这样用法就太大材小用了，所以今天就系统的看一下flex的使用。

读完本篇文章你将会了解到

- flex 布局的基本概念
- 容器的属性
- 项目的属性
- flex 解决的实际问题

### 大局为重，概念先行

flex 是 flexible 简写，flex 布局就是弹性灵活的布局，现在大部分浏览器都能支持的布局，是目前最通用的布局方式之一。它有两个概念是容器和项目，容器是指的最外层的父组件，项目是里面每一个子组件，在这样一个平面上有横竖两条线，默认水平线是主轴，垂直线是交叉轴。


### 容器的属性

开启 flex 布局可以使用

```css
    display: flex
```

#### 主轴方向

可以使用flex-direction来定义主轴的方向是在水平上还是垂直上（默认属性是 row），还可以添加 reverse 表示反向

```css
    /* 水平方向 */
    flex-direction: row
    flex-direction: row-reverse
```

```css
    /* 垂直方向 */
    flex-direction: column
    flex-direction: column-reverse
```

#### 项目是否换行

使用 flex-wrap 可以让容器中的项目是否换行，默认不换行

```css
    /* 不换行 */
    flex-wrap: nowrap
```

```css
    /* 换行 */
    flex-wrap: wrap
```

```css
    /* 换行反转 */
    flex-wrap: wrap-reverse
```

#### 项目流向

指的是项目流的方向，和流向是否换行，使用 flex-flow 是主轴方向和是否换行的合写属性

```css
    flex-flow: <flex-direction> <flex-wrap>
```

#### 项目对齐方式

**主轴对齐方式**
设置项目在主轴上的对齐方式可以用 justify-content 实现

```css
	/* 左对齐 */
	justify-content: flex-start
	/* 右对齐 */
	justify-content:flex-end
	/* 居中对齐 */
	justify-content: center
	/* 两端贴边对齐 */
	justiify-content: space-between
	/* 两端空隙对齐 */
	justify-content: space-around
```

**交叉轴对齐方式**
交叉轴对齐方式分成两种，一种是只设置单行线 align-items 在交叉轴上对齐

```css
    /* 左对齐 */
    align-items: flex-start

    /* 右对齐 */
    align-items: flex-end

    /* 居中 */
    align-items: center 

    /* 拉伸 */
    align-items: stretch

    /* 基线对齐 */
    align-items: baseline
```

第二种是设置交叉轴上的两条线以上的对齐方式 使用 align-content

```css
    /* 拥有 align-items 除基线对齐*/
    /* 两端对齐 */
    align-content: space-around
    align-content: space-between
```

### 项目属性

#### 项目的排列顺序

可以使用 order 来进行，数值越小越靠前，默认值是0

#### 项目缩放

项目放大有空闲位置时放大比例 flex-grow 默认值0 不放大

```css
    flex-grow: 0
```

在空闲位置不够的时候，使用 flex-shrink 默认值 1 会缩小

```css
    flex-shrink: 1
```

项目本身的大小，可以用数值和百分比固定, auto 表示原本大小

```css
    flex-basis: auto
```

缩放和本身大小可以使用合写属性 flex 来表示

```css
    flex: <flex-grow> <flex-shrink> <flex-basis>
    /* 缩写 */
    flex: 1
    
    /* flex: auto */
    flex: 1 1 auto

    /* flex: none */
    flex: 0 0 auto
```

#### 单独设置某个项目的对齐方式

```css
    /* 其余属性和align-items一样 */
    align-self: auto
```

## flex实战

### 网格布局

```css
    .Grid {
        display: flex;
      }
      .Grid-cell {
        background: #ccc;
        margin-left: 10px;
        flex: 1;
      }
      .Grid-cell.u-full {
        flex: 0 0 100%;
      }

      .Grid-cell.u-1of2 {
        flex: 0 0 50%;
      }

      .Grid-cell.u-1of3 {
        flex: 0 0 33.3333%;
      }

      .Grid-cell.u-1of4 {
        flex: 0 0 25%;
      }

    <div class="Grid">
      <div class="Grid-cell u-1of4">...</div>
      <div class="Grid-cell">...</div>
      <div class="Grid-cell">...</div>
    </div>
```

### 圣杯布局

```css
      .HolyGrail {
        display: flex;
        flex-direction: column;
        min-height: 100vh;
      }
      header,
      footer {
        flex: 1;
        border: 1px solid #ccc;
      }
      .HolyGrail-body {
        display: flex;
        flex: 6;
      }
      .HolyGrail-nav,
      .HolyGrail-ads {
        flex: 0 0 12em;
        border: 1px solid #ccc;
      }
      .HolyGrail-ads {
        order: -1;
      }
      .HolyGrail-content {
        flex: 1;
      }
      @media (max-width: 765px) {
        .HolyGrail-body {
          flex-direction: column;
        }
        .HolyGrail-content,
        .HolyGrail-nav,
        .HolyGrail-ads {
          flex: auto;
        }
      }

    <div class="HolyGrail">
      <header>...</header>
      <div class="HolyGrail-body">
        <main class="HolyGrail-content">...</main>
        <nav class="HolyGrail-nav">...</nav>
        <aside class="HolyGrail-ads">...</aside>
      </div>
      <footer>...</footer>
    </div>
```

### 输入框布局

```css
      .InputAddOn {
        display: flex;
      }
      .InputAddOn-field {
        flex: 1;
      }

    <div class="InputAddOn">
      <span class="InputAddOn-item">...</span>
      <input class="InputAddOn-field" />
      <button class="InputAddOn-item">...</button>
    </div>
```

### 悬挂式布局

```css
      .Media {
        display: flex;
        align-items: flex-start;
      }
      .Media-bod {
        margin-left: 1em;
        flex: 1;
      }

   <div class="Media">
      <img class="Media-figure" src="" alt="image" />
      <p class="Media-body">...</p>
    </div>
```

### 固定布局

```css
      .fixed {
        display: flex;
        min-height: 100vh;
        flex-direction: column;
      }
      .Site-content {
        flex: 1;
      }
      
    <div class="fixed">
      <header>...</header>
      <main class="Site-content">...</main>
      <footer>...</footer>
    </div>
```