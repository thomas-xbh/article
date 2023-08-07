---
link: https://juejin.cn/post/6844904095774408711
title: Webpack4不求人(5)——编写自定义插件
description: Webpack通过Loader完成模块的转换工作，让“一切皆模块”成为可能。Plugin机制则让其更加灵活，可以在Webpack生命周期中调用钩子完成各种任务，包括修改输出资源、输出目录等等。 今天我们一起来学习如何编写Webpack插件[1]。 在编写插件之前，还需要了解一下…
keywords: Webpack
author: 首页 首页 沸点 课程 直播 活动 竞赛 商城 App 插件 搜索历史 清空 创作者中心 写文章 发沸点 写笔记 写代码 草稿箱 创作灵感 查看更多 会员 登录 注册
date: 2020-03-18T03:44:53.000Z
publisher: 稀土掘金
stats: paragraph=101 sentences=157, words=782
---
![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/3/18/170ebbe0f2fdbc73~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.image) Webpack通过Loader完成模块的转换工作，让"一切皆模块"成为可能。Plugin机制则让其更加灵活，可以在Webpack生命周期中调用钩子完成各种任务，包括修改输出资源、输出目录等等。

**今天我们一起来学习如何编写Webpack插件 [1]**。

## 构建流程

在编写插件之前，还需要了解一下Webpack的构建流程，以便在合适的时机插入合适的插件逻辑。Webpack的基本构建流程如下：

1. 校验配置文件
2. 生成Compiler对象
3. 初始化默认插件
4. `run/watch`：如果运行在watch模式则执行watch方法，否则执行run方法
5. `compilation`：创建Compilation对象回调compilation相关钩子
6. `emit`：文件内容准备完成，准备生成文件，这是最后一次修改最终文件的机会
7. `afterEmit`：文件已经写入磁盘完成
8. `done`：完成编译

## 插件示例

一个典型的Webpack插件代码如下：

```js

class MyWebpackPlugin {
  constructor(options) {
  }

  apply(compiler) {

    compiler.hooks.emit.tap('MyWebpackPlugin', (compilation) => {});
  }
}

module.exports = MyWebpackPlugin;
复制代码
```

接下来需要在webpack.config.js中引入这个插件。

```java
module.exports = {
  plugins:[

    new MyWebpackPlugin({
      param:'paramValue'
    }),
  ]
};
复制代码
```

Webpack在启动时会实例化插件对象，在初始化compiler对象之后会调用插件实例的apply方法，传入compiler对象，插件实例在apply方法中会注册感兴趣的钩子，Webpack在执行过程中会根据构建阶段回调相应的钩子。

## Compiler && Compilation对象

在编写Webpack插件过程中，最常用也是最主要的两个对象就是Webpack提供的Compiler和Compilation，Plugin通过访问Compiler和Compilation对象来完成工作。

* Compiler 对象包含了当前运行Webpack的配置，包括entry、output、loaders等配置，这个对象在启动Webpack时被实例化，而且是全局唯一的。Plugin可以通过该对象获取到Webpack的配置信息进行处理。
* Compilation对象可以理解编译对象，包含了模块、依赖、文件等信息。在开发模式下运行Webpack时，每修改一次文件都会产生一个新的Compilation对象，Plugin可以访问到本次编译过程中的模块、依赖、文件内容等信息。

### 常见钩子

Webpack会根据执行流程来回调对应的钩子，下面我们来看看都有哪些常见钩子，这些钩子支持的tap操作是什么。

钩子 说明 参数 类型 afterPlugins 启动一次新的编译 compiler 同步 compile 创建compilation对象之前 compilationParams 同步 compilation compilation对象创建完成 compilation 同步 emit 资源生成完成，输出之前 compilation 异步 afterEmit 资源输出到目录完成 compilation 异步 done 完成编译 stats 同步

## Tapable

Tapable是Webpack的一个核心工具，Webpack中许多对象扩展自Tapable类。Tapable类暴露了tap、tapAsync和tapPromise方法，可以根据钩子的同步/异步方式来选择一个函数注入逻辑。

* tap 同步钩子
* tapAsync 异步钩子，通过callback回调告诉Webpack异步执行完毕
* tapPromise 异步钩子，返回一个Promise告诉Webpack异步执行完毕

