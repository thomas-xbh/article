---
link: null
title: @loadable/components是如何实现懒加载的
description: 先看下官方文档是如何使用@loadable/component： loadable(() => import('./OtherComponent')) 的返回值是一个Reac...
keywords: null
author: null
date: 2022-04-24T02:45:53.000Z
publisher: 简书
stats: paragraph=49 sentences=60, words=433
---
先看下官方文档是如何使用 `@loadable/component`：

```
import loadable from '@loadable/component'
const OtherComponent = loadable(() => import('./OtherComponent'))
function MyComponent() {
  return (

  )
}
```

`loadable(() => import('./OtherComponent'))` 的返回值是一个React组件。那究竟是如何做到懒加载的？

本文不会讲解 `import('./OtherComponent')`如何分包，如果想了解请自行查阅资料。

## createLoadable

源码中 `loadable`由 `createLoadable`生成。 `createLoadble`接受 `defaultResolveComponent`和 `render`两个参数，执行后返回 `loadable`：

```
import React from 'react'
import createLoadable from './createLoadable'
import { defaultResolveComponent } from './resolvers'

export const { loadable, lazy } = createLoadable({
  defaultResolveComponent,
  render({ result: Component, props }) {
    return <component {...props}>
  },
})
</component>
```

至于 `defaultResolveComponent`这里先不用管，下文使用到会介绍。

接下来就是最核心的 `createLoadable` ，它的代码框架如下:

`createLoadable`内部定义并返回了 `loadable`和 `lazy`两个函数。其中我们的关注重点是官方文档提到的 `loadable`。

## loadable

loadable有2个参数：

首先， `loadable`会先执行 `resolveConstructor`，对构造函数包装：

```
const ctor = resolveConstructor(loadableConstructor);
```

包装后，ctor的值如下：

```
const ctor = {
    requireAsync: () => import('./OtherComponent'),
    resolve() {
        return undefined
    },
    chunkName() {
        return undefined
    }
}
```

编译后的ctor值：

在 `loadable`内部还定义了一个类组件 `InnerLoadable`：

```
  class InnerLoadable extends React.Component {
      constructor(props) {
        super(props)

        this.state = {
          result: null,
          error: null,
          loading: true,
          cacheKey: getCacheKey(props),
        }
      }
      // ........

   }

    const EnhancedInnerLoadable = withChunkExtractor(InnerLoadable)
    const Loadable = React.forwardRef((props, ref) => (
      <enhancedinnerloadable forwardedref="{ref}" {...props}>
    ))

    Loadable.displayName = 'Loadable';
    return Loadable;
</enhancedinnerloadable>
```

其中 `withChunkExtractor`是一个高阶组件， `InnerLoadable`经过HOC `withChunkExtractor`包装后再由 `React.forwardRef`包装一次。
因此， `Loadable`实质上也是一个高阶组件，至于具体是如何加载bundle的请继续往下阅读。

在生命周期 `componentDidMount`执行 `loadAsync`：

```
    componentDidMount() {
       this.mounted = true

       // .....

       // component might be resolved synchronously in the constructor
       if (this.state.loading) {
            this.loadAsync()
        }
      }

```

在 `loadAsync`中去加载bundle(实际上源码内部调用了多个方法，为了阅读方便，我把方法拆开简化后都写到loadAsync中)，加载成功后bundle存到 `result`，同时 `loading`变为false。

```
  loadAsync() {
      const { __chunkExtractor, forwardedRef, ...props } = this.props;

        /**
        *  &#x8FD9;&#x4E00;&#x6B65;&#x4F1A;&#x8BF7;&#x6C42;&#x9700;&#x8981;&#x61D2;&#x52A0;&#x8F7D;&#x7684;&#x5305;, &#x5BF9;&#x5E94;&#x4EE3;&#x7801;import('./OtherComponent')
        */
       let promise = ctor.requireAsync(props);

        promise
          .then(loadedModule => {
            const result = resolve(loadedModule, this.props);
            if(this.mounted) {
               this.setState({
                 result,
                 loading: false,
               })
            }
          })
          .catch(error => this.safeSetState({ error, loading: false }))

        return promise
   }
```

在 `resolve`中判断使用者是否传入 `resolveComponent`，如果有就用 `resolveComponent`去处理module和props，如果没有则使用 `defaultResolveComponent`返回module。

```
   function resolve(module, props) {
      const Component = options.resolveComponent
        ? options.resolveComponent(module, props)
        : defaultResolveComponent(module)

      return Component
   }
```

`defaultResolveComponent`也是一个函数，传入一个module作为参数，如果是esModule则返回module.default ，否则返回 module.default || module：

```
export function defaultResolveComponent(loadedModule) {
  return loadedModule.__esModule
    ? loadedModule.default
    : loadedModule.default || loadedModule
}
```

```
    render() {
        const {
          forwardedRef,
          fallback: propFallback,
          __chunkExtractor,
          ...props
        } = this.props
        const { loading, result } = this.state

        const fallback = propFallback || options.fallback || null

        if (loading) {
          return fallback
        }

        return render({
          fallback,
          result,
          options,
          props: { ...props, ref: forwardedRef },
        })
      }
```

代码中执行的render如下：

`loadable`是一个高阶组件，在 `componentDidMount`加载 bundle _(() => import('./xxx')被打包后的bundle)_，如果没有加载或者加载未完成则在 `render`中渲染 `fallback`，加载完成则渲染组件。
