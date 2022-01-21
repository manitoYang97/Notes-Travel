# 看完 React 哲学，我悟了



### 前言

最近测试给我提的的 `bug` 终于少了很多， 在 `codeReview`  的时候同事们也很少指出我那个地方写的不对，反而对我整体的文件结构和组件的编写结构及状态的设计提出了更高的要求，不得不说我这代码水平还是有所提高的，表示在稳步提升的过程还有很大的进步空间。

但是当我在看到同事给我说的整个组件如何分离才能提高维护性和复用性，别人在看的时候也能更清晰的知道这部分的逻辑，当时我就好奇，为啥同事能有这种见解，难道只是因为经验比我多，思考比我深入吗？我的直觉告诉我没有这么简单。

如何在看到设计图的时候就想好如何划分这块业务的逻辑，如何设计自己想要的数据结构，我在脑中思索着，忽然我想起初学 `React` 的时候那个被我撇过一眼就速度滑过的概念 `React` 哲学

我立马去官网看着概念， 果然那些我以前我以前对于一个组件不知道证明设计结构和状态，这些组件写到后面状态不通，还有我几乎还会在每个子组件都请求一遍数据，对于一些想显示的数据都定义一个 `state` 来简单粗暴的解决。在写一个模块的时候我几乎是马上就着手去写，往往都是没有任何思考和设计只是想到什么就写什么，只想着赶快把功能学完，以至于写出很多缝缝补补不合理的代码让我踩过很多坑，收获很多 `bug` ，在看到之前的组件，我的第一想法就是重构。

说了这么多我的血泪史，我们看一下 `React`  哲学到底是说的什么，它都是如何解决我上述的痛点的，我又因此悟到了什么？



### 准备阶段

首先在我们写代码之前肯定会有的是会有的一是 `PM` 的产品设计图，二是后端同学返回的 `JSON` 数据

<img src="/Users/admin/Library/Application Support/typora-user-images/image-20210801203507483.png" alt="image-20210801203507483" style="zoom:50%;" />

```json
[
  {category: "Sporting Goods", price: "$49.99", stocked: true, name: "Football"},
  {category: "Sporting Goods", price: "$9.99", stocked: true, name: "Baseball"},
  {category: "Sporting Goods", price: "$29.99", stocked: false, name: "Basketball"},
  {category: "Electronics", price: "$99.99", stocked: true, name: "iPod Touch"},
  {category: "Electronics", price: "$399.99", stocked: false, name: "iPhone 5"},
  {category: "Electronics", price: "$199.99", stocked: true, name: "Nexus 7"}
];
```

先理解这样一个简单的产品设计图所包含的需求都有哪些，这是一个展示商品的列表，用户可以在对商品进行关键字搜索，并且通过点击复选框选择是是否展示现货，商品列表包含商品名和价格，商品支持分类显示，其中告罄的商品名为红色显示。

当我们基本了解产品图所表达的需求之后，就可以开始代码编写的第一步了



### 通过产品图划分组件层级

在一开始不太熟练划分的时候可以在产品设计稿上通过画方框来确定组件和子组件，可以报组件当成一个函数或者对象来看，组件同样遵照单一功能原则，也就是说一个组件只负责一个功能

同时一个好的 `JSON` 数据模型也应该是和组件一一对应的，组件与数据模型中的某个部分匹配

<img src="/Users/admin/Library/Application Support/typora-user-images/image-20210801205928730.png" alt="image-20210801205928730" style="zoom:50%;" />

不同的颜色划分成不同的组件，可以分成五部分：

1. **`FilterableProductTable` (橙色):** 是整个示例应用的整体
2. **`SearchBar` (蓝色):** 接受所有的*用户输入*
3. **`ProductTable` (绿色):** 展示*数据内容*并根据*用户输入*筛选结果
4. **`ProductCategoryRow` (天蓝色):** 为每一个*产品类别*展示标题
5. **`ProductRow` (红色):** 每一行展示一个*产品*

组件名应该能让人迅速 `get` 到这个组件的写的是什么（不得不说我之前的组件命名真的太糟糕了，过几天回头看都是一脸懵逼的那种

组件的层级划分：

- `FilterableProductTable`
  - `SearchBar`
  - `ProductTable`
    - `ProductCategoryRow`
    - `ProductRow`



### 用 `React` 构建静态页面

当我们划分好了组件层级之后可以来写代码了，先利用已有数据模型来写一个不包含交互的`UI`渲染，这是因为`UI`渲染的代码比较多，交互要考虑的细节比较多，把这两个过程分开写不容易漏掉一些细节，整个思路也比较清晰



通过复用编写的组件，使用 `props` 来进行数据的传递，父组件把数据进行层层的传递，这也是 `React` 的一个特点就是单向数据流动，在这个过程中先不使用 `state` ，因为 `state` 表示的是会随着时间变化而变化的，所以在交互的过程中使用



构建应用的时候可以使用自上而下或者自上而下的方法，自上而下表示先写层级最高的组件，如`FilterableProductTable` 组件，这种比较适合简单的应用； 自下而上表示先写层级最低的组件，如 `ProductRow` 组件，这种方法比较适合大型的应用构建

