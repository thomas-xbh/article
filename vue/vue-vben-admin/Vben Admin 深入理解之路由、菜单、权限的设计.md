---
link: https://juejin.cn/post/7001851383607459848
title: Vben Admin 深入理解之路由、菜单、权限的设计
description: 本部分主要分析路由、菜单、权限之间的关系，以下代码主要是流程的内容某些相关的数据处理函数和分支可自行阅读。 路由可以影响到菜单的生成，菜单根据权限模式做不同的处理、权限影响到路由的注册和登录的校验。
keywords: Vue.js,前端
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2021-08-29T14:02:04.000Z
publisher: 稀土掘金
stats: paragraph=83 sentences=86, words=587
---
个人认为 [Vben Admin](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fanncwb%2Fvue-vben-admin "https://github.com/anncwb/vue-vben-admin") 是一个很好的项目，完成度和文档都比较完善，技术栈也是一直保持最新的。当我在跟着文档操作一遍后，却对很多东西不明所以而没有立即投入项目中使用。

当然也必要等全部理解完了再去使用，用着看着呗毕竟优秀点很多。指南部分文档对怎么使用写的很清楚而源码中高度的封装，而这部分也常常是需要根据实际业务修改的地方，所以决定把先把这部分的源码看一下再使用。

本部分主要分析路由、菜单、权限之间的关系，以下代码主要是流程的内容某些相关的数据处理函数和分支可自行阅读。

路由可以影响到菜单的生成，菜单根据权限模式做不同的处理、权限影响到路由的注册和登录的校验。

* 路由是怎么自动加载并生成菜单的？
* 菜单权限模式分别有什么不同，怎么做的区分和处理？
* 权限的认证流程和初始化是怎么完成的？

因为涉及到整体的结构设计，所以先看一下项目初始化了哪些内容。

```ts

async function bootstrap() {
  const app = createApp(App);

  setupStore(app);

  initAppConfigStore();

  registerGlobComp(app);

  await setupI18n(app);

  setupRouter(app);

  setupRouterGuard(router);

  setupGlobDirectives(app);

  setupErrorHandle(app);

  await router.isReady();

  app.mount("#app", true);
}
复制代码
```

实现自动加载 `modules` 下的路由文件并生成路由配置信息和一些通用的配置。

```ts

import { PAGE_NOT_FOUND_ROUTE, REDIRECT_ROUTE } from "/@/router/routes/basic";
import { mainOutRoutes } from "./mainOut";

const modules = import.meta.globEager("./modules/**/*.ts");
const routeModuleList: AppRouteModule[] = [];

export const asyncRoutes = [PAGE_NOT_FOUND_ROUTE, ...routeModuleList];

export const RootRoute: AppRouteRecordRaw = {};
export const LoginRoute: AppRouteRecordRaw = {};

export const basicRoutes = [

  LoginRoute,

  RootRoute,

  ...mainOutRoutes,

  REDIRECT_ROUTE,

  PAGE_NOT_FOUND_ROUTE
];
复制代码
```

点击登录获取用户信息，存储使用的 `pinia` 实现。

```ts

const userInfo = await userStore.login(
  toRaw({
    password: data.password,
    username: data.account,
    mode: "none"
  })
);
复制代码
```

```ts

login(params){

  const data = await loginApi(loginParams, mode);
  const { token } = data;

  this.setToken(token);

  const userInfo = await this.getUserInfoAction();

  const routes = await permissionStore.buildRoutesAction();
  routes.forEach((route) => {
    router.addRoute(route);
  });
  return userInfo;
}
复制代码
```

获取 `token` 之后调用 `getUserInfoAction` 获取用户信息。

```ts

getUserInfoAction() {
  const userInfo = await getUserInfo();
  const { roles } = userInfo;
  const roleList = roles.map((item) => item.value);

  this.setUserInfo(userInfo);

  this.setRoleList(roleList);
  return userInfo;
}
复制代码
```

