---
link: null
title: npm install时出现gyp verb `which` failed Error: not found: python2相关错误 - xkfx - 博客园
description: 问题描述 从网上下载一个vue项目，npm install时出现如下错误： npm ERR! code 1 npm ERR! path D:\2022_2_11_clear\sadjkl\node_modules\node-sass npm ERR! command failed npm ERR!
keywords: null
author: 我的博客 我的园子 账号设置 简洁模式 ... 退出登录
date: 2022-03-02T10:57:00.000Z
publisher: null
stats: paragraph=40 sentences=1661, words=6752
---
## 问题描述

从网上下载一个vue项目，npm install时出现如下错误：
![](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
npm ERR! code 1
npm ERR! path D:\2022_2_11_clear\sadjkl\node_modules\node-sass
npm ERR! command failed
npm ERR! command C:\WINDOWS\system32\cmd.exe /d /s /c node scripts/build.js
npm ERR! Building: D:\Program Files\nodejs\node.exe D:\2022_2_11_clear\sadjkl\node_modules\node-gyp\bin\node-gyp.js rebuild --verbose --libsass_ext= --libsass_cflags= --libsass_ldflags= --libsass_library=
npm ERR! gyp info it worked if it ends with ok
npm ERR! gyp verb cli [
npm ERR! gyp verb cli   'D:\\Program Files\\nodejs\\node.exe',
npm ERR! gyp verb cli   'D:\\2022_2_11_clear\\sadjkl\\node_modules\\node-gyp\\bin\\node-gyp.js',
npm ERR! gyp verb cli   'rebuild',
npm ERR! gyp verb cli   '--verbose',
npm ERR! gyp verb cli   '--libsass_ext=',
npm ERR! gyp verb cli   '--libsass_cflags=',
npm ERR! gyp verb cli   '--libsass_ldflags=',
npm ERR! gyp verb cli   '--libsass_library='
npm ERR! gyp verb cli ]
npm ERR! gyp info using node-gyp@3.8.0
npm ERR! gyp info using node@16.13.0 | win32 | x64
npm ERR! gyp verb command rebuild []
npm ERR! gyp verb command clean []
npm ERR! gyp verb clean removing "build" directory
npm ERR! gyp verb command configure []
npm ERR! gyp verb check python checking for Python executable "python2" in the PATH
npm ERR! gyp verb `which` failed Error: not found: python2
npm ERR! gyp verb `which` failed     at getNotFoundError (D:\2022_2_11_clear\sadjkl\node_modules\which\which.js:13:12)
npm ERR! gyp verb `which` failed     at F (D:\2022_2_11_clear\sadjkl\node_modules\which\which.js:68:19)
npm ERR! gyp verb `which` failed     at E (D:\2022_2_11_clear\sadjkl\node_modules\which\which.js:80:29)
npm ERR! gyp verb `which` failed     at D:\2022_2_11_clear\sadjkl\node_modules\which\which.js:89:16
npm ERR! gyp verb `which` failed     at D:\2022_2_11_clear\sadjkl\node_modules\isexe\index.js:42:5
npm ERR! gyp verb `which` failed     at D:\2022_2_11_clear\sadjkl\node_modules\isexe\windows.js:36:5
npm ERR! gyp verb `which` failed     at FSReqCallback.oncomplete (node:fs:198:21)
npm ERR! gyp verb `which` failed  python2 Error: not found: python2
npm ERR! gyp verb `which` failed     at getNotFoundError (D:\2022_2_11_clear\sadjkl\node_modules\which\which.js:13:12)
npm ERR! gyp verb `which` failed     at F (D:\2022_2_11_clear\sadjkl\node_modules\which\which.js:68:19)
npm ERR! gyp verb `which` failed     at E (D:\2022_2_11_clear\sadjkl\node_modules\which\which.js:80:29)
npm ERR! gyp verb `which` failed     at D:\2022_2_11_clear\sadjkl\node_modules\which\which.js:89:16
npm ERR! gyp verb `which` failed     at D:\2022_2_11_clear\sadjkl\node_modules\isexe\index.js:42:5
npm ERR! gyp verb `which` failed     at D:\2022_2_11_clear\sadjkl\node_modules\isexe\windows.js:36:5
npm ERR! gyp verb `which` failed     at FSReqCallback.oncomplete (node:fs:198:21) {
npm ERR! gyp verb `which` failed   code: 'ENOENT'
npm ERR! gyp verb `which` failed }
npm ERR! gyp verb check python checking for Python executable "python" in the PATH
npm ERR! gyp verb `which` succeeded python D:\anaconda3\python.EXE
npm ERR! gyp ERR! configure error
npm ERR! gyp ERR! stack Error: Command failed: D:\anaconda3\python.EXE -c import sys; print "%s.%s.%s" % sys.version_info[:3];
npm ERR! gyp ERR! stack   File "", line 1
npm ERR! gyp ERR! stack     import sys; print "%s.%s.%s" % sys.version_info[:3];
npm ERR! gyp ERR! stack                       ^
npm ERR! gyp ERR! stack SyntaxError: invalid syntax
npm ERR! gyp ERR! stack
npm ERR! gyp ERR! stack     at ChildProcess.exithandler (node:child_process:397:12)
npm ERR! gyp ERR! stack     at ChildProcess.emit (node:events:390:28)
npm ERR! gyp ERR! stack     at maybeClose (node:internal/child_process:1064:16)
npm ERR! gyp ERR! stack     at Process.ChildProcess._handle.onexit (node:internal/child_process:301:5)
npm ERR! gyp ERR! System Windows_NT 10.0.14393
npm ERR! gyp ERR! command "D:\\Program Files\\nodejs\\node.exe" "D:\\2022_2_11_clear\\sadjkl\\node_modules\\node-gyp\\bin\\node-gyp.js" "rebuild" "--verbose" "--libsass_ext=" "--libsass_cflags=" "--libsass_ldflags=" "--libsass_library="
npm ERR! gyp ERR! cwd D:\2022_2_11_clear\sadjkl\node_modules\node-sass
npm ERR! gyp ERR! node -v v16.13.0
npm ERR! gyp ERR! node-gyp -v v3.8.0
npm ERR! gyp ERR! not ok
npm ERR! Build failed with error code: 1

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\mdzz\AppData\Local\npm-cache\_logs\2022-03-01T13_51_54_809Z-debug.log
```

文字版错误日志

![](https://img2022.cnblogs.com/blog/1042431/202203/1042431-20220301215318537-939697142.png)

[经过查找资料](https://stackoverflow.com/questions/45801457/node-js-python-not-found-exception-due-to-node-sass-and-node-gyp)，推测是NodeJS和node-sass版本不匹配的问题：

```
| NodeJS  | Supported node-sass version | Node Module |
|---------|-----------------------------|-------------|
| Node 16 | 6.0+                        | 93          |
| Node 15 | 5.0+                        | 88          |
| Node 14 | 4.14+                       | 83          |
| Node 13 | 4.13+, <5.0                 | 79          |
| Node 12 | 4.12+                       | 72          |
| Node 11 | 4.10+, <5.0                 | 67          |
| Node 10 | 4.9+, <6.0                  | 64          |
| Node 8  | 4.5.3+, <5.0                | 57          |
| Node <8 | <5.0                        | <57         |
```

执行node --version指令，本机node版本为v16.13.0

查看项目的package.json，devDependencies中node-sass版本是4.12.0

根据上面的表格，确实存在不匹配。

## 一些尝试，最后通过办法4解决

### 尝试办法①——修改node-sass的版本

根据上表，

```
npm i node-sass@6.0.1
```

执行该指令之后，devDependencies中的node-sass版本也被修改为相应版本，继续执行：

```
npm install
```

这次没有再报错，继续执行：

```
npm run serve
```

结果报错： **Syntax Error: Error: Node Sass version 6.0.1 is incompatible with ^4.0.0.**

详情如下：

```
D:\2022_2_11_clear\sadjkl>npm run serve

> unclear@0.1.0 serve
> vue-cli-service serve

 INFO  Starting development server...

98% after emitting CopyPlugin

Failed to compile with 1 error                                                       下午10:56:39
 error  in ./src/App.vue?vue&type=style&index=0&lang=scss&

Syntax Error: Error: Node Sass version 6.0.1 is incompatible with ^4.0.0.

 @ ./node_modules/vue-style-loader??ref--9-oneOf-1-0!./node_modules/css-loader/dist/cjs.js??ref--9-oneOf-1-1!./node_modules/vue-loader/lib/loaders/stylePostLoader.js!./node_modules/postcss-loader/src??ref--9-oneOf-1-2!./node_modules/sass-loader/dist/cjs.js??ref--9-oneOf-1-3!./node_modules/cache-loader/dist/cjs.js??ref--1-0!./node_modules/vue-loader/lib??vue-loader-options!./src/App.vue?vue&type=style&index=0&lang=scss& 4:14-416 15:3-20:5 16:22-424
 @ ./src/App.vue?vue&type=style&index=0&lang=scss&
 @ ./src/App.vue
 @ ./src/main.js
 @ multi (webpack)-dev-server/client?http://172.27.2.205:8080&sockPath=/sockjs-node (webpack)/hot/dev-server.js ./src/main.js
```

![](https://img2022.cnblogs.com/blog/1042431/202203/1042431-20220301234721155-450414140.png)

### 办法②——npm install --global windows-build-tools

但是根据stackoverflow网友描述：

```
npm install --global windows-build-tools installs Python 2.7, and installs it globally
```

也就是该办法本质上仍然是通过配置python2环境解决问题。执行指令后会全局安装python2.7，对于这种全局安装没有详细描述，这可能会产生一些后果，影响我本来搭建好的python3环境。因此考虑采用其它办法配置python2环境。

### 尝试③——用conda配置python2环境

本地原始环境状况：已安装anaconda，默认python版本为3.9（详见[win10按默认步骤安装Anaconda后各指令状况&Anaconda配置环境变量](https://www.cnblogs.com/xkxf/p/15926219.html)）

**如何用conda配置、启用python2环境：** [https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html#managing-environments](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html#managing-environments)

**执行：**

```
conda create --name py2env python=2.7
```

报错：
![](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
Collecting package metadata (current_repodata.json): failed

CondaHTTPError: HTTP 000 CONNECTION FAILED for url //repo.anaconda.com/pkgs/main/win-64/current_repodata.json>
Elapsed: -

An HTTP error occurred when trying to retrieve this URL.

HTTP errors are often intermittent, and a simple retry will get you on your way.

If your current network has https://www.anaconda.com blocked, please file
a support request with your network engineering team.

'https://repo.anaconda.com/pkgs/main/win-64'
```

log(http error)

切换手机热点后有改善，但还是不行：

![](https://img2022.cnblogs.com/blog/1042431/202203/1042431-20220302172833282-1127071331.png)

于是只能找镜像了，先用了清华镜像，卡死在开始，后面采用下面这个镜像（截至2022-03-02证明有效），才成功。

采用 **编辑文件配置镜像**的办法，编辑用户文件夹下的 **.condarc**文件，把下面的内容替换进去，保存：

```
channels:
  - defaults
show_channel_urls: true
default_channels:
  - http://mirrors.aliyun.com/anaconda/pkgs/main
  - http://mirrors.aliyun.com/anaconda/pkgs/r
  - http://mirrors.aliyun.com/anaconda/pkgs/msys2
custom_channels:
  conda-forge: http://mirrors.aliyun.com/anaconda/cloud
  msys2: http://mirrors.aliyun.com/anaconda/cloud
  bioconda: http://mirrors.aliyun.com/anaconda/cloud
  menpo: http://mirrors.aliyun.com/anaconda/cloud
  pytorch: http://mirrors.aliyun.com/anaconda/cloud
  simpleitk: http://mirrors.aliyun.com/anaconda/cloud
```

执行：

```
conda clean -i
```

重新执行最开始的"创建环境"指令。

测试新创建的环境：

```
C:\Users\mdzz>conda activate py2env

(py2env) C:\Users\mdzz>python --version
Python 2.7.18 :: Anaconda, Inc.

(py2env) C:\Users\mdzz>where python
D:\anaconda3\envs\py2env\python.exe
D:\anaconda3\python.exe

(py2env) C:\Users\mdzz>pip --version
pip 19.3.1 from D:\anaconda3\envs\py2env\lib\site-packages\pip (python 2.7)

(py2env) C:\Users\mdzz>pip list
DEPRECATION: Python 2.7 will reach the end of its life on January 1st, 2020. Please upgrade your Python as Python 2.7 won't be maintained after that date. A future version of pip will drop support for Python 2.7. More details about Python 2 support in pip, can be found at https://pip.pypa.io/en/latest/development/release-process/#python-2-support
Package      Version
------------ -------------------
certifi      2020.6.20
pip          19.3.1
setuptools   44.0.0.post20200106
wheel        0.37.1
wincertstore 0.2

(py2env) C:\Users\mdzz>conda deactivate
```

用该环境去执行npm install，仍然报错，还是node-sass，但和之前的不一样：
![](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
npm ERR! code 1
npm ERR! path D:\2022_2_11_clear\unclear-power\node_modules\node-sass
npm ERR! command failed
npm ERR! command C:\WINDOWS\system32\cmd.exe /d /s /c node scripts/build.js
npm ERR! Building: D:\Program Files\nodejs\node.exe D:\2022_2_11_clear\unclear-power\node_modules\node-gyp\bin\node-gyp.js rebuild --verbose --libsass_ext= --libsass_cflags= --libsass_ldflags= --libsass_library=
npm ERR! �ڴ˽��������һ������һ����Ŀ����Ҫ���ò������ɣ������ӡ�/m�����ء�
npm ERR! ��������ʱ��Ϊ 2022/3/2 19:11:03��
npm ERR! �ڵ� 1 �ϵ���Ŀ��D:\2022_2_11_clear\unclear-power\node_modules\node-sass\build\binding.sln��(Ĭ��Ŀ��)��
npm ERR! ValidateSolutionConfiguration:
npm ERR!   �������ɽ���������á�Release|x64����
npm ERR! MSBUILD : error MSB3428: δ�ܼ��� Visual C++ �����VCBuild.exe����Ҫ��������⣬1) ��װ .NET Framework 2.0 SDK��2) ��װ Microsoft Visual Studio 2005���� 3) �������� ����װ��������λ�ã��뽫��λ�����ӵ�ϵͳ·���С� [D:\2022_2_11_clear\unclear-power\node_modules\node-sass\build\binding.sln]
npm ERR! �����������Ŀ��D:\2022_2_11_clear\unclear-power\node_modules\node-sass\build\binding.sln��(Ĭ��Ŀ��)�Ĳ��� - ʧ�ܡ�
npm ERR!

