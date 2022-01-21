## 一、concat()

concat() 方法用于**连接两个或多个数组**。该方法不会改变现有的数组，仅会返回被连接数组的一个副本。

```js
var arr1 = [1,2,3];
var arr2 = [4,5];
var arr3 = arr1.concat(arr2);
console.log(arr1); //[1, 2, 3]
console.log(arr3); //[1, 2, 3, 4, 5]
```

## 二、join()

join() 方法用于把**数组中的所有元素放入一个字符串**。元素是通过指定的**分隔符**进行分隔的，默认使用','号分割，不改变原数组。

```js
var arr = [2,3,4];
console.log(arr.join());  //2,3,4
console.log(arr);  //[2, 3, 4]
```

## 三、push()

push() 方法可向**数组的末尾添加一个或多个元素**，并返回新的长度。**会改变原数组**。

```js
var a = [2,3,4];
var b = a.push(5);
console.log(a);  //[2,3,4,5]
console.log(b);  //4
push方法可以一次添加多个元素push(data1,data2....)
```

## 四、pop()

pop() 方法用于**删除并返回数组的最后一个元素**。**会改变原数组**。

```js
var arr = [2,3,4];
console.log(arr.pop()); //4
console.log(arr);  //[2,3]
```

## 五、shift()

shift() 方法用于**把数组的第一个元素从其中删除，并返回第一个元素的值**。**`改变原数组`**。

```
var arr = [2,3,4];
console.log(arr.shift()); //2
console.log(arr);  //[3,4]
```

## 六、unshift()

unshift() 方法可向**数组的开头添加一个或更多元素，并返回新的长度**。**`返回新长度，改变原数组`**。

```js
var arr = [2,3,4,5];
console.log(arr.unshift(3,6)); //6
console.log(arr); //[3, 6, 2, 3, 4, 5]
tip:该方法可以不传参数,不传参数就是不增加元素。
```

## 七、slice()

**返回一个新的数组，包含从 start 到 end （不包括该元素）的 arrayObject 中的元素**。返回选定的元素，该方法不会修改原数组。

```js
var arr = [2,3,4,5];
console.log(arr.slice(1,3));  //[3,4]
console.log(arr);  //[2,3,4,5]
```

## 八、splice()

splice() 方法可**删除从 index 处开始的零个或多个元素**，并且用参数列表中**声明的一个或多个值来替换那些被删除的元素**。如果从 arrayObject 中删除了元素，则返回的是含有被删除的元素的数组。splice() 方法会**直接对数组进行修改。**

```js
var a = [5,6,7,8];
console.log(a.splice(1,0,9)); //[]
console.log(a);  //[5, 9, 6, 7, 8]
var b = [5,6,7,8];
console.log(b.splice(1,2,3));  //[6, 7]
console.log(b); //[5, 3, 8]
```

## 九、sort 排序

> **`sort()`** 方法用原地算法对**数组的元素进行排序，并返回数组**。默认排序顺序是在将元素转换为字符串，然后比较它们的UTF-16代码单元值序列 由于它取决于具体实现，因此无法保证排序的时间和空间复杂性。
>
> ```
> arr.sort([compareFunction]) 
> ```
>
> ### 参数
>
> - `compareFunction` 可选
>
>   用来指定按某种顺序进行排列的函数。如果省略，元素按照转换为的字符串的各个字符的Unicode位点进行排序。`firstEl`第一个用于比较的元素。`secondEl`第二个用于比较的元素。
>
> ### 返回值
>
> 排序后的数组。请注意，数组已原地排序，并且不进行复制。
>
> ### 描述
>
> 如果没有指明 `compareFunction` ，那么元素会按照转换为的字符串的诸个字符的Unicode位点进行排序。例如 "Banana" 会被排列到 "cherry" 之前。当数字按由小到大排序时，9 出现在 80 之前，但因为（没有指明 `compareFunction`），比较的数字会先被转换为字符串，所以在Unicode顺序上 "80" 要比 "9" 要靠前。
>
> 如果指明了 `compareFunction` ，那么数组会按照调用该函数的返回值排序。即 a 和 b 是两个将要被比较的元素：
>
> - 如果 `compareFunction(a, b)` 小于 0 ，那么 a 会被排列到 b 之前；
> - 如果 `compareFunction(a, b)` 等于 0 ， a 和 b 的相对位置不变。备注： ECMAScript 标准并不保证这一行为，而且也不是所有浏览器都会遵守（例如 Mozilla 在 2003 年之前的版本）；
> - 如果 `compareFunction(a, b)` 大于 0 ， b 会被排列到 a 之前。
> - `compareFunction(a, b)` 必须总是对相同的输入返回相同的比较结果，否则排序的结果将是不确定的。

