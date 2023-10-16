---
link: https://juejin.cn/post/7277799790296416290
title: lodash-实用库及常用方法20个CSS性能优化技巧，每个前端都需要知道各位微信小程序的开发者们，你们还好吗？深入理解 TypeScript 工具函数：一起学习，一起进步！做了几年前端，别跟我说没配置过webpack
description: 前言 在日常的前端开发中，总是涉及到对数据的处理，比如后端返给你一坨数据，你需要进行处理并回显到页面上，又或者提交form表单到服务端时，你需要将数据处理成后端接口定义的数据结构，而这些都离不开数据处
keywords: 前端,JavaScript,ECMAScript 6
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2023-09-12T09:05:01.000Z
publisher: 稀土掘金
stats: paragraph=147 sentences=107, words=937
---
在日常的前端开发中，总是涉及到对 `&#x6570;&#x636E;&#x7684;&#x5904;&#x7406;`，比如后端返给你一坨数据，你需要进行处理并回显到页面上，又或者 `&#x63D0;&#x4EA4;form&#x8868;&#x5355;`到服务端时，你需要将数据处理成 `&#x540E;&#x7AEF;&#x63A5;&#x53E3;&#x5B9A;&#x4E49;&#x7684;&#x6570;&#x636E;&#x7ED3;&#x6784;`，而这些都离不开数据处理。

那数据处理有什么好用的 `&#x5DE5;&#x5177;&#x5E93;`吗？ `lodash`当之无愧。

## lodash使用

使用:

```js

"lodash.js"</span>>

npm i --save lodash
```

接下来给大家介绍下我平时开发用lodash `&#x6700;&#x6700;&#x6700;&#x5E38;&#x7528;`的一些方法。

**作用**：剔除掉数组中的 `&#x5047;&#x503C;`（ `&#x5047;&#x503C;`包括 `false`, `null`, `0`, `""`, `undefined`, 和 `NaN`这5个）元素，并返回一个新数组。

**使用示例**：

```js
const _ = require('lodash')
console.log(_.compact([0, 1, false, 2, '', 3, undefined, 4, null, 5]));

```

**项目中的应用**：剔除数组中的一些脏数据。

**作用**：过滤掉数组中的指定元素，并返回一个新数组

**使用示例**：

```js
const _ = require('lodash')
console.log(_.difference([1, 2, 3], [2, 4]))

const arr = [1, 2], obj = { a: 1 }
console.log(_.difference([1, arr, [3, 4], obj, { a: 2 }], [1, arr, obj]))

```

**类似方法**：

* `_.pull(array, [values])`，与 `_.difference`不同之处在于 `_.pull`会改变原数组。
* `_.without(array, [values])`: 剔除所有给定值，并返回一个新数组，这个方法的作用和 `_.difference`相同。

**项目中的应用**：这个可以在某些场景代替掉 `Array.prototype.filter`。

**作用**：返回数组的 `&#x6700;&#x540E;&#x4E00;&#x4E2A;&#x5143;&#x7D20;`

```js
console.log(_.last([1, 2, 3, 4, 5]))

```

**项目中的应用**：有了这个方法，就不需要通过 `arr[arr.length - 1]`这样去取数组的最后一项了，比如一个省市区 `&#x7EA7;&#x8054;&#x9009;&#x62E9;&#x5668;Cascader`，但传给后端的时候只需要最后一级的id，所以直接用 `_.last`取最后一项给后端。

**类似方法**：

* `_.head(aray)`方法，返回 `&#x6570;&#x7EC4;&#x7684;&#x7B2C;&#x4E00;&#x9879;`。
* `_.tail(array)`方法，返回 `&#x9664;&#x4E86;&#x6570;&#x7EC4;&#x7B2C;&#x4E00;&#x9879;&#x4EE5;&#x5916;&#x7684;&#x5168;&#x90E8;&#x5143;&#x7D20;`。

顺便一提，我在实际开发项目中还遇到过用数组的 `pop`方法去取最后一项的，然后由于取了两次调用了两次 `pop`，造成了一个 `bug`，让人哭笑不得。

**作用**：将数组按给定的 `size`进行区块拆分，多余的元素会被拆分到 `&#x6700;&#x540E;&#x4E00;&#x4E2A;&#x533A;&#x5757;`当中，返回值是一个二维数组。

**使用示例**：

```js
console.log(_.chunk([1, 2, 3, 4, 5], 2))

```

