在入口文件通常需要引入全局样式,图标,组件库,使用插件;在使用插件方面这里提供了一个很好的思路,在插件中暴露出一个函数,参数为应用实例,在入口文件中调用该函数;这种方式让入口文件更加简洁.

例:以使用store为例,在store/index文件中有一个函数,参数为应用实例对象,通过应用实例对象将store实例挂载,所以在入口文件调用该函数传参即可.

```js
//main.ts
import { setupStore } from '@/store';
import App from './App.vue';
async function bootstrap() {
  const app = createApp(App);
  // Configure store
  // 配置 store
  setupStore(app);
  app.mount('#app');
}
bootstrap();


//store/index.ts
import type { App } from 'vue';
import { createPinia } from 'pinia';
const store = createPinia();
export function setupStore(app: App<Element>) {
  app.use(store);
}

```

接下来这个例子可以更清晰的体现出这种书写方式的好处

通常注册全局组件需要在入口文件中一个一个注册,如果组件很多显得代码很繁重

如:

```js
//main.ts
app.use(Input).use(Button).use(Layout).use(VXETable);
```

此时我们可以将代码提取出来

```js
//main.ts
import App from './App.vue';
import { registerGlobComp } from '@/components/registerGlobComp';
async function bootstrap() {
  const app = createApp(App);
      // 注册全局组件
  registerGlobComp(app);
  app.mount('#app');
}
bootstrap();


//components/registerGlobComp
import { Button } from './Button';
import { Input, Layout } from 'ant-design-vue';
import VXETable from 'vxe-table';
export function registerGlobComp(app: App) {
  app.use(Input).use(Button).use(Layout).use(VXETable);
}
```

