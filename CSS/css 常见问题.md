### 水平垂直居中方法

1. #### 绝对定位+平移translate

   适合在不确定盒子的高度和宽度的时候

   ```
      .box {
               position: absolute;
               transform: translate(-50%, -50%);
               left: 50%;
               top: 50%;
               background: pink;
           }
   ```

2. #### 绝对定位+margin盒子的负一半

   确定盒子的高度和宽度的时候

   ```
           .box {
               position: absolute;
               width: 100px;
               height: 100px;
               top: 50%;
               left: 50%;
               margin-top: -50px;
               margin-left: -50px;
               background: pink;
           }
   ```

3. #### 绝对定位+上下左右0+margin:auto

   确定盒子的高度和宽度的时候

   ```
           .box {
               position: absolute;
               width: 100px;
               height: 100px;
               top: 0;
               left: 0;
               bottom: 0;
               right: 0;
               margin: auto;
               background: pink;
           }
   ```

4. #### 弹性盒子

   有父盒子且父盒子有高度，在父盒子里居中，确定盒子的高度和宽度的时候

   ```
           .box {
               height: 800px;
               display: flex;
               align-items: center;
               justify-content: center;
                border: 1px solid #ccc;
           }
           
           div.child {
               width: 100px;
               height: 100px;
               background-color: pink;
           }
   ```

5. #### calc()函数动态计算+子绝父相

   需要父盒子，父子盒子都需要知道宽度高度，只是在父盒子里水平垂直居中

   ```
          .box {
               position: relative;
               width: 400px;
               height: 100px;
               border: 1px solid #ccc;
           }
           
           .box .child {
               position: absolute;
               width: 200px;
               height: 40px;
               left: calc((400px - 200px)/2);
               top: calc((100px - 40px)/2);
               background: pink;
           }
   ```

6. #### table-cell

   盒子里的内容居中，确定盒子的高度和宽度的时候

   ```
    .box {
               display: table-cell;
           //用来指定行内元素（inline）或表格单元格（table-cell）元素的垂直对齐方式
               vertical-align: middle; 
               text-align: center;
               width: 240px;
               height: 180px;
               background-color: pink;
           }
   ```

   

### 隐藏元素的方法和区别

1. #### display:none 

   DOM结构：浏览器不会渲染display值为none的元素，不占据空间。

   事件监听：无法进行DOM事件监听

   性能：动态改变此属性会引起重排，性能较差

   继承：不会被子元素继承，因为子元素也不会被渲染

   transition:transition不支持display

2. #### visibility;hidden

   DOM结构：元素被隐藏，但是渲染不会消失，占据空间

   事件监听：无法进行DOM事件监听

   性能：动态改变此元素会引起重绘，性能较高

   继承：会被子元素继承，子元素通过visibility:visible显示元素

   transition：visibility会立即显示，隐藏时会被过延时

3. #### opacity:0

   DOM结构：元素会被渲染，但是透明度为100%，占据空间

   事件监听：可以进行DOM事件监听

   性能：提升为合成层，不会触发重绘，性能较高

   继承：会被子元素继承，子元素通过opacity:1来取消隐藏

   transition：显示和隐藏都会延时

### 不改变原样式的情况下，改变元素的宽度

```
    <div style="width: 480px!important;background: pink;height: 100px; "></div>
```

1. #### css方法

   ```
    //最大宽度
    <div style="width: 480px!important;background: pink;height: 100px; max-width: 300px;"></div>
   ```

   ```
   //X坐标缩放
   <div style="width: 480px!important;background: pink;height: 100px; transform: scale(0.625,1);"></div>
   ```

   ```
   //用新的同级宽度覆盖
   <div style="width: 480px!important;background: pink;height: 100px; width: 300px!important; "></div>
   
   ```

####   2.js方法

```
//获得元素之后设置属性
document.getElementsByTagName('div')[0].setAttribute('style', 'width: 300px!important;')
```

### 传统布局：圣杯与双飞翼

要点：