**项目中的应用**：比如你需要渲染出一个 `xx&#x884C;xx&#x5217;`的布局，你就可以用这个方法将数据变变成一个 `&#x4E8C;&#x7EF4;&#x6570;&#x7EC4;arr`， `arr.length`代表 `&#x884C;&#x6570;`, `arr[0].length`代表 `&#x5217;&#x6570;`。

**作用**：从对象中获取路径 `path`的值，如果获取值为 `undefined`，则用 `defaultValue`代替。

**使用示例**：

```js
const _ = require('lodash')
const object = { a: { b: [{ c: 1 }, null] }, d: 3 };

console.log(_.get(object, 'a.b[0].c'));

console.log(_.get(object, ['a', 'b', 1], 4));

console.log(_.get(object, 'e', 5));

```

**项目中的应用**：这个是获取数据的 `&#x795E;&#x5668;`，再也不用写出 `if(a && a.b && a.b.c)`的这种代码了，直接用 `_.get(a, 'b.c')`搞定， `_.get`里面会帮你做判断，绝对省事！

**作用**：判断对象上是否有路径 `path`的值，不包括 `&#x539F;&#x578B;`。

**使用示例**：

```js
const _ = require('lodash')
const obj = { a: 1 };
const obj1 = { b: 1 }

const obj2 = Object.create(obj1)

console.log(_.has(obj, 'a'));

console.log(_.has(obj2, 'b'));

```

**项目中的应用**：这个可以代替 `Object.prototype.hasOwnProperty`，判断对象上有没有某个属性。

**作用**：遍历并修改对象的key值，并返回一个新对象。

**使用示例**：

```js
const _ = require('lodash')
const obj = { a: 1, b: 1 };

const res = _.mapKeys(obj, (value, key) => {
    return key + value;
})
console.log(res)

```

**项目中的应用**：调接口传递给后端数据时，如果定义的key和后端接口数据结构定义的key不匹配，可以用 `_.mapKeys`进行适配。

**作用**：遍历并修改对象的value值，并返回一个新对象。

**使用示例**：

```js
const _ = require('lodash')
const obj = { a: { age: 1 }, b: { age: 2 } };

const res = _.mapValues(obj, (value) => {
    return value.age;
})
console.log(res)

```

**项目中的应用**：依次对对象values值进行处理，进行数据格式化，以适配后端接口。

**作用**：从object中挑出对应的属性，并组成一个新对象

**使用示例**：

```js
const _ = require('lodash')
const obj = { a: 1, b: 2, c: 3 };

const res = _.pick(obj, ['a', 'b'])
console.log(res)

```

**项目中的应用**：从后端接口中，pick出对应你需要用的值，然后进行逻辑处理和页面渲染，或者pick对应的值，传给后端。

**作用**：与 `_.pick`类似，只是第二个参数是一个函数，当返回为真时才会被 `pick`

**使用示例**：

```js
const _ = require('lodash')
const obj = { a: 1, b: 2, c: 3 };

const res = _.pickBy(obj, (val, key) => val === 2)
console.log(res)

```

**项目中的应用**：是 `_.pick`的增强版，可以实现 `&#x52A8;&#x6001;pick`。

**作用**：_.pick的 `&#x53CD;&#x5411;&#x7248;`，忽略掉某些属性后，剩下的属性组成一个新对象。

**使用示例**：

```js
const _ = require('lodash')
const obj = { a: 1, b: 2, c: 3 };

const res = _.omit(obj, ['b'])
console.log(res)

```

**项目中的应用**：代替 `delete obj.xx`，剔除某些属性。

**作用**：_.omit的 `&#x589E;&#x5F3A;&#x7248;`，第二个参数是一个函数，当返回为真时才会被 `omit`

**使用示例**：

```js
const _ = require('lodash')
const obj = { a: 1, b: 2, c: 3, cc: 4 };

const res = _.omitBy(obj, (val, key) => {
    return key.includes('c');
} )
console.log(res)

```

**项目中的应用**：与 `_.omit`类似。

**作用**：给object上对应的path设置值，路径不存在会自动创建，索引创建成数组，其它创建为对象。

**使用示例**：

```js
const _ = require('lodash')
const obj = { };

const res = _.set(obj, ['a', '0', 'b'], 1)
console.log(res)

const res1 = _.set(obj, 'a.1.c', 2)
console.log(res1)

```

**项目中的应用**：给对象设置值，再也不用设置的时候一层层判断了。

**作用**：与 `_.set`相反，删除object上对应的path上的值，删除成功返回true，否则返回false