```jsx
const PRODUCTS = [
  {category: 'Sporting Goods', price: '$49.99', stocked: true, name: 'Football'},
  {category: 'Sporting Goods', price: '$9.99', stocked: true, name: 'Baseball'},
  {category: 'Sporting Goods', price: '$29.99', stocked: false, name: 'Basketball'},
  {category: 'Electronics', price: '$99.99', stocked: true, name: 'iPod Touch'},
  {category: 'Electronics', price: '$399.99', stocked: false, name: 'iPhone 5'},
  {category: 'Electronics', price: '$199.99', stocked: true, name: 'Nexus 7'}
];

const FilterableProductTable = () => (
  <div>
    <SearchBar />
    <ProductTable products={PRODUCTS} />
  </div>
);

const SearchBar = () => (
  <form>
    <input type="text" placeholder="Search..." />
    <p>
      <input type="checkbox" /> Only show products in stock
    </p>
  </form>
);

const ProductTable = ({ products }) => {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category}
        />
      );
      rows.push(<ProductRow product={product} key={product.name} />);
    }
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
};

const ProductCategoryRow = ({ category }) => (
  <tr>
    <td colSpan="2">{category}</td>
  </tr>
);

const ProductRow = ({ product }) => {
  const name = product.stocked ? (
    product.name
  ) : (
    <span style={{ color: "red" }}>{product.name}</span>
  );

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
};

```





### 确定 `state` 的最小且完整的集合

当我们一些其他的数据来触发改变基础数据，让`UI`具有交互结果，在 `React` 中就可以使用 `state` 来表示

最小且完整的表示在于我们可以先找到一些会根据时间产生变化的全部数据，再从这些数据中选出最必要的数据作为 `state` ，其他数据能通过计算得到。



看一下当前应用有哪些数据：

- 商品的原始数据
- 用户的搜索数据
- 复选框是否选中的值
- 经过筛选后的数据



在确定这些数据能否成为 `state` 可以先问一下自己这几个问题

- 数据是否能通过 `prop`s 来传递
- 是否会通过时间而产生改变
- 是否可以通过其他 `state` 和 `props` 计算得到



那么最后我们就可以确认，原始数据可以通过 `props` 传递，用户搜索的数据和复选框的值可以作为 `state `，筛选后的数据可以通过原始数据和用户搜索数据以及复选框数据计算得来。所以最后 `state` 可以是：

- 用户的搜索数据
- 复选框是否选中的值



### 确定 `state` 放置的位置

当确定了 `state` 的最小集合之后，接下来就该确定 `state` 应该放置在哪个组件里

在前面我们知道了 `React`  是单向的数据流，自上而下的流动，所以我们应把 `state` 写在共同所有者（也就是需要这些 `state`  的组件的共同父组件）

我们可以看到 `ProductRow` 组件需要筛选后的数据， `SearchBar` 组件需要搜索的数据和复选框的值， 所以就可以把 `state` 放在它们的共同所有者组件 `FilterableProductTable` 组件里，再通过 `props` 来进行 `state` 的传递





### 添加反向数据流

当我们要通过层级较低的组件改变层级较高的组件，就需要通过反向数据流的方式

`React` 中的反向数据流是通过需要高层级组件通过 `props` 把改变 `state` 的方法 (回调函数) 传递给层级较低的组件，子组件 `state` 的改变后的值传给这个回调函数。

在当前应用中如果想要拿到最新的 `state` 就需要`FilterableProductTable` 必须将一个能够触发 state 改变的回调函数（`callback`）传递给 `SearchBar`。我们可以使用输入框的 `onChange` 事件来监视用户输入的变化，并通知 `FilterableProductTable` 传递给 `SearchBar` 的回调函数。

```jsx
const FilterableProductTable = () => {
  const [filterText, setFilterText] = React.useState("");
  const [inStockOnly, setInStockOnly] = React.useState(false);

  return (
    <div>
      <SearchBar
        filterText={filterText}
        setFilterText={setFilterText}
        inStockOnly={inStockOnly}
        setInStockOnly={setInStockOnly}
      />
      <ProductTable
        products={PRODUCTS}
        inStockOnly={inStockOnly}
        filterText={filterText}
      />
    </div>
  );
};

const SearchBar = ({
  filterText,
  setFilterText,
  inStockOnly,
  setInStockOnly,
}) => {
  const handleProductsSearch = (value) => {
    setFilterText(value);
  };

  const handleStockCheck = (value) => {
    setInStockOnly(value);
  };

  return (
    <form>
      <input
        type="text"
        placeholder="Search..."
        value={filterText}
        onChange={handleProductsSearch}
      />
      <p>
        <input
          type="checkbox"
          value={inStockOnly}
          onChange={handleStockCheck}
        />{" "}
        Only show products in stock
      </p>
    </form>
  );
};

const ProductTable = ({ products, inStockOnly, filterText }) => {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (product.name.indexOf(filterText) === -1) {
      return;
    }
    if (inStockOnly && !product.stocked) {
      return;
    }
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category}
        />
      );
      rows.push(<ProductRow product={product} key={product.name} />);
    }
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
};
```



### 总结

`React` 哲学并没有对深奥的道理，相反它更倡导我们把代码写得更加简洁清晰，更具有模块化，这一点在写大型的项目尤为重要，在写代码之前就把大致的结构和涉及的数据结构设计好，会减少` Bug` 的产生，减少重构的时间，减少维护的成本。