npm ERR! ����ʧ�ܡ�
npm ERR!

npm ERR! ��D:\2022_2_11_clear\unclear-power\node_modules\node-sass\build\binding.sln��(Ĭ��Ŀ��) (1) ->
npm ERR! (_src_\libsass Ŀ��) ->
npm ERR!   MSBUILD : error MSB3428: δ�ܼ��� Visual C++ �����VCBuild.exe����Ҫ��������⣬1) ��װ .NET Framework 2.0 SDK��2) ��װ Microsoft Visual Studio 2005���� 3) ������� �����װ��������λ�ã��뽫��λ�����ӵ�ϵͳ·���С� [D:\2022_2_11_clear\unclear-power\node_modules\node-sass\build\binding.sln]
npm ERR!

npm ERR!     0 ������
npm ERR!     1 ������
npm ERR!

npm ERR! ����ʱ�� 00:00:00.92
npm ERR! gyp info it worked if it ends with ok
npm ERR! gyp verb cli [
npm ERR! gyp verb cli   'D:\\Program Files\\nodejs\\node.exe',
npm ERR! gyp verb cli   'D:\\2022_2_11_clear\\unclear-power\\node_modules\\node-gyp\\bin\\node-gyp.js',
npm ERR! gyp verb cli   'rebuild',
npm ERR! gyp verb cli   '--verbose',
npm ERR! gyp verb cli   '--libsass_ext=',
npm ERR! gyp verb cli   '--libsass_cflags=',
npm ERR! gyp verb cli   '--libsass_ldflags=',
npm ERR! gyp verb cli   '--libsass_library='
npm ERR! gyp verb cli ]
npm ERR! gyp info using node-gyp@3.8.0
npm ERR! gyp info using node@16.13.0 | win32 | x64
npm ERR! gyp verb command rebuild []
npm ERR! gyp verb command clean []
npm ERR! gyp verb clean removing "build" directory
npm ERR! gyp verb command configure []
npm ERR! gyp verb check python checking for Python executable "python2" in the PATH
npm ERR! gyp verb `which` failed Error: not found: python2
npm ERR! gyp verb `which` failed     at getNotFoundError (D:\2022_2_11_clear\unclear-power\node_modules\which\which.js:13:12)
npm ERR! gyp verb `which` failed     at F (D:\2022_2_11_clear\unclear-power\node_modules\which\which.js:68:19)
npm ERR! gyp verb `which` failed     at E (D:\2022_2_11_clear\unclear-power\node_modules\which\which.js:80:29)
npm ERR! gyp verb `which` failed     at D:\2022_2_11_clear\unclear-power\node_modules\which\which.js:89:16
npm ERR! gyp verb `which` failed     at D:\2022_2_11_clear\unclear-power\node_modules\isexe\index.js:42:5
npm ERR! gyp verb `which` failed     at D:\2022_2_11_clear\unclear-power\node_modules\isexe\windows.js:36:5
npm ERR! gyp verb `which` failed     at FSReqCallback.oncomplete (node:fs:198:21)
npm ERR! gyp verb `which` failed  python2 Error: not found: python2
npm ERR! gyp verb `which` failed     at getNotFoundError (D:\2022_2_11_clear\unclear-power\node_modules\which\which.js:13:12)
npm ERR! gyp verb `which` failed     at F (D:\2022_2_11_clear\unclear-power\node_modules\which\which.js:68:19)
npm ERR! gyp verb `which` failed     at E (D:\2022_2_11_clear\unclear-power\node_modules\which\which.js:80:29)
npm ERR! gyp verb `which` failed     at D:\2022_2_11_clear\unclear-power\node_modules\which\which.js:89:16
npm ERR! gyp verb `which` failed     at D:\2022_2_11_clear\unclear-power\node_modules\isexe\index.js:42:5
npm ERR! gyp verb `which` failed     at D:\2022_2_11_clear\unclear-power\node_modules\isexe\windows.js:36:5
npm ERR! gyp verb `which` failed     at FSReqCallback.oncomplete (node:fs:198:21) {
npm ERR! gyp verb `which` failed   code: 'ENOENT'
npm ERR! gyp verb `which` failed }
npm ERR! gyp verb check python checking for Python executable "python" in the PATH
npm ERR! gyp verb `which` succeeded python D:\anaconda3\envs\py2env\python.EXE
npm ERR! gyp verb check python version `D:\anaconda3\envs\py2env\python.EXE -c "import sys; print "2.7.18
npm ERR! gyp verb check python version .%s.%s" % sys.version_info[:3];"` returned: %j
npm ERR! gyp verb get node dir no --target version specified, falling back to host node version: 16.13.0npm ERR! gyp verb command install [ '16.13.0' ]
npm ERR! gyp verb install input version string "16.13.0"
npm ERR! gyp verb install installing version: 16.13.0
npm ERR! gyp verb install --ensure was passed, so won't reinstall if already installed
npm ERR! gyp verb install version not already installed, continuing with install 16.13.0
npm ERR! gyp verb ensuring nodedir is created C:\Users\mdzz\.node-gyp\16.13.0
npm ERR! gyp verb created nodedir C:\Users\mdzz\.node-gyp
npm ERR! gyp http GET https://nodejs.org/download/release/v16.13.0/node-v16.13.0-headers.tar.gz
npm ERR! gyp http 200 https://nodejs.org/download/release/v16.13.0/node-v16.13.0-headers.tar.gz
npm ERR! gyp verb extracted file from tarball include\node\common.gypi
npm ERR! gyp verb extracted file from tarball include\node\config.gypi
npm ERR! gyp verb extracted file from tarball include\node\node.h
npm ERR! gyp verb extracted file from tarball include\node\node_api.h
npm ERR! gyp verb extracted file from tarball include\node\js_native_api.h
npm ERR! gyp verb extracted file from tarball include\node\js_native_api_types.h
npm ERR! gyp verb extracted file from tarball include\node\node_api_types.h
npm ERR! gyp verb extracted file from tarball include\node\node_buffer.h
npm ERR! gyp verb extracted file from tarball include\node\node_object_wrap.h
npm ERR! gyp verb extracted file from tarball include\node\node_version.h
npm ERR! gyp verb extracted file from tarball include\node\v8-internal.h
npm ERR! gyp verb extracted file from tarball include\node\v8-platform.h
npm ERR! gyp verb extracted file from tarball include\node\v8-profiler.h
npm ERR! gyp verb extracted file from tarball include\node\v8.h
npm ERR! gyp verb extracted file from tarball include\node\v8config.h
npm ERR! gyp verb extracted file from tarball include\node\v8-version.h
npm ERR! gyp verb extracted file from tarball include\node\libplatform\libplatform-export.h
npm ERR! gyp verb extracted file from tarball include\node\libplatform\libplatform.h
npm ERR! gyp verb extracted file from tarball include\node\libplatform\v8-tracing.h
npm ERR! gyp verb extracted file from tarball include\node\cppgc\common.h
npm ERR! gyp verb extracted file from tarball include\node\uv.h
npm ERR! gyp verb extracted file from tarball include\node\uv\aix.h
npm ERR! gyp verb extracted file from tarball include\node\uv\android-ifaddrs.h
npm ERR! gyp verb extracted file from tarball include\node\uv\bsd.h
npm ERR! gyp verb extracted file from tarball include\node\uv\darwin.h
npm ERR! gyp verb extracted file from tarball include\node\uv\linux.h
npm ERR! gyp verb extracted file from tarball include\node\uv\os390.h
npm ERR! gyp verb extracted file from tarball include\node\uv\posix.h
npm ERR! gyp verb extracted file from tarball include\node\uv\stdint-msvc2008.h
npm ERR! gyp verb extracted file from tarball include\node\uv\sunos.h
npm ERR! gyp verb extracted file from tarball include\node\uv\threadpool.h
npm ERR! gyp verb extracted file from tarball include\node\uv\unix.h
npm ERR! gyp verb extracted file from tarball include\node\uv\win.h
npm ERR! gyp verb extracted file from tarball include\node\uv\errno.h
npm ERR! gyp verb extracted file from tarball include\node\uv\tree.h
npm ERR! gyp verb extracted file from tarball include\node\uv\version.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\pem.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\pem2.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\pemerr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\pkcs12.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\pkcs12err.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\pkcs7.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\pkcs7err.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\rand.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\rand_drbg.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\randerr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\rc2.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\rc4.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\rc5.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\ripemd.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\rsa.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\rsaerr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\safestack.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\seed.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\sha.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\srp.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\srtp.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\ssl.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\ssl2.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\ssl3.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\sslerr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\stack.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\store.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\storeerr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\symhacks.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\tls1.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\ts.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\tserr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\txt_db.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\ui.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\uierr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\whrlpool.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\x509.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\x509_vfy.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\x509err.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\x509v3.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\x509v3err.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\asn1t.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\async.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\bioerr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\blowfish.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\bnerr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\cmac.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\cms.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\cmserr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\comp.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\comperr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\cryptoerr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\ct.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\cterr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\des.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\dh.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\dsa.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\dsaerr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\dtls1.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\ebcdic.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\ocsp.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\aes.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\asn1.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\asn1_mac.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\asn1err.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\ecdh.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\ecdsa.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\ecerr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\err.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\evp.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\evperr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\hmac.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\idea.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\kdferr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\lhash.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\md4.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\objectserr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\ocsperr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\conferr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\crypto.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\e_os2.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\engine.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\engineerr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\kdf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\md2.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\md5.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\modes.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\asyncerr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\bio.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\bn.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\buffer.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\conf_api.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\dherr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\ec.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\obj_mac.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\ossl_typ.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\buffererr.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\camellia.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\cast.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\mdc2.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\objects.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\opensslv.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86_64\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86_64\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86_64\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86_64\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86_64\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86_64\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86_64\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86_64\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86_64\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86_64\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86_64\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86_64\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86_64\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86_64\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\BSD-x86_64\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN32\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN32\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN32\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN32\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN32\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN32\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN32\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN32\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN32\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN32\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN32\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN32\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN32\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN32\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN32\no-asm\include\progs.hnpm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64-ARM\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64-ARM\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64-ARM\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64-ARM\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64-ARM\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64A\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64A\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64A\asm\crypto\buildinf.hnpm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64A\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64A\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64A\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64A\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64A\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64A\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64A\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64A\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64A\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64A\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64A\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\VC-WIN64A\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix-gcc\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix-gcc\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix-gcc\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix-gcc\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix-gcc\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix-gcc\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix-gcc\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix-gcc\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix-gcc\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix-gcc\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix-gcc\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix-gcc\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix-gcc\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix-gcc\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix-gcc\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix64-gcc\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix64-gcc\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix64-gcc\asm\crypto\buildinf.hnpm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix64-gcc\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix64-gcc\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix64-gcc\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix64-gcc\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix64-gcc\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix64-gcc\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix64-gcc\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix64-gcc\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix64-gcc\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix64-gcc\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix64-gcc\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\aix64-gcc\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin-i386-cc\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin-i386-cc\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin-i386-cc\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin-i386-cc\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin-i386-cc\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin-i386-cc\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin-i386-cc\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin-i386-cc\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin-i386-cc\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin-i386-cc\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin-i386-cc\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin-i386-cc\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin-i386-cc\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin-i386-cc\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin-i386-cc\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-arm64-cc\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-arm64-cc\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-arm64-cc\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-arm64-cc\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-arm64-cc\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-arm64-cc\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-arm64-cc\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-arm64-cc\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-arm64-cc\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-arm64-cc\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-arm64-cc\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-arm64-cc\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-arm64-cc\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-arm64-cc\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-arm64-cc\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-x86_64-cc\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-x86_64-cc\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-x86_64-cc\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-x86_64-cc\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-x86_64-cc\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-x86_64-cc\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-x86_64-cc\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-x86_64-cc\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-x86_64-cc\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-x86_64-cc\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-x86_64-cc\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-x86_64-cc\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-x86_64-cc\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-x86_64-cc\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\darwin64-x86_64-cc\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-aarch64\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-aarch64\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-aarch64\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-aarch64\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-aarch64\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-aarch64\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-aarch64\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-aarch64\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-aarch64\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-aarch64\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-aarch64\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-aarch64\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-aarch64\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-aarch64\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-aarch64\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-armv4\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-armv4\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-armv4\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-armv4\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-armv4\asm\include\progs.hnpm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-armv4\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-armv4\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-armv4\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-armv4\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-armv4\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-armv4\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-armv4\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-armv4\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-armv4\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-armv4\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-elf\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-elf\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-elf\asm\crypto\buildinf.hnpm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-elf\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-elf\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-elf\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-elf\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-elf\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-elf\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-elf\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-elf\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-elf\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-elf\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-elf\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-elf\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc\asm\crypto\buildinf.hnpm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64\asm\include\progs.hnpm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64le\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64le\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64le\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64le\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64le\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64le\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64le\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64le\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64le\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64le\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64le\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64le\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64le\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64le\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-ppc64le\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x32\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x32\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x32\asm\crypto\buildinf.hnpm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x32\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x32\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x32\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x32\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x32\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x32\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x32\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x32\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x32\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x32\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x32\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x32\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x86_64\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x86_64\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x86_64\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x86_64\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x86_64\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x86_64\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x86_64\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x86_64\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x86_64\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x86_64\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x86_64\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x86_64\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x86_64\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x86_64\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux-x86_64\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux32-s390x\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux32-s390x\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux32-s390x\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux32-s390x\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux32-s390x\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux32-s390x\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux32-s390x\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux32-s390x\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux32-s390x\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux32-s390x\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux32-s390x\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux32-s390x\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux32-s390x\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux32-s390x\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux32-s390x\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-mips64\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-mips64\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-mips64\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-mips64\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-mips64\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-mips64\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-mips64\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-mips64\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-mips64\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-mips64\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-mips64\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-mips64\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-mips64\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-mips64\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-mips64\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-s390x\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-s390x\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-s390x\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-s390x\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-s390x\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-s390x\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-s390x\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-s390x\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-s390x\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-s390x\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-s390x\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-s390x\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-s390x\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-s390x\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-s390x\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris-x86-gcc\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris-x86-gcc\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris-x86-gcc\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris-x86-gcc\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris-x86-gcc\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris-x86-gcc\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris-x86-gcc\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris-x86-gcc\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris-x86-gcc\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris-x86-gcc\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris-x86-gcc\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris-x86-gcc\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris-x86-gcc\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris-x86-gcc\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris-x86-gcc\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris64-x86_64-gcc\asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris64-x86_64-gcc\asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris64-x86_64-gcc\asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris64-x86_64-gcc\asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris64-x86_64-gcc\asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris64-x86_64-gcc\asm_avx2\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris64-x86_64-gcc\asm_avx2\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris64-x86_64-gcc\asm_avx2\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris64-x86_64-gcc\asm_avx2\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris64-x86_64-gcc\asm_avx2\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris64-x86_64-gcc\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris64-x86_64-gcc\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris64-x86_64-gcc\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris64-x86_64-gcc\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\solaris64-x86_64-gcc\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-riscv64\no-asm\crypto\include\internal\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-riscv64\no-asm\crypto\include\internal\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-riscv64\no-asm\crypto\buildinf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-riscv64\no-asm\include\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\archs\linux64-riscv64\no-asm\include\progs.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\bn_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\dso_conf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\opensslconf.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\bn_conf_asm.h
npm ERR! gyp verb content checksum node-v16.13.0-headers.tar.gz 9abfc6dcd32bce3b9a978b8c23b8bb48a562c94919feba489f9bb9d4bbeeae66
npm ERR! gyp verb extracted file from tarball include\node\openssl\bn_conf_no-asm.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\dso_conf_asm.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\dso_conf_no-asm.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\opensslconf_asm.h
npm ERR! gyp verb extracted file from tarball include\node\openssl\opensslconf_no-asm.h
npm ERR! gyp verb extracted file from tarball include\node\zconf.h
npm ERR! gyp verb extracted file from tarball include\node\zlib.h
npm ERR! gyp verb tarball done parsing tarball
npm ERR! gyp verb on Windows; need to download `node.lib`...

