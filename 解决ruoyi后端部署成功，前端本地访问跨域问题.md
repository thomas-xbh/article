### 情况

ruoyi后端部署成功

前端本地运行配置跨域访问不了

问题：ruoyi后端访问url组成为ip:port/prod-api

本人以为是ip:port导致错误

devServer配置为：

```
devServer: {
    host: '0.0.0.0',
    port: port,
    open: true,
    proxy: {
      // detail: https://cli.vuejs.org/config/#devserver-proxy
      [process.env.VUE_APP_BASE_API]: {
        target: `http://119.91.138.137:80/prod-api`,
        changeOrigin: true,
        pathRewrite: {
          ['^'+process.env.VUE_APP_BASE_API]: ''
        }
      }
    },
    disableHostCheck: true
  }
```

### 错误认知

后端部署上去端口为8080,这里代理到80端口能访问是因为,在部署的服务器上开启了nginx代理,将80端口代理到8080端口