**使用示例**：

```js
const _ = require('lodash')
const obj = { a: [{ b: 2 }] }

const res = _.unset(obj, ['a', '0', 'b'])
console.log(res)

const res1 = _.unset(obj, ['a', '1', 'c'])
console.log(res1)

```

**项目中的应用**：给对象删除值，替换 `delete a.b.c`。使用delete如果在访问 `a.b.c`的时候，发现没有 `b`属性就会报错，而 `_.unset`不会报错，有更加好的容错处理。

**作用**：标准的 `&#x6DF1;&#x62F7;&#x8D1D;`函数，这个无须多言，用过的人都说好

**使用示例**：

```js
const _ = require('lodash')
const obj = { a: [{ b: 2 }] }

const res = _.cloneDeep(obj)
console.log(res)

```

**项目中的应用**：代替 `JSON.parse(JSON.string(obj))`等深拷贝方法，能处理 `&#x5FAA;&#x73AF;&#x5F15;&#x7528;`，有更好的兼容性。

**作用**：深度比较两者的值是否相等

**使用示例**：

```js
const _ = require('lodash')
const obj = { a: [{ b: 2 }] }
const obj1 = { a: [{ b: 2 }] }

const res = _.isEqual(obj, obj1)
console.log(res)

```

**项目中的应用**：比较form表单前后的数据是否发生了变化，再也不用自己 `&#x5FAA;&#x73AF;&#x4E24;&#x6B21;+&#x9012;&#x5F52;`去手动比较了。

**作用**：某个值是 `null`或者 `undefined`

**使用示例**：

```js
const _ = require('lodash')
let a = null;

const res = _.isNil(a)
console.log(res)

```

**项目中的应用**：有时候我们并不想用 `if(obj.xx)`判断是否有值，因为 `0`也是算有值的，而且可能在后端定义中还有含义，但它转成boolean去判断却是false，所以我们用 `_.isNil`去判断更为准确。

**作用**：标准的 `&#x9632;&#x6296;`函数，简单理解就是，函数被触发多次，只有最后一次会被触发

**使用示例**：

```js
const _ = require('lodash')

const fn = () => ({
   fetch('https://xxx.cn/api')
})
const res = _.debounce(fn, 3000)
```

**项目中的应用**： `input`输入框的实时搜索，减少接口调用，节约服务器资源。

**作用**：标准的 `&#x8282;&#x6D41;`函数，简单理解就是，函数被触发多次，在指定时间范围内只会调用一次

**使用示例**：

```js
const _ = require('lodash')

const fn = () => ({
   fetch('https://xxx.cn/api')
})
const res = _.throttle(fn, 300)
```

**项目中的应用**：监听页面 `scroll`事件滚动加载，监听页面的 `resize`事件等。

**作用**：判断一个对象/数组/map/set是否为空

**使用示例**：

```js
const _ = require('lodash')

const obj = {}
const res = _.isEmpty(obj);
console.log(res)

```

**项目中的应用**：对传入的数据做非空校验。

**作用**：传入一个函数数组，并返回一个新函数。 `_.flow`内部从左到右依次调用数组中的函数，上一次函数的返回的结果，会作为下个函数调用的入参

**使用示例**：

```js
const _ = require('lodash')

const add = (a, b) => a + b;
const multi = (a) => a * a;
const computerFn = _.flow([add, multi]);
console.log(computerFn(1, 2))

```

**项目中的应用**：我们可以把各种工具方法进行抽离，然后用 `_.flow`自由组装成新的工具函数，帮助我们 `&#x6D41;&#x5F0F;&#x5904;&#x7406;`数据，有点 `&#x51FD;&#x6570;&#x5F0F;&#x7F16;&#x7A0B;`那味儿了。

**作用**：与 `_.flow`相反，函数会从右到左执行，相当于React中的 `compose`函数

**使用示例**：

```js
const _ = require('lodash')

const add = (a) => a + 3;
const multi = (a) => a * a;
const computerFn = _.flowRight([add, multi]);
console.log(computerFn(4))

```

**项目中的应用**：与 `_.flow`类似，遇到相关场景，用 `flow`还是 `flowRight`都行，看个人习惯。

以上就是我个人在项目中常用的 `lodash&#x65B9;&#x6CD5;`了，使用体验是非常好的，节约了不少处理数据的时间，所以想分享给大家。

大家熟练用起来， `&#x6478;&#x9C7C;&#x65F6;&#x95F4;`这不就有了么!