npm ERR! gyp verb 32-bit node.lib dir C:\Users\mdzz\.node-gyp\16.13.0\ia32
npm ERR! gyp verb 64-bit node.lib dir C:\Users\mdzz\.node-gyp\16.13.0\x64
npm ERR! gyp verb `node.lib` 32-bit url https://nodejs.org/download/release/v16.13.0/win-x86/node.lib
npm ERR! gyp verb `node.lib` 64-bit url https://nodejs.org/download/release/v16.13.0/win-x64/node.lib
npm ERR! gyp verb check download content checksum, need to download `SHASUMS256.txt`...

npm ERR! gyp verb checksum url https://nodejs.org/download/release/v16.13.0/SHASUMS256.txt
npm ERR! gyp http GET https://nodejs.org/download/release/v16.13.0/SHASUMS256.txt
npm ERR! gyp verb streaming 64-bit node.lib to: C:\Users\mdzz\.node-gyp\16.13.0\x64\node.lib
npm ERR! gyp http GET https://nodejs.org/download/release/v16.13.0/win-x64/node.lib
npm ERR! gyp verb streaming 32-bit node.lib to: C:\Users\mdzz\.node-gyp\16.13.0\ia32\node.lib
npm ERR! gyp http GET https://nodejs.org/download/release/v16.13.0/win-x86/node.lib
npm ERR! gyp http 200 https://nodejs.org/download/release/v16.13.0/win-x86/node.lib
npm ERR! gyp http 200 https://nodejs.org/download/release/v16.13.0/SHASUMS256.txt
npm ERR! gyp verb checksum data {"node-v16.13.0-aix-ppc64.tar.gz":"eadf10e991a690c2af98b6df8a118336ded513d6b13a83721bb28bec290df908","node-v16.13.0-darwin-arm64.tar.gz":"46d83fc0bd971db5050ef1b15afc44a6665dee40bd6c1cbaec23e1b40fa49e6d","node-v16.13.0-darwin-arm64.tar.xz":"ca8d79ceecfa8b7d74651fba648c9034f6108070b7cd02437ecb2b7f103842d4","node-v16.13.0-darwin-x64.tar.gz":"37e09a8cf2352f340d1204c6154058d81362fef4ec488b0197b2ce36b3f0367a","node-v16.13.0-darwin-x64.tar.xz":"8dc70eb0965896a4d1755e719be2b5efeff7cb8a54e1f3b8dccc5a2864965504","node-v16.13.0-headers.tar.gz":"9abfc6dcd32bce3b9a978b8c23b8bb48a562c94919feba489f9bb9d4bbeeae66","node-v16.13.0-headers.tar.xz":"9f38e4e1702dde08937125d618c0de119bd77da4665e584aaffc332691af7ef1","node-v16.13.0-linux-arm64.tar.gz":"46e3857f5552abd36d9548380d795b043a3ceec2504e69fe1a754fa76012daaf","node-v16.13.0-linux-arm64.tar.xz":"93a0d03f9f802353cb7052bc97a02cd9642b49fa985671cdc16c99936c86d7d2","node-v16.13.0-linux-armv7l.tar.gz":"3d22bc15b47c26129d56745cd587ead7e240a36968ceb3c4105bebc5c6a0be16","node-v16.13.0-linux-armv7l.tar.xz":"c2c548387ec6b08291b746423bfdf6475948561884985cad16a95983e4913ee4","node-v16.13.0-linux-ppc64le.tar.gz":"3ed1d82ecd6eee3549e9b7f4e65c8a42cbc77f1c6e4da35cf7e496d540fa1760","node-v16.13.0-linux-ppc64le.tar.xz":"46ae5cd3b4c554a600d1b3df5ead1567f6f223a7d4d4667a2be92a67ae70aea7","node-v16.13.0-linux-s390x.tar.gz":"8883af58d2e7c8f8c8311a9504ba1d6ce77c6badc7120165823a516049378042","node-v16.13.0-linux-s390x.tar.xz":"49e972bf3e969d621157df4c8f2fa18ff748c167d5ebd0efc87e1b9f0c6541cc","node-v16.13.0-linux-x64.tar.gz":"589b7e7eb22f8358797a2c14a0bd865459d0b44458b8f05d2721294dacc7f734","node-v16.13.0-linux-x64.tar.xz":"a876ce787133149abd1696afa54b0b5bc5ce3d5ae359081d407ff776e39b7ba8","node-v16.13.0.pkg":"33c3b6eba6b7f82413545b3789b35bf9b9a187f92c984d6c5555934c773ce4fc","node-v16.13.0.tar.gz":"9c00e5b6024cfcbc9105f9c58cf160762e78659a345d100c5bd80a7fb38c684f","node-v16.13.0.tar.xz":"32114b3dc3945ed0f95f8bc33b42c68e0ef18c408cb56122572a163d907ecbcc","node-v16.13.0-win-x64.7z":"f4402bebb34339d9b6f1814df17eed278ec79ec6f0f7501203b2681b284c644d","node-v16.13.0-win-x64.zip":"5a39ec5d4786c2814a6c04488bebac6423c2aaa12832b24f0882456f2e4674e1","node-v16.13.0-win-x86.7z":"d93bceda06924a46c7035d74e99dd58802960f4ce9dc6c3aa8c55118612c4106","node-v16.13.0-win-x86.zip":"dd2e7fccf073ac356878e541dd4e165f05ff145fe9722feb52613f58f88ded7b","node-v16.13.0-x64.msi":"bf55b68293b163423ea4856c1d330be23158e78aea18a8756cfdff6fb6ffcd88","node-v16.13.0-x86.msi":"53443f9147d86d538ee24ef9897ae0ff0be740c18fd0e3c17058dbafe343364f","win-x64/node.exe":"7fca04f83b0e2169e41b2e1845e8da0f07d66cf9c3a1b4150767bf3ffddccf62","win-x64/node.lib":"7c94657df6918a77dc8edefaf3b5415dbfb9eb83a88c17d216a48c8c36fcc58d","win-x64/node_pdb.7z":"1009402493f1eba91c587486033fd46d8006cb7d9c41b84abc19fa3ba18b0d55","win-x64/node_pdb.zip":"72d08ea9e7a7b10d8d1da92a0261e60f71e1f0cf1e14443db8477cad9fcd511e","win-x86/node.exe":"0414fdd69004885755cb6782100970e62814d6f1f66ce1d6ba0d7bf89c2522f6","win-x86/node.lib":"8090f51a19ff2d5e765920262a4367203be2e69e64ac3725e4e14dd034c98443","win-x86/node_pdb.7z":"90759e7c7aa604f25ba3454618834d3a31a30bd8c500a8625d4dd270a8127614","win-x86/node_pdb.zip":"703b18c4ffcd5f082254a4885cd58f3924352dc7c2287749b0087cdfc1b7f2ce"}
npm ERR! gyp http 200 https://nodejs.org/download/release/v16.13.0/win-x64/node.lib
npm ERR! gyp verb content checksum win-x64/node.lib 7c94657df6918a77dc8edefaf3b5415dbfb9eb83a88c17d216a48c8c36fcc58d
npm ERR! gyp verb content checksum win-x86/node.lib 8090f51a19ff2d5e765920262a4367203be2e69e64ac3725e4e14dd034c98443
npm ERR! gyp verb download contents checksum {"node-v16.13.0-headers.tar.gz":"9abfc6dcd32bce3b9a978b8c23b8bb48a562c94919feba489f9bb9d4bbeeae66","win-x64/node.lib":"7c94657df6918a77dc8edefaf3b5415dbfb9eb83a88c17d216a48c8c36fcc58d","win-x86/node.lib":"8090f51a19ff2d5e765920262a4367203be2e69e64ac3725e4e14dd034c98443"}
npm ERR! gyp verb validating download checksum for node-v16.13.0-headers.tar.gz (9abfc6dcd32bce3b9a978b8c23b8bb48a562c94919feba489f9bb9d4bbeeae66 == 9abfc6dcd32bce3b9a978b8c23b8bb48a562c94919feba489f9bb9d4bbeeae66)
npm ERR! gyp verb validating download checksum for win-x64/node.lib (7c94657df6918a77dc8edefaf3b5415dbfb9eb83a88c17d216a48c8c36fcc58d == 7c94657df6918a77dc8edefaf3b5415dbfb9eb83a88c17d216a48c8c36fcc58d)
npm ERR! gyp verb validating download checksum for win-x86/node.lib (8090f51a19ff2d5e765920262a4367203be2e69e64ac3725e4e14dd034c98443 == 8090f51a19ff2d5e765920262a4367203be2e69e64ac3725e4e14dd034c98443)
npm ERR! gyp verb get node dir target node version installed: 16.13.0
npm ERR! gyp verb build dir attempting to create "build" dir: D:\2022_2_11_clear\unclear-power\node_modules\node-sass\build
npm ERR! gyp verb build dir "build" dir needed to be created? D:\2022_2_11_clear\unclear-power\node_modules\node-sass\build
npm ERR! gyp verb find vs2017 Found installation at: C:\Program Files (x86)\Microsoft Visual Studio\2017\Community
npm ERR! gyp verb find vs2017   - Found Microsoft.VisualStudio.Component.Windows10SDK.17134
npm ERR! gyp verb find vs2017   - Found Microsoft.VisualStudio.Component.Windows10SDK.17763
npm ERR! gyp verb find vs2017   - Found Microsoft.VisualStudio.VC.MSBuild.Base
npm ERR! gyp verb find vs2017   - Missing VC++ 2017 v141 toolset (x86,x64) (Microsoft.VisualStudio.Component.VC.Tools.x86.x64)
npm ERR! gyp verb find vs2017   - Some required components are missing, not using this installation
npm ERR! gyp verb Not using VS2017: No usable installation of VS2017 found
npm ERR! gyp verb build/config.gypi creating config file
npm ERR! gyp verb build/config.gypi writing out config file: D:\2022_2_11_clear\unclear-power\node_modules\node-sass\build\config.gypi
npm ERR! (node:12700) [DEP0150] DeprecationWarning: Setting process.config is deprecated. In the future the property will be read-only.

