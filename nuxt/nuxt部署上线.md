# Nuxt部署上线

- nuxt打包方式有两种：build和generate
- generate:使用generate打包和之前使用vue打包一样，生成一个dist文件夹，然后各种发布操作和vue一样的
- build:build打包就比较复杂了，他会生成一个.nuxt文件夹，然后你如果要发布的话就会比较复杂
- 注意：`nuxt.config.js`文件需要对不同打包方式进行配置

> target: 'server', //build打包用server，generate用static,默认 server