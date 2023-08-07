### [计数器 II](https://leetcode.cn/problems/counter-ii/)

##### 题目描述

请你写一个函数 `createCounter`. 这个函数接收一个初始的整数值 `init` 并返回一个包含三个函数的对象。

这三个函数是：

- `increment()` 将当前值加 1 并返回。
- `decrement()` 将当前值减 1 并返回。
- `reset()` 将当前值设置为 `init` 并返回。

##### 示例1

```js
输入：init = 5, calls = ["increment","reset","decrement"]
输出：[6,5,4]
解释：
const counter = createCounter(5);
counter.increment(); // 6
counter.reset(); // 5
counter.decrement(); // 4
```

##### 示例2

```js
输入：init = 0, calls = ["increment","increment","decrement","reset","reset"]
输出：[1,2,1,0,0]
解释：
const counter = createCounter(0);
counter.increment(); // 1
counter.increment(); // 2
counter.decrement(); // 1
counter.reset(); // 0
counter.reset(); // 0
```

### 我的思路

这题主要考察JS中闭包的运用

完整代码如下：

```js
/**
 * @param {integer} init
 * @return { increment: Function, decrement: Function, reset: Function }
 */
var createCounter = function (init) {
    let temp = init;
    return {
      increment() {
        return (temp += 1);
      },
      decrement() {
        return (temp -= 1);
      },
      reset() {
        return temp=init;
      },
    };
  };
```