npm ERR! (Use `node --trace-deprecation ...` to show where the warning was created)
npm ERR! gyp verb config.gypi checking for gypi file: D:\2022_2_11_clear\unclear-power\node_modules\node-sass\config.gypi
npm ERR! gyp verb common.gypi checking for gypi file: D:\2022_2_11_clear\unclear-power\node_modules\node-sass\common.gypi
npm ERR! gyp verb gyp gyp format was not specified; forcing "msvs"
npm ERR! gyp info spawn D:\anaconda3\envs\py2env\python.EXE
npm ERR! gyp info spawn args [
npm ERR! gyp info spawn args   'D:\\2022_2_11_clear\\unclear-power\\node_modules\\node-gyp\\gyp\\gyp_main.py',
npm ERR! gyp info spawn args   'binding.gyp',
npm ERR! gyp info spawn args   '-f',
npm ERR! gyp info spawn args   'msvs',
npm ERR! gyp info spawn args   '-G',
npm ERR! gyp info spawn args   'msvs_version=auto',
npm ERR! gyp info spawn args   '-I',
npm ERR! gyp info spawn args   'D:\\2022_2_11_clear\\unclear-power\\node_modules\\node-sass\\build\\config.gypi',
npm ERR! gyp info spawn args   '-I',
npm ERR! gyp info spawn args   'D:\\2022_2_11_clear\\unclear-power\\node_modules\\node-gyp\\addon.gypi',npm ERR! gyp info spawn args   '-I',
npm ERR! gyp info spawn args   'C:\\Users\\mdzz\\.node-gyp\\16.13.0\\include\\node\\common.gypi',
npm ERR! gyp info spawn args   '-Dlibrary=shared_library',
npm ERR! gyp info spawn args   '-Dvisibility=default',
npm ERR! gyp info spawn args   '-Dnode_root_dir=C:\\Users\\mdzz\\.node-gyp\\16.13.0',
npm ERR! gyp info spawn args   '-Dnode_gyp_dir=D:\\2022_2_11_clear\\unclear-power\\node_modules\\node-gyp',
npm ERR! gyp info spawn args   '-Dnode_lib_file=C:\\Users\\mdzz\\.node-gyp\\16.13.0\\',
npm ERR! gyp info spawn args   '-Dmodule_root_dir=D:\\2022_2_11_clear\\unclear-power\\node_modules\\node-sass',
npm ERR! gyp info spawn args   '-Dnode_engine=v8',
npm ERR! gyp info spawn args   '--depth=.',
npm ERR! gyp info spawn args   '--no-parallel',
npm ERR! gyp info spawn args   '--generator-output',
npm ERR! gyp info spawn args   'D:\\2022_2_11_clear\\unclear-power\\node_modules\\node-sass\\build',
npm ERR! gyp info spawn args   '-Goutput_dir=.'
npm ERR! gyp info spawn args ]
npm ERR! Warning: unrecognized setting VCCLCompilerTool/MultiProcessorCompilation
npm ERR! Warning: unrecognized setting VCCLCompilerTool/MultiProcessorCompilation
npm ERR! Warning: unrecognized setting VCCLCompilerTool/MultiProcessorCompilation
npm ERR! Warning: unrecognized setting VCCLCompilerTool/MultiProcessorCompilation
npm ERR! gyp verb command build []
npm ERR! gyp verb build type Release
npm ERR! gyp verb architecture x64
npm ERR! gyp verb node dev dir C:\Users\mdzz\.node-gyp\16.13.0
npm ERR! gyp verb found first Solution file build/binding.sln
npm ERR! gyp verb could not find "msbuild.exe" in PATH - finding location in registry
npm ERR! gyp info spawn C:\Windows\Microsoft.NET\Framework\v4.0.30319\msbuild.exe
npm ERR! gyp info spawn args [
npm ERR! gyp info spawn args   'build/binding.sln',
npm ERR! gyp info spawn args   '/nologo',
npm ERR! gyp info spawn args   '/p:Configuration=Release;Platform=x64'
npm ERR! gyp info spawn args ]
npm ERR! gyp ERR! build error
npm ERR! gyp ERR! stack Error: `C:\Windows\Microsoft.NET\Framework\v4.0.30319\msbuild.exe` failed with exit code: 1
npm ERR! gyp ERR! stack     at ChildProcess.onExit (D:\2022_2_11_clear\unclear-power\node_modules\node-gyp\lib\build.js:262:23)
npm ERR! gyp ERR! stack     at ChildProcess.emit (node:events:390:28)
npm ERR! gyp ERR! stack     at Process.ChildProcess._handle.onexit (node:internal/child_process:290:12)
npm ERR! gyp ERR! System Windows_NT 10.0.14393
npm ERR! gyp ERR! command "D:\\Program Files\\nodejs\\node.exe" "D:\\2022_2_11_clear\\unclear-power\\node_modules\\node-gyp\\bin\\node-gyp.js" "rebuild" "--verbose" "--libsass_ext=" "--libsass_cflags=" "--libsass_ldflags=" "--libsass_library="
npm ERR! gyp ERR! cwd D:\2022_2_11_clear\unclear-power\node_modules\node-sass
npm ERR! gyp ERR! node -v v16.13.0
npm ERR! gyp ERR! node-gyp -v v3.8.0
npm ERR! gyp ERR! not ok
npm ERR! Build failed with error code: 1

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\mdzz\AppData\Local\npm-cache\_logs\2022-03-02T11_11_19_326Z-debug.log
```

debug.log

### 尝试④——用conda配置nodejs14环境

本来以为要把我电脑上的nodejs16卸载重装，自降版本去跑一个项目，所以就不太情愿，没有作为首选方案。突然想到conda似乎是可以配置node环境的，查阅资料证明确实可以。

创建并激活一个新的环境，用来配置nodejs14：

```
conda create --name node14env
conda activate node14env
```

补充个常用命令——查看当前存在的环境：

```
conda env list
```

查看可安装的nodejs包：

```
conda search nodejs
```

安装相应版本：

```
conda install nodejs=14.8.0
```

检查安装情况（npm也会相应更改）：

```
node --version
```

回到项目的根目录执行：

```
npm install
```

接着：

```
npm run serve
```

一切顺利。

该环境python还是3.9，也证明这个问题和本地的python版本没有关系，归根结底，还是nodejs和node-sass版本不匹配造成的。