- 包括头部和底部，左中右三栏布局
- 两侧固定，中间自适应
- 中间部分DOM结构先行渲染
- 允许三栏任意一栏成为最高列

#### 圣杯布局

结构：左中右放在一个大盒子里，中间放在前面先渲染出来

```
<div id="header">header</div>
    <div id="container">
        <div id="center" class="column">center</div>
        <div id="left" class="column">left</div>
        <div id="right" class="column">right</div>
 </div>
 <div id="footer">footer</div>
```

- 在父盒子上把左右的位置用padding留出来
- 并且里面的每个一元素都向左浮动但在footer上清除浮动，
- 中间的宽度为100%自适应，
- 给左盒子使用负外边距margin-left:-100%和相对定位再向右移动right:200px把它放在预留的位置上,
- 右盒子定义宽度再添加右负外边距margin-right:-150px,

- 最后页面最小宽度设置为200+150+200=550px

```
    body {
        min-width: 550px;
    }
    
    #header,
    #footer {
        width: 100%;
        height: 30px;
        background: #666;
    }
    
    #container {
        padding-left: 200px;
        padding-right: 150px;
    }
    
    .column {
        float: left;
    }
    
    #center {
        width: 100%;
        background: skyblue;
    }
    
    #left {
        width: 200px;
        margin-left: -100%;
        position: relative;
        right: 200px;
        background: pink;
    }
    
    #right {
        width: 150px;
        margin-right: -150px;
        background: pink;
    }
    
    #footer {
        clear: both;
    }
```

#### 使用flex

- 结构与圣杯布局一样，有兼容问题
- order：-1 在视觉上排在前面，不会影响元素的逻辑和实际显示顺序
- 中间自适应，两边固定定位

```
        #header,
        #footer {
            width: 100%;
            height: 30px;
            background: #666;
        }
        
        #container {
            display: flex;
        }
        
        #center {
            flex: 1;
        }
        
        #left {
            flex: 0 0 200px;
            order: -1;
        }
        
        #right {
            flex: 0 0 150px;
        }
```



#### 双飞翼布局

结构：中间放在一个盒子里面，中间放在前面

```
    <div id="header">header</div>
    <div id="container" class="column">
        <div id="center">center</div>
    </div>
    <div id="left" class="column">left</div>
    <div id="right" class="column">right</div>
    <div id="footer">footer</div>
```

- 中间宽度父盒子100%自适应，内容设置padding距离左右的宽度把距离留出来，左右宽度固定
- 左中右都向左浮动，顶部清除浮动
- 左边设置外边距margin-left:-100%
- 右边设置外边距margin-left:-150px
- 最小宽度200+150+150=350px

```
        body {
            min-width: 500px;
        }
        
        #header,
        #footer {
            width: 100%;
            height: 30px;
            background: #666;
        }
        
        #container {
            width: 100%;
        }
        
        .column {
            float: left;
        }
        
        #center {
            padding-left: 200px;
            padding-right: 150px;
        }
        
        #left {
            width: 200px;
            margin-left: -100%;
        }
        
        #right {
            width: 150px;
            margin-left: -150px;
        }
        
        #footer {
            clear: both;
        }
```

#### 总结

圣杯布局比双飞翼布局的结构更加直观易懂，但是双飞翼比圣杯的样式更加简洁没有定位，双飞翼比圣杯的最小宽度更小。但都引入了额外标签

#### 无额外标签

没有额外的标签，在双飞翼布局样式的基础上，center模块添加了一些样式，但是有一点兼容问题

```
  <div id="header">header</div>
    <div id="center" class="column">center</div>
    <div id="left" class="column">left</div>
    <div id="right" class="column">right</div>
    <div id="footer">footer</div>
```

#### 使用border-box

```
        #center {
            padding-left: 200px;
            padding-right: 150px;
            width: 100%;
            box-sizing: border-box;
        }
```

#### 使用calc计算属性

```
      #center {
            padding-left: 200px;
            padding-right: 150px;
            width: calc(100%, -350px);
        }
```

