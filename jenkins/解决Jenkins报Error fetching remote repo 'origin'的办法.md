---
link: https://blog.csdn.net/qq_27361727/article/details/91360683
title: 解决Jenkins报Error fetching remote repo 'origin'的办法
description: Jenkins build时有时候报Error fetching remote repo ‘origin’，网上都说是git权限问题，试了之后也没有用，找了很久才发现，造成这个问题的原因是Jenkins有个工作空间（ workspace）的概念，Jenkins构建时产生的缓存会存储到工作空间，清理掉缓存就好，如下图：如果觉得每次手动清理比较麻烦，我们可以配置Jenkins在每次构建完成之后就进..._error fetching remote repo
keywords: error fetching remote repo
author: 一枝夏雨荷丶 Csdn认证博客专家 Csdn认证企业博客 码龄8年 暂无认证
date: 2022-04-25T07:01:52.000Z
publisher: null
stats: paragraph=3 sentences=94, words=276
---
[

_Fetch_ _ing_ upstream changes from git@github. com:a792883583/treeHoleScore.git > /usr/bin/git --version # timeout=10 > git --version # 'git version 1.8.3.1' us _ing_ GIT _ASKPASS to set credentials github > /usr/bin/git _fetch_ --tags --progress git@github. com:a792883583/treeHoleScore.git +refs/heads/*:refs/ _remote_s/ _origin_/* # timeout=10 _ERROR_: _Error_ _fetch_ _ing_ _remote_ re _po_ ' _origin_' hudson. plugins.git.GitException: Failed to _fetch_ from git@github. com:a792883583/treeHoleScore.git at hudson. plugins . git .GitSCM. _fetch_From(GitSCM. java:1003) at hudson. plugins .git .GitSCM. retrieveChanges(GitSCM. java:1245) at hudson.plugins.git.GitsCM. checkout(GitSCM. java:1309) at hudson.scm. SCM. checkout(SCM. java:540) at hudson. mode1. AbstractProject . checkout(AbstractProject . java:1240) at hudson. model AbstractBuild$AbstractBuildExecution. def aultCheckout (AbstractBuild. java:649) at _jenkins_ .scm. SCMCheckoutStrategy . checkout(SCMCheckoutStrategy . java:85) at hudson . model. AbstractBuild$AbstractBuildExecution. run(AbstractBuild. java:521) at hudson.model . Run. execute(Run. java:1900) at hudson.model. FreeSty1eBuild.run(FreeStyleBuild.java:44) at hudson. model. ResourceController . execute(ResourceController . java:101) at hudson. model. Executor .run(Executor. java:442) Caused by: hudson. plugins.git .GitException: Command "/usr/bin/git _fetch_ --tags --progress git@github . com: a792883583/treeHoleScore.git +refs/heads/* :refs/ _remote_s/ _origin_/*" returned status code 128: stdout: stderr: Host key verification failed. fatal: Could not read from _remote_ re _po_sitory. Please make sure you have the correct access rights and the re _po_sitory exists. at org. _jenkins_ci .plugins.gitclient .CliGitAPIImp1.1aunchCommandIn(CliGitAPImp1.java:2734) at org. _jenkins_ci .plugins.gitclient .CliGitAPIImp1.1aunchCommandWithCredentials(CliGitAPIImpl.java:2111) at org. _jenkins_ci.plugins . gitclient .CliGitAPIImp1$1. execute(CliGitAPIImp1.java:623) at hudson.p1ugins . git .GitSCM. _fetch_From (GitSCM. java:1001) 11 more _ERROR_: _Error_ _fetch_ _ing_ _remote_ re _po_ ' _origin_' Finished: FAILURE

最新发布](https://wenku.csdn.net/answer/63e2ff33b43dc14a7634ab1b)
