## 字符串操作API



### split ()

用于分割字符串，将一个String对象拆分成子字符数组，指定一个分割字符来确定分割的位置。

```js
const str = 'The quick brown fox jumps over the lazy dog.';

const words = str.split(' ');
console.log(words[3]);
// expected output: "fox"

const chars = str.split('');
console.log(chars[8]);
// expected output: "k"

const strCopy = str.split();
console.log(strCopy);
// expected output: Array ["The quick brown fox jumps over the lazy dog."]
```

### join()

用于将数组或一个类数组转化成字符串并返回这个字符串，如果只有一个数组就不用指定分隔符

```js
const elements = ['Fire', 'Air', 'Water'];

console.log(elements.join());
// expected output: "Fire,Air,Water"


console.log(elements.join(''));
// expected output: "FireAirWater"

console.log(elements.join('-'));
// expected output: "Fire-Air-Water"
```



#### Object.assign()

 方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。

```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };

const returnedTarget = Object.assign(target, source);

console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }

console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
```

#### set

`Set`对象是值的集合，你可以按照插入的顺序迭代它的元素。 Set中的元素只会**出现一次**，即 Set 中的元素是唯一的。

Set( )  创建一个新的`Set`对象。

Set.prototype.keys( )  与**`values()`**方法相同，返回一个新的迭代器对象，该对象包含`Set`对象中的按插入顺序排列的所有元素的值。

```tsx
 const keys = [...new Set(regions.map((item) => item.continent))]
 console.log('keys:', keys)  //keys: ["North America", "South America", "Europe", "Middle East", "Africa", "Asia Pacific"]
```



#### Map

**`Map`** 对象保存键值对，并且能够记住键的原始插入顺序。任何值(对象或者[原始值] 都可以作为一个键或一个值。

一个Map对象在迭代时会根据对象中元素的插入顺序来进行  一个  `for...of` 循环在每次迭代后会返回一个形式为[key，value]的数组。

`Map.prototype.get(key)`返回键对应的值，如果不存在，则返回undefined

`Map.prototype.keys()`返回一个新的 `Iterator`对象， 它按插入顺序包含了Map对象中每个元素的**键** 。

```ts
export const VENDOR_MAP = new Map<VendorKey, VendorSt>([
  [
    VendorKey.AWS,
    {
      vendor: VendorKey.AWS,
      name: 'Amazon AWS',
      logo: 'amazon_aws.svg',
      disabled: false,
    },
  ],
  [
    VendorKey.GCP,
    {
      vendor: VendorKey.GCP,
      name: 'Google Cloud',
      logo: 'google_cloud.svg',
      disabled: true,
    },
  ],
])

export const VENDOR_KEYS = [...VENDOR_MAP.keys()]
```

