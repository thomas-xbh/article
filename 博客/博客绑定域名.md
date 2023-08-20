### 前言
通过上面两篇教程的介绍,已经将博客基本搭建完成,可以通过输入Github仓库名称来在浏览器访问,接下来将介绍如何将blog与域名绑定起来,直接通过域名访问blog
### 前置条件
> 拥有一个域名 阿里云/腾讯云
### 开始
1. 域名解析
有两种方式
 1):通过将域名指向一个IPV4地址,可以通过控制台 ping `仓库名.github.io`来获取ip地址,将获得的ip地址填入记录值即可
 ![image.png](https://s2.loli.net/2023/08/17/E1fqhC4BvLo7Pjg.png)
 2):通过将域名指向另一个域名,在记录值直接填入`仓库名.github.io`即可
 ![image.png](https://s2.loli.net/2023/08/17/CSdiIGTu594rx1E.png)
将域名解析成两条,因为ip地址可能会变,这样可以确保以后不用修改
![image.png](https://s2.loli.net/2023/08/17/YOEmi3u1szcM9Ll.png)
2. 在blog根目录下的source文件夹中新增一个文件CNAME,不需要后缀名,里面填入
> www.域名
3. 在博客仓库中settings=>pages找到Custom domain,将域名填入即可
![image.png](https://s2.loli.net/2023/08/17/xhtmifabQMz1BZC.png)
稍等几分钟,就可以通过域名来访问啦
### 总结
通过这三篇文章,初步了解如何利用Hexo+Github来搭建博客,使用icarus主题来美化博客;当然还有更多的方式来搭建博客,也有更多漂亮的主题来选择,但是相比于一个好看的界面,博客的内容才是最重要的