登录成功之后调用 `buildRoutesAction` 获取路由配置、生成菜单配置。

```ts

buildRoutesAction() {

  const permissionMode = appStore.getProjectConfig.permissionMode;

  switch (permissionMode) {

    case PermissionModeEnum.ROLE:

      routes = filter(asyncRoutes, routeFilter);
      routes = routes.filter(routeFilter);

      routes = flatMultiLevelRoutes(routes);
      break;

    case PermissionModeEnum.ROUTE_MAPPING:

      routes = filter(asyncRoutes, routeFilter);
      routes = routes.filter(routeFilter);

      const menuList = transformRouteToMenu(routes, true);

      this.setFrontMenuList(menuList);

      routes = flatMultiLevelRoutes(routes);
      break;

    case PermissionModeEnum.BACK:

      let routeList: AppRouteRecordRaw[] = [];
      routeList = await getMenuList();

      const backMenuList = transformRouteToMenu(routeList);

      this.setBackMenuList(backMenuList);

      routeList = flatMultiLevelRoutes(routeList);
      routes = [PAGE_NOT_FOUND_ROUTE, ...routeList];
      break;
  }
}
复制代码
```

根据不同的权限模式从不同的数据源获取菜单。

```ts

const modules = import.meta.globEager("./modules/**/*.ts");
const staticMenus = transformMenuModule(modules);

async function getAsyncMenus() {
  const permissionStore = usePermissionStore();

  if (isBackMode()) {

    return permissionStore.getBackMenuList.filter(item => !item.meta?.hideMenu && !item.hideMenu);
  }

  if (isRouteMappingMode()) {

    return permissionStore.getFrontMenuList.filter(item => !item.hideMenu);
  }

  return staticMenus;
}
复制代码
```

在菜单组件中获取菜单配置渲染。

```tsx

function renderMenu() {
  const { menus, ...menuProps } = unref(getCommonProps);
  if (!menus || !menus.length) return null;
  return !props.isHorizontal ? (
    <SimpleMenu {...menuProps} isSplitMenu={unref(getSplit)} items={menus} />
  ) : (
    <BasicMenu
      {...(menuProps as any)}
      isHorizontal={props.isHorizontal}
      type={unref(getMenuType)}
      showLogo={unref(getIsShowLogo)}
      mode={unref(getComputedMenuMode as any)}
      items={menus}
    />
  );
}
复制代码
```

判断是否登录以及刷新之后的初始化。

```ts

export function createPermissionGuard(route) {
  const userStore = useUserStoreWithOut();
  const permissionStore = usePermissionStoreWithOut();
  router.beforeEach(async (to, from, next) => {

    if (whitePathList.includes(to.path)) {
      next();
      return;
    }

    const token = userStore.getToken;
    if (!token) {
      if (to.meta.ignoreAuth) {
        next();
        return;
      }

      const redirectData: { path: string; replace: boolean; query?: Recordable<string> } = {
        path: LOGIN_PATH,
        replace: true
      };
      if (to.path) {
        redirectData.query = {
          ...redirectData.query,
          redirect: to.path
        };
      }
      next(redirectData);
      return;
    }

    if (userStore.getLastUpdateTime === 0) {
      try {
        await userStore.getUserInfoAction();
      } catch (err) {
        next();
        return;
      }
    }

    if (permissionStore.getIsDynamicAddedRoute) {
      next();
      return;
    }
    const routes = await permissionStore.buildRoutesAction();
    routes.forEach(route => {
      router.addRoute(route);
    });
    router.addRoute(PAGE_NOT_FOUND_ROUTE);
    permissionStore.setDynamicAddedRoute(true);
  });
}
复制代码
```

我们知道了权限认证后通过权限过滤配置好的路由表并动态添加路由，菜单的生成根据权限模式的不同获取不同的数据源渲染菜单栏。这个流程在实际项目必会更改，所以我们应该知道更新现有的流程。
