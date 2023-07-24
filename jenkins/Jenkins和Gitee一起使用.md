### 所需环境

Docker安装Jenkins

安装Git

### 正式开始

1. 在Jenkins中下载Gitee插件
2. 进入系统设置中配置Gitee
3. 在Jenkins中新建任务
4. 进行配置
5. 填写仓库链接
6. 选择验证方式
7. 在构建触发器中选择webhook触发构建
8. 将url和密码填入gitee项目仓库webhook中
9. 保存即可

### 目前存在的问题

1. 获取仓库只能通过用户名和密码和http链接，无法使用ssh方式

### 注意

1. 在选择构建指令过滤的时候勾选无即可，否则会导致测试不成功
1. ssh公钥要确保是jenkins用户生成的，而不是root用户，jenkins用户是生成在jenkins目录下，root用户生成在root目录下