## 十、reverse()

reverse() 方法用于**颠倒数组中元素的顺序。返回的是颠倒后的数组**，**会改变原数组**。

```js
var arr = [2,3,4];
console.log(arr.reverse()); //[4, 3, 2]
console.log(arr);  //[4, 3, 2]
```

## 十一、indexOf 和 lastIndexOf

都接受两个参数：**查找的值、查找起始位置 不存在，返回 -1 ；存在，返回位置**。**indexOf 是从前往后查找， lastIndexOf 是从后往前查找**。

 **indexOf**

```js
var array = [2, 5, 9];
array.indexOf(2);     // 0
array.indexOf(7);     // -1
array.indexOf(9, 2);  // 2
array.indexOf(2, -1); // -1
array.indexOf(2, -3); // 0
```

**lastIndexOf**

```js
var numbers = [2, 5, 9, 2];
numbers.lastIndexOf(2);     // 3
numbers.lastIndexOf(7);     // -1
numbers.lastIndexOf(2, 3);  // 3
numbers.lastIndexOf(2, 2);  // 0
numbers.lastIndexOf(2, -2); // 0
numbers.lastIndexOf(2, -1); // 3
```

## 十二、every()

对**数组的每一项都运行给定的函数，每一项都返回 ture,则返回 true**

```js
function isBigEnough(element, index, array) {
  return element < 10;
}    
[2, 5, 8, 3, 4, 18].every(isBigEnough);   // false
```

## 十三、some()

对数组的每一项都运行给定的函数，**任意一项**返回 ture,则返回 true

```js
function compare(element, index, array) {
  return element > 10;
}    
[2, 5, 8, 1, 4].some(compare);  // false
[12, 5, 8, 1, 4].some(compare); // true
```

## 十四、filter()

对数组的每一项都运行给定的函数，返回 **结果为 ture 的项组成的数组**

```js
var words = ["spray", "limit", "elite", "exuberant", "destruction", "present", "happy"];

var longWords = words.filter(function(word){
  return word.length > 6;
});
// Filtered array longWords is ["exuberant", "destruction", "present"]
```

## 十五、map()

对数组的每一项都运行给定的函数，返回**每次函数调用的结果组成一个新数组**

```js
var numbers = [1, 5, 10, 15];
var doubles = numbers.map(function(x) {
   return x * 2;
});
// doubles is now [2, 10, 20, 30]
// numbers is still [1, 5, 10, 15]
```

## 十六、forEach ()

```js
const items = ['item1', 'item2', 'item3'];
const copy = [];    
items.forEach(function(item){
  copy.push(item)
});
```

## 十七、reduce()

**`reduce()`** 方法对数组中的每个元素执行一个由您提供的**reducer**函数(升序执行)，将其**结果汇总为单个返回值**。

> ```
> arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])
> ```
>
> ### 参数
>
> - `callback`
>
>   执行数组中每个值 (如果没有提供 `initialValue则第一个值除外`)的函数，包含四个参数：
>
>   **`accumulator`**累计器累计回调的返回值; 它是上一次调用回调时返回的累积值，或`initialValue`（见于下方）。`currentValue`数组中正在处理的元素。`index` 可选数组中正在处理的当前元素的索引。 如果提供了`initialValue`，则起始索引号为0，否则从索引1起始。`array`可选调用`reduce()`的数组
>
> - `initialValue`可选
>
>   作为第一次调用 `callback`函数时的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错。
>
> ### 返回值
>
> 函数累计处理的结果

```js
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
// expected output: 10

console.log(array1.reduce((acc, cur) => acc + cur, 10))
// expected output: 20
```



## ES6新增新操作数组的方法

## 1、find()

传入一个**回调函数，找到数组中符合当前搜索规则的第一个元素，返回它，并且终止搜索**。

