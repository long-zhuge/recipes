## Array


### concat()

- 复制一个全新的数据
- 修改复制的数据不会造成原始数据变化

```
const a = [1,2,3];
const b = a.concat();
console.log(b);
// [1,2,3]
```

- 合并数据
- 合并的数据不会造成原数据的变化

```
const a = [1,2,3];
const b = a.concat([4,5,6]);
console.log(b);
// [1,2,3,4,5,6]
console.log(a);
// [1,2,3]
```

### from()

- 将类数组对象转化为真实的数组

```
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

const arr = Array.from(arrayLike);
console.log(arr);
// ['a','b','c']
```
```
// DOM 对象也是一个类数组对象
const ps = document.querySelectorAll('p');
const nodeList = Array.from(ps);
```

### Set()

- 数组去重，但是不能对数组对象进行去重

```
const arr = [1,2,3,1];
const newArr = new Set(arr);
console.log(newArr);
// [1,2,3]

// 无法对以下数组进行去重
const arr = [{id:1,name:'a'},{id:1,name:'b'}];
```

### some(callback)

- 根据函数给出的匹配要求进行遍历
- 匹配到后返回 true，并终止当前遍历。无匹配项时返回 false
- 不会对空数组进行遍历
- 不会改变原数据

```
var arr = [1,2,3,4,5,6];
arr.some(item => (item > 3));
// true
```

### filter(callback)

- 遍历数组，根据函数要求返回匹配项
- 不会对空数组进行遍历
- 不会改变原数据

```
var arr = [1,2,3,4,5,6];
arr.filter(item => (item > 3));
// [4,5,6]
```

### reduce(callback, initialValue)

- 接收一个函数作为累加器（accumulator），数组中的每个值（从左到右）开始合并，最终为一个值。
- callback(previousValue, currentValue, index, array)
	- previousValue: 上一次调用的返回值，或者是提供的初始值“initialValue”
	- currentValue: 当前调用的值
	- index: 当前值的索引
	- array: 当前值调用的数组

```
const arr = [1,2,3,4];
arr.reduce((previousValue, currentValue, index, array) => {
	retrun previousValue + currentValue;
}, initialValue)
// 10
```

### map(callback)

- 返回一个新数组，数组中的元素为原始数据处理后的值
- 不会对空数组进行遍历
- 不会改变原数据

```
const arr = [1,2,3,4];
arr.map(item => item + 1);
// [2,3,4,5]
```

### forEach(callback)

- 将数组的每项元素传入回调函数
- 不会对空数据进行遍历

```
const arr = [1,2,3,4];
const newArr = [];
arr.forEach(item => newArr.push(item + 1))
console.log(newArr);
// [2,3,4,5]
```

### join

- 把数组的所有元素放入一个字符串。元素通过指定的分隔符进行分隔。

```
const arr = [1,2,3,4];
arr.join('-');
// 1-2-3-4
```

### shift

- 删除并返回数组的第一个元素

```
const arr = [1,2,3,4];
const shift = arr.shift();
console.log(arr, shift);
// [2,3,4], 1
```

### unshift(item, item2, item3, ...)

- 向数组的开头添加一个或更多元素，并返回新的长度。
- 可向数组添加若干个数据

```
const arr = [1,2,3,4];
const unshift = arr.unshift(0,5,6);
console.log(arr, unshift);
// [0,5,6,1,2,3,4], 7
```

### pop

- 删除并返回数组的最后一个元素

```
const arr = [1,2,3,4];
const pop = arr.pop();
console.log(arr, pop);
// [1,2,3], 4
```

### push

- 向数组的末尾添加一个或更多元素，并返回新的长度

```
const arr = [1,2,3,4];
const length = arr.push(5);
console.log(arr, length);
// [1,2,3,4,5], 5
```

### slice(startIndex, endIndex)

- 截取指定位置数据并返回
- 不会更改原数据
- 参数：
	- startIndex：必须，整数
	- endIndex：非必须，整数

```
const arr = [1,2,3,4,5];
const sliceArr = arr.slice(2); // 从下标 2 的位置向后截取
// [3,4,5]
const slice2Arr = arr.slice(2, 3); // 从下标 2 位置开始，3 位置结束进行截取
// [3]
```

### splice(index, howmany, item_n...)

- 想数组中添加/删除数据，并返回被删除对象
- 会对原数据进行更改
- 参数：
	- index：需要添加/删除的位置
	- howmany：删除的个数
	- item_n：需要添加的数据

```
const arr = [1,2,3,4,5];
arr.splice(1, 2);
// [2,3]
console.log(arr);
// [1,4,5]
```

### every()

- 方法用于检测数组所有元素是否都符合指定条件（通过函数提供）
- 如果数组中检测到有一个元素不满足，则整个表达式返回 false ，且剩余的元素不会再进行检测。
- 如果所有元素都满足条件，则返回 true。
- 不会修改原始数据

```
const arr = [1, 2, 3, 4, 5];
arr.every(item => item < 10);
// true

const arr = [1, 11, 3, 4, 5];
arr.every(item => item < 10);
// false
```

### reverse()

- 将数组倒序，会修改原始数据

```
var a = [1,2,3,4,5,6];
a.reverse(); // [6,5,4,3,2,1]
```

### remove(index)

- 无此对象方法，通过 prototype 属性添加一个吧
- 删除数组中指定下标项，并返回新数组
- 不会修改原始数据

```
// 注意：此方法不能修改为箭头函数，否则 this 将不会是当前的数组对象
Array.prototype.remove = function(index) {
  const newArr = [];
  this.forEach((item, key) => {
    if (key !== index) {
      newArr.push(item);
    }
  });

  return newArr;
};

const arr = [1,2,3,4];
arr.remove(2);
// [1,2,4]
console.log(arr);
// [1,2,3,4]
```

### existsSameValues(arr)

- 无此对象方法，通过 prototype 属性添加一个吧
- 判断两个数组中是否有相同项，有则返回相同项，无则返回 false

```
// 注意：此方法不能修改为箭头函数，否则 this 将不会是当前的数组对象
Array.prototype.existsSameValues = function(arr) {
  const newArray = [];
  this.forEach((item) => {
    arr.forEach((item2) => {
      if (item === item2) {
        newArray.push(item);
      }
    });
  });

  return newArray;
};

const arr = [1,2,3,4];
arr.existsSameValues([2,4,5,6]);
// [2,4]
```

### removeId(fieldName, id)

- 无此对象方法，通过 prototype 属性添加一个吧
- 删除数组中的某个对象，并返回新数组
- 不会修改原始数据
- 参数：
	- fieldName：对象字段
	- id：需要删除对象的唯一值

```
// 注意：此方法不能修改为箭头函数，否则 this 将不会是当前的数组对象
Array.prototype.removeId = function(fieldName, id) {
  const newArray = [];
  this.forEach((item) => {
 	 if (item[fieldName] !== id) {
 	   newArray.push(item);
 	 }
  });
  	 
  return newArray;
}

const arr = [{ name: 'Tom' }, { name: 'Sam' }, ...];
arr.removeId('name', 'Tom');
// [{ name: 'Sam' }, ...]
```

### 查询元素的下标

```
Array.prototype.indexVf = function(arr){
  let num = null;
  this.some((item, index) => {
    if (item === arr) {
      num = index;
      return true;
    }
    return false;
  });

  return num;
};

const arr = [1,2,3,4,5];
arr.indexVf(2);
arr.indexVf(6);
// 1
// null
```