### tap

tap是一个同步钩子，同步钩子在使用时不可以包含异步调用，因为函数返回时异步逻辑有可能未执行完毕导致问题。

下面一个在compile阶段插入同步钩子的示例。

```js
compiler.hooks.compile.tap('MyWebpackPlugin', params => {
  console.log('我是同步钩子')
});
复制代码
```

### tapAsync

tapAsync是一个异步钩子，我们可以通过callback告知Webpack异步逻辑执行完毕。

下面是一个在emit阶段的示例，在1秒后打印文件列表。

```js
compiler.hooks.emit.tapAsync('MyWebpackPlugin', (compilation, callback) => {
  setTimeout(()=>{
    console.log('文件列表', Object.keys(compilation.assets).join(','));
    callback();
  }, 1000);
});
复制代码
```

### tapPromise

tapPromise也是也是异步钩子，和tapAsync的区别在于tapPromise是通过返回Promise来告知Webpack异步逻辑执行完毕。

下面是一个将生成结果上传到CDN的示例。

```js
compiler.hooks.afterEmit.tapPromise('MyWebpackPlugin', (compilation) => {
  return new Promise((resolve, reject) => {
    const filelist = Object.keys(compilation.assets);
    uploadToCDN(filelist, (err) => {
      if(err) {
        reject(err);
        return;
      }
      resolve();
    });
  });
});
复制代码
```

apply方法中插入钩子的一般形式如下：

```js
compileer.hooks.阶段.tap函数('插件名称', (阶段回调参数) => {

});
复制代码
```

## 常用操作

### 读取输出资源、模块及依赖

在emit阶段，我们可以读取最终需要输出的资源、chunk、模块和对应的依赖，如果有需要还可以更改输出资源。

```js
apply(compiler) {
  compiler.hooks.emit.tapAsync('MyWebpackPlugin', (compilation, callback) => {

    compilation.chunks.forEach(chunk => {

     for(const module of chunk.modulesIterable) {

      	module.dependencies.forEach(dependency => {
          console.log(dependency);
        });
      }
    });
    callback();
  });
}
复制代码
```

### 修改输出资源

通过操作compilation.assets对象，我们可以添加、删除、更改最终输出的资源。

```js
apply(compiler) {
  compiler.hooks.emit.tapAsync('MyWebpackPlugin', (compilation) => {

    compilation.assets['main.js']  = {
      source() {
        return 'modified content';
      },
      size() {
        return this.source().length;
      }
    };

    delete compilation.assets['main.js'];
  });
}
复制代码
```

assets对象需要定义 `source`和 `size`方法， `source&#x65B9;&#x6CD5;&#x8FD4;&#x56DE;&#x8D44;&#x6E90;&#x7684;&#x5185;&#x5BB9;`，支持字符串和Node.js的Buffer， `size&#x8FD4;&#x56DE;&#x6587;&#x4EF6;&#x7684;&#x5927;&#x5C0F;&#x5B57;&#x8282;&#x6570;`。

## 插件示例

接下来我们开始编写自定义插件，所有插件使用的示例项目如下(需要安装 `webpack`和 `webpack-cli`)：

```css
|----src
		|----main.js
|----plugins
		|----my-webpack-plugin.js
|----package.json
|----webpack.config.js
复制代码
```

相关文件的内容如下:

```arduino

console.log('Hello World');
复制代码
```

```json

{
  "scripts":{
    "build":"webpack"
  }
}
复制代码
```

```ini
const path = require('path')
const MyWebpackPlugin = require('my-webpack-plugin')

// webpack.config.js
module.exports = {
  entry:'./src/main',
  output:{
    path: path.resolve(__dirname, 'build'),
    filename:'[name].js',
  },
  plugins:[
    new MyWebpackPlugin()
  ]
}
复制代码
```

### 生成清单文件

通过在 `emit`阶段操作 `compilation.assets`实现。

