### 对象API

1. Object.assign(target,source1,2,3...)   将可枚举的属性值从一个或多个源对象分配到目标对象上(存在的替换，不存在的添加上），返回目标对象

   ```js
   //合并对象
   const target = { a: 1, b: 2 }
   const source = { b: 4, c: 5 }
   
   const returnedTarget = Object.assign(target, source)  // {a: 1, b: 4, c: 5}
   console.log(target)  //{a: 1, b: 4, c: 5}
   
   //复制对象:只是进行浅拷贝，如果源对象是一个对象的引用只能复制引用值,直接复制可以实现深拷贝
   const copy = Object.assign({}, source) // { b: 4, c: 5 }
   
   //继承属性和不可枚举属性是不能拷贝的
   ```

   

2. Object.create(obj)  用于创建新对象，使用现有对象来提供创建新对象的__proto__，相当与继承，返回值是一个带有指定原型对象和属性的新对象

   ```js
   const person = {
     isHuman: false,
     printIntroduction: function() {
       console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`)
     }
   };
   
   const me = Object.create(person)
   
   me.name = 'Matthew' // "name" is a property set on "me", but not on "person"
   me.isHuman = true // inherited properties can be overwritten
   
   me.printIntroduction()  //"My name is Matthew. Am I human? true"
   ```

   

3. Object.key(obj)  返回一个由一个给定对象的可枚举的属性组成的数组，数组中的属性名的排列顺序和正常循环对象返回的顺序一致。

   ```js
   var arr = ['a', 'b', 'c']
   console.log(Object.keys(arr))  //  ['0', '1', '2']
   
   var obj = { 0: 'a', 1: 'b', 2: 'c' };
   console.log(Object.keys(obj)); // ['0', '1', '2']
   
   var anObj = { 100: 'a', 2: 'b', 7: 'c' }  // {2: "b", 7: "c", 100: "a"}
   console.log(Object.keys(anObj)); // ['2', '7', '100']
   ```

   

4. Object.values(obj)  返回一个由一个给定对象的可枚举的属性值组成的数组，值的顺序与for..in循环的顺序相同

   ```js
   var obj = { foo: 'bar', baz: 42 };
   console.log(Object.values(obj)); // ['bar', 42]
   
   var an_obj = { 100: 'a', 2: 'b', 7: 'c' };
   console.log(Object.values(an_obj)); // ['b', 'c', 'a']
   ```

   