```js
const arr = [1, "2", 3, 3, "2"]
console.log(arr.find(n => typeof n === "number")) // 1
```

## 2、findIndex()

传入**一个回调函数，找到数组中符合当前搜索规则的第一个元素，返回它的下标，终止搜索**。

```js
const arr = [1, "2", 3, 3, "2"]
console.log(arr.findIndex(n => typeof n === "number")) // 0
```

## 3、fill()

用**新元素替换掉数组内的元素，可以指定替换下标范围**。

```js
arr.fill(value, start, end)

[1, 2, 3].fill(4);               // [4, 4, 4]
[1, 2, 3].fill(4, 1);            // [1, 4, 4]
[1, 2, 3].fill(4, 1, 2);         // [1, 4, 3]
[1, 2, 3].fill(4, 1, 1);         // [1, 2, 3]
[1, 2, 3].fill(4, 3, 3);         // [1, 2, 3]
[1, 2, 3].fill(4, -3, -2);       // [4, 2, 3]
[1, 2, 3].fill(4, NaN, NaN);     // [1, 2, 3]
[1, 2, 3].fill(4, 3, 5);         // [1, 2, 3]
Array(3).fill(4);                // [4, 4, 4]
[].fill.call({ length: 3 }, 4);  // {0: 4, 1: 4, 2: 4, length: 3}

// Objects by reference.
var arr = Array(3).fill({}) // [{}, {}, {}];
// 需要注意如果fill的参数为引用类型，会导致都执行同一个引用类型
// 如 arr[0] === arr[1] 为true
arr[0].hi = "hi"; // [{ hi: "hi" }, { hi: "hi" }, { hi: "hi" }]
```

## 4、copyWithin()

**选择数组的某个下标，从该位置开始复制数组元素**，默认从0开始复制。也可以指定要复制的元素范围。

```js
arr.copyWithin(target, start, end)
const arr = [1, 2, 3, 4, 5]
console.log(arr.copyWithin(3))
 // 1,2,3,4,5
 // [1,2,3,1,2] 从下标为3的元素开始，复制数组，所以4, 5被替换成1, 2
const arr1 = [1, 2, 3, 4, 5]
// 2,3,4,5
console.log(arr1.copyWithin(3, 1)) 
// [1,2,3,2,3] 从下标为3的元素开始，复制数组，指定复制的第一个元素下标为1，所以4, 5被替换成2, 3
const arr2 = [1, 2, 3, 4, 5]
// 2
console.log(arr2.copyWithin(3, 1, 2)) 
// [1,2,3,2,5] 从下标为3的元素开始，复制数组，指定复制的第一个元素下标为1，结束位置为2，所以4被替换成2
```

## 5、from()

**将类似数组的对象（array-like object）和可遍历（iterable）的对象转为真正的数组**

```js
const bar = ["a", "b", "c"];
Array.from(bar);
// ["a", "b", "c"]

Array.from('foo');
// ["f", "o", "o"]

/*从 Map 生成数组*/
const map = new Map([[1, 2], [2, 4], [4, 8]]);
Array.from(map);
// [[1, 2], [2, 4], [4, 8]]

const mapper = new Map([['1', 'a'], ['2', 'b']]);
Array.from(mapper.values());
// ['a', 'b'];

Array.from(mapper.keys());
// ['1', '2'];

/*从Set生成数组*/
const set = new Set(['foo', 'bar', 'baz', 'foo']);
Array.from(set);
// [ "foo", "bar", "baz" ]
```

## 6、of()

**用于将一组值，转换为数组。**这个方法的主要目的，是弥补数组构造函数 Array() 的不足。因为参数个数的不同，会导致 Array() 的行为有差异。

```
Array() // []
Array(3) // [ , , ]
Array(3, 11, 8) // [3, 11, 8]

Array.of(7);       // [7]
Array.of(1, 2, 3); // [1, 2, 3]

Array(7);          // [ , , , , , , ]
Array(1, 2, 3);    // [1, 2, 3]
```

## 7、entries() 

**返回迭代器：返回键值对**