```js
class MyWebpackPlugin {
    apply(compiler) {
        compiler.hooks.emit.tapAsync('MyWebpackPlugin', (compilation, callback) => {
            const manifest = {};
            for (const name of Object.keys(compilation.assets)) {
                manifest[name] = compilation.assets[name].size();

            }
            compilation.assets['manifest.json'] = {
                source() {
                    return JSON.stringify(manifest);
                },
                size() {
                    return this.source().length;
                }
            };
            callback();
        });
    }
}

module.exports = MyWebpackPlugin;
复制代码
```

构建完成后会在build目录添加manifest.json，内容如下：

```css
{"main.js":956}
复制代码
```

### 构建结果上传到七牛

在实际开发中，资源文件构建完成后一般会同步到CDN，最终前端界面使用的是CDN服务器上的静态资源。

下面我们编写一个Webpack插件，文件构建完成后上传的七牛CDN。

我们的插件依赖qiniu，因此需要额外安装qiniu模块

```css
npm install qiniu --save-dev
复制代码
```

七牛的Node.js SDK文档地址如下：

```bash
https://developer.qiniu.com/kodo/sdk/1289/nodejs
复制代码
```

开始编写插件代码：

```js
const qiniu = require('qiniu');
const path = require('path');

class MyWebpackPlugin {

    mac = null;

    constructor(options) {

        this.options = options || {};

        this.checkQiniuConfig();

        this.mac = new qiniu.auth.digest.Mac(
            this.options.qiniu.accessKey,
            this.options.qiniu.secretKey
        );
    }
    checkQiniuConfig() {

        if (!this.options.qiniu) {
            this.options.qiniu = {
                accessKey: process.env.QINIU_ACCESS_KEY,
                secretKey: process.env.QINIU_SECRET_KEY,
                bucket: process.env.QINIU_BUCKET,
                keyPrefix: process.env.QINIU_KEY_PREFIX || ''
            };
        }
        const qiniu = this.options.qiniu;
        if (!qiniu.accessKey || !qiniu.secretKey || !qiniu.bucket) {
            throw new Error('invalid qiniu config');
        }
    }

    apply(compiler) {
        compiler.hooks.afterEmit.tapPromise('MyWebpackPlugin', (compilation) => {
            return new Promise((resolve, reject) => {

                const uploadCount = Object.keys(compilation.assets).length;

                let currentUploadedCount = 0;

                const putPolicy = new qiniu.rs.PutPolicy({ scope: this.options.qiniu.bucket });
                const uploadToken = putPolicy.uploadToken(this.mac);
                const config = new qiniu.conf.Config();
                config.zone = qiniu.zone.Zone_z1;
                const formUploader = new qiniu.form_up.FormUploader()
                const putExtra = new qiniu.form_up.PutExtra();

                let globalError = null;

                for (const filename of Object.keys(compilation.assets)) {

                    formUploader.putFile(
                        uploadToken,
                        this.options.qiniu.keyPrefix + filename,
                        path.resolve(compilation.outputOptions.path, filename),
                        putExtra,
                        (err) => {
                            console.log(`uploade ${filename} result: ${err ? `Error:${err.message}` : 'Success'}`)
                            currentUploadedCount++;
                            if (err) {
                                globalError = err;
                            }
                            if (currentUploadedCount === uploadCount) {
                                globalError ? reject(globalError) : resolve();
                            }
                        });
                }
            })
        });
    }
}

module.exports = MyWebpackPlugin;
复制代码
```

Webpack中需要传递给该插件传递相关配置：

```css
module.exports = {
    entry: './src/index',
    target: 'node',
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: '[name].js',
      	publicPath: 'CDN域名'
    },
    plugins: [
        new CleanWebpackPlugin(),
        new QiniuWebpackPlugin({
            qiniu: {
                accessKey: '七牛AccessKey',
                secretKey: '七牛SecretKey',
                bucket: 'static',
                keyPrefix: 'webpack-inaction/demo1/'
            }
        })
    ]
};
复制代码
```

编译完成后资源会自动上传到七牛CDN，这样前端只用交付index.html即可。

## 小结

至此，Webpack相关常用知识和进阶知识都介绍完毕，需要各位读者在工作中去多加探索，Webpack配合Node.js生态，一定会涌现出更多优秀的新语言和新工具！
![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/3/11/170c836a7287fb13~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.image)

### 参考资料

[1]

Webpack Plugin: _https://webpack.js.org/api/plugins/_
