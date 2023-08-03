### 过滤数组中的元素

##### 题目描述

给定一个整数数组 `arr` 和一个过滤函数 `fn`，并返回一个过滤后的数组 `filteredArr` 。

`fn` 函数接受一个或两个参数：

- `arr[i]` - `arr` 中的数字
- `i` - `arr[i]` 的索引

`filteredArr` 应该只包含使表达式 `fn(arr[i], i)` 的值为 **真值** 的 `arr` 中的元素。**真值** 是指 `Boolean(value)` 返回参数为 `true` 的值。

请在不使用内置的 Array.filter 方法的情况下解决该问题。

##### 示例1

```js
输入：arr = [0,10,20,30], fn = function greaterThan10(n) { return n > 10; }
输出： [20,30]
解释：
const newArray = filter(arr, fn); // [20, 30]
过滤函数过滤掉不大于 10 的值
```

##### 示例2

```js
输入：arr = [-2,-1,0,1,2], fn = function plusOne(n) { return n + 1 }
输出：[-2,0,1,2]
解释：
像 0 这样的假值应被过滤掉
```

### 我的思路

直接用for循环破招

```js
let temp = [];
  for (let i = 0; i < arr.length; i++) {
    if (fn(arr[i], i)) {
      temp.push(arr[i]);
    }
  }
  return temp;
```

还有一种方式，万变不离其宗，使用reduce

```js
return arr.reduce((prev, cur, index) => {
    fn(cur, index) && prev.push(cur);
    return prev;
  }, []);
```

完整代码如下：

```js
/**
 * @param {number[]} arr
 * @param {Function} fn
 * @return {number[]}
 */
// 法一
/* var filter = function (arr, fn) {
  let temp = [];
  for (let i = 0; i < arr.length; i++) {
    if (fn(arr[i], i)) {
      temp.push(arr[i]);
    }
  }
  return temp;
}; */

// 法二
var filter = function (arr, fn) {
  return arr.reduce((prev, cur, index) => {
    fn(cur, index) && prev.push(cur);
    return prev;
  }, []);
};
```