```js
//数组
const arr = ['a', 'b', 'c'];
for(let v of arr.entries()) {
  console.log(v)
}
// [0, 'a'] [1, 'b'] [2, 'c']

//Set
const arr = new Set(['a', 'b', 'c']);
for(let v of arr.entries()) {
  console.log(v)
}
// ['a', 'a'] ['b', 'b'] ['c', 'c']

//Map
const arr = new Map();
arr.set('a', 'a');
arr.set('b', 'b');
for(let v of arr.entries()) {
  console.log(v)
}
// ['a', 'a'] ['b', 'b']
```

## 8、values() 

**返回迭代器：返回键值对的value**

```js
//数组
const arr = ['a', 'b', 'c'];
for(let v of arr.values()) {
  console.log(v)
}
//'a' 'b' 'c'

//Set
const arr = new Set(['a', 'b', 'c']);
for(let v of arr.values()) {
  console.log(v)
}
// 'a' 'b' 'c'

//Map
const arr = new Map();
arr.set('a', 'a');
arr.set('b', 'b');
for(let v of arr.values()) {
  console.log(v)
}
// 'a' 'b'
```

## 9、keys() 

**返回迭代器：返回键值对的key**

```js
//数组
const arr = ['a', 'b', 'c'];
for(let v of arr.keys()) { // [0,1,2]
  console.log(v)
}
// 0 1 2

//Set
const arr = new Set(['a', 'b', 'c']);
for(let v of arr.keys()) { // 0: a .....
  console.log(v)
}
// 'a' 'b' 'c'

//Map
const arr = new Map();
arr.set('a', 'a');
arr.set('b', 'b');
// arr=>[['a','a'],['b',b]]
for(let v of arr.keys()) {
  console.log(v)
}
// 'a' 'b'
```

## 10、includes()

**判断数组中是否存在该元素，参数：查找的值、起始位置**，可以替换 ES5 时代的 **indexOf** 判断方式。indexOf 判断元素是否为 NaN，会判断错误。

```js
var a = [1, 2, 3];
a.includes(2); // true
a.includes(4); // false

var a = [1,2,NaN]
a.includes(NaN) // true
a.indexOf(NaN)  // -1
```

## 11、isArray()

**Array.isArray()** 用于确定传递的值是否是一个 Array

```js
Array.isArray([1, 2, 3]);  
// true
Array.isArray({foo: 123}); 
// false
Array.isArray("foobar");   
// false
Array.isArray(undefined);  
// false
```

## 12、flat()

**`flat()`** 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。

> ```
> var newArray = arr.flat([depth])
> ```
>
> ### 参数
>
> - `depth` 可选
>
>   指定要提取嵌套数组的结构深度，默认值为 1。
>
> ### 返回值
>
> 一个包含将数组与子数组中所有元素的新数组。

```js
var arr1 = [1, 2, [3, 4]];
arr1.flat(); 
// [1, 2, 3, 4]

var arr2 = [1, 2, [3, 4, [5, 6]]];
arr2.flat();
// [1, 2, 3, 4, [5, 6]]

var arr3 = [1, 2, [3, 4, [5, 6]]];
arr3.flat(2);
// [1, 2, 3, 4, 5, 6]

//使用 Infinity，可展开任意深度的嵌套数组
var arr4 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]];
arr4.flat(Infinity);
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

## 13、flatMap()

**`flatMap()`** 方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。它与 map 连着深度值为1的 flat 几乎相同，但 `flatMap` 通常在合并成一种方法的效率稍微高一些。

> ```
> var new_array = arr.flatMap(function callback(currentValue[, index[, array]]) {
> // return element for new_array
> }[, thisArg])
> ```
>
> ### 参数
>
> - `callback`
>
>   可以生成一个新数组中的元素的函数，可以传入三个参数：`currentValue`当前正在数组中处理的元素`index`可选可选的。数组中正在处理的当前元素的索引。`array`可选可选的。被调用的 `map` 数组
>
> - `thisArg`可选
>
>   可选的。执行 `callback` 函数时 使用的`this` 值。
>
> ### 返回值
>
> 一个新的数组，其中每个元素都是回调函数的结果，并且结构深度 `depth` 值为1。

```js
var arr1 = [1, 2, 3, 4];

arr1.map(x => [x * 2]); 
// [[2], [4], [6], [8]]

arr1.flatMap(x => [x * 2]);
// [2, 4, 6, 8]

// only one level is flattened
arr1.flatMap(x => [[x * 2]]);
// [[2], [4], [6], [8]]
```