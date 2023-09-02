### 前言

在本地开发时，默认使用`http://localhost`。Service Workers、Web Authentication API等都可以正常工作。然而，在以下情况下，你需要HTTPS来进行本地开发。

- 在不同的浏览器中以一致的方式设置安全cookies
- 调试mixed-content的问题
- 使用HTTP/2及更高版本
- 使用需要HTTPS的第三方库或API
- 使用自定义的主机名

### 安装mkcert

> npm install -g mkcert

1. 生成ca证书,生成之后会看到一个ca.crt和ca.key文件

   > mkcert create-ca

2. 生成cert证书,生成了cert.crt和cert.key文件

   > mkcert create-cert

### 使用

1. 将ca.crt安装到电脑受信任的根证书中

2. 双击ca.crt文件在弹出的界面中安装

   ![image.png](https://s2.loli.net/2023/08/20/V8WfuC7xUp4LFyN.png)

下一步这样操作

![image.png](https://s2.loli.net/2023/08/20/YfkdC8EvTroxiUm.png)

接下来,一直选择下一步即可

### 在vue中使用

* 将刚刚生成的`cert.crt`和`cert.key`两个拷贝到项目的`src/ssl`文件夹中，没有可以新建一个`ssl`目录

在vue.config.js中使用

```js
//vue2
devServer: {
        open: false,
        https: {
            cert: fs.readFileSync(path.join(__dirname, 'src/ssl/server.crt')),
            key: fs.readFileSync(path.join(__dirname, 'src/ssl/server.key'))
        },
}
```

