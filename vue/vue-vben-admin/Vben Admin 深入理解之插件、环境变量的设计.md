---
link: https://juejin.cn/post/7001851258101301255
title: Vben Admin 深入理解之插件、环境变量的设计
description: 新加了一个规则是以 `VITE_GLOB_` 开头的的变量，在打包的时候会。那么怎么处理的环境变量做了什么转换、做了什么扩展？那么 `_app.config.js` 是怎么生成的、内容怎么写入的？
keywords: Vue.js,前端
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2021-08-29T14:00:46.000Z
publisher: 稀土掘金
stats: paragraph=53 sentences=50, words=488
---
个人认为 [Vben Admin](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fanncwb%2Fvue-vben-admin "https://github.com/anncwb/vue-vben-admin") 是一个很好的项目，完成度和文档都比较完善，技术栈也是一直保持最新的。当我在跟着文档操作一遍后，却对很多东西不明所以而没有立即投入项目中使用。

当然也必要等全部理解完了再去使用，用着看着呗毕竟优秀点很多。指南部分文档对怎么使用写的很清楚而源码中高度的封装，而这部分也常常是需要根据实际业务修改的地方，所以决定把先把这部分的源码看一下再使用。

本部分主要分析环境变量的处理和怎么使用一个 Vite 插件。

在 `Vite` 中只有以 `VITE_` 开头的变量会被嵌入到客户端侧的包中。新加了一个规则是以 `VITE_GLOB_` 开头的的变量，在打包的时候会被加入 `_app.config.js`。

* 那么怎么处理的环境变量做了什么转换、做了什么扩展？
* 那么 `_app.config.js` 是怎么生成的、内容怎么写入的？

首先从 `vite.config.ts` 入口开始。

使用 `loadEnv` 读取对应的环境配置文件(.env + .env.[mode] + .env.[mode].local)，可在后续配置中使用。加载方式详情[环境变量和模式](https://link.juejin.cn?target=https%3A%2F%2Fcn.vitejs.dev%2Fguide%2Fenv-and-mode.html "https://cn.vitejs.dev/guide/env-and-mode.html")。

```ts

import { loadEnv } from 'vite';
import { wrapperEnv } from './build/utils';
export default ({ command, mode }: ConfigEnv): UserConfig => {
  const root = process.cwd();

  const env = loadEnv(mode, root);

  const viteEnv = wrapperEnv(env);
  };
};
复制代码
```

使用 `wrapperEnv` 对读取到内容做进一步处理。

```ts

export function wrapperEnv(envConf: Recordable): ViteEnv {
  const ret: any = {};

  for (const envName of Object.keys(envConf)) {
    let realName = envConf[envName].replace(/\\n/g, "\n");

    realName = realName === "true" ? true : realName === "false" ? false : realName;

    if (envName === "VITE_PORT") {
      realName = Number(realName);
    }

    if (envName === "VITE_PROXY") {
      try {
        realName = JSON.parse(realName);
      } catch (error) {}
    }

    ret[envName] = realName;

    if (typeof realName === "string") {
      process.env[envName] = realName;
    } else if (typeof realName === "object") {
      process.env[envName] = JSON.stringify(realName);
    }
  }
  return ret;
}
复制代码
```

然后把解析后 `viteEnv` 传入插件列表中使用。

```ts

export default ({ command, mode }: ConfigEnv): UserConfig => {
  const viteEnv = wrapperEnv(env);
  return {
    plugins: createVitePlugins(viteEnv, isBuild)
  };
};
复制代码
```

```sh
"build": "cross-env NODE_ENV=production vite build && esno ./build/script/postBuild.ts"
复制代码
```

主要作用是解析环境变量文件，并把内容写入到文件中。

```ts

function createConfig({ configName, config, configFileName = GLOB_CONFIG_FILE_NAME }) {
  try {

    const windowConf = `window.${configName}`;
    const configStr = `${windowConf}=${JSON.stringify(config)};
      Object.freeze(${windowConf});
      Object.defineProperty(window, "${configName}", {
        configurable: false,
        writable: false,
      });
    `.replace(/\s/g, "");
    fs.mkdirp(getRootPath(OUTPUT_DIR));

    writeFileSync(getRootPath(`${OUTPUT_DIR}/${configFileName}`), configStr);
  } catch (error) {
    console.log(chalk.red("configuration file configuration file failed to package:\n" + error));
  }
}

export function runBuildConfig() {

  const config = getEnvConfig();

  const configFileName = getConfigFileName(config);

  createConfig({ config, configName: configFileName });
}
复制代码
```

插件都是集中管理在 `build/vite/plugin` 里的，处理完配置后返回数组并配置到 `vite` 的 `plugins` 配置上。

```ts

export function configHtmlPlugin(env: ViteEnv, isBuild: boolean) {
  const { VITE_GLOB_APP_TITLE, VITE_PUBLIC_PATH } = env;

  const path = VITE_PUBLIC_PATH.endsWith("/") ? VITE_PUBLIC_PATH : `${VITE_PUBLIC_PATH}/`;

  const getAppConfigSrc = () => {
    return `${path || "/"}${GLOB_CONFIG_FILE_NAME}?v=${pkg.version}-${new Date().getTime()}`;
  };

  const htmlPlugin: Plugin[] = html({
    minify: isBuild,
    inject: {
      data: {
        title: VITE_GLOB_APP_TITLE
      },

      tags: isBuild
        ? [
            {
              tag: "script",
              attrs: {
                src: getAppConfigSrc()
              }
            }
          ]
        : []
    }
  });
  return htmlPlugin;
}
复制代码
```

首先我们还是可以使用 `vite` 提供的方式。

```ts
import.meta.env.VITE_XXX;
复制代码
```

根据文档可知道通过 `useGlobSetting` 函数来进行获取动态配置。

```ts

export const useGlobSetting = () => {
  const { VITE_GLOB_APP_TITLE } = getAppEnvConfig();
  const glob = {
    title: VITE_GLOB_APP_TITLE
  };
  return glob;
};
复制代码
```

生产环境动态配置的实现。

```ts

export function getAppEnvConfig() {

  const ENV_NAME = getConfigFileName(import.meta.env);

  const ENV = import.meta.env.DEV ? import.meta.env : window[ENV_NAME];

  const { VITE_GLOB_APP_TITLE } = ENV;

  return {
    VITE_GLOB_APP_TITLE
  };
}
复制代码
```

我们知道当我们配置了一个环境变量后 Vben Admin 做了哪些处理又是怎么来的。这样我们在使用的时候如果有问题或增加新的插件逻辑我们也能随心所欲。
