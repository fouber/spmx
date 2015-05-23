开发seajs项目的超级利器！
======

闲着无聊，写了一个支持seajs模块化项目的构建工具（前端集成解决方案），具有以下功能：

* 以 ``seajs`` 作为模块化框架
* 所有静态资源自动加 ``md5版本戳``，seajs均可加载定位
* 支持给所有静态资源添加域名前缀，seajs加载毫无压力
* 非常易用的自动 ``csssprites``
* 自动jshint校验js、coffee文件，校验结果为 ``中文`` 显示
* js、css压缩，压缩时保留require关键字，使得seajs运行正常
* png图片压缩，支持 ``将png24压缩为png8``
* 内置本地开发调试服务器，支持运行 ``jsp``、``php``
* 支持使用 ``less``、``coffeescript`` 开发项目
* 支持将underscore模板编译成 ``js模板函数`` 直接嵌入到js或coffee文件中使用
* 支持define自动包装，写seajs组件如同写nodejs一样舒爽
* 支持文件监听，保存即发布
* 支持浏览器自动刷新，保存即刷新
* 可以上传到远端服务器，保存即增量编译上传
* 超低学习成本，只须记忆 ``3`` 条命令即可完成开发
* 抹平编码差异，开发中无论是gbk、gb2312、utf8、utf8-bom等编码的文件，输出时都能统一指定为utf8无bom（默认）或者gbk文件
* 跨平台支持win、mac、linux等系统

详细用法
=========

## 安装

```bash
npm install -g spmx
```

非win系统最好将npm的包安装在用户目录下，尽量避免使用 ``sudo`` 来安装。修改npm install -g安装目录的方法为：

```bash
# 设置global路径为用户目录
npm config set prefix ~/npm
# 将~/npm/bin路径加到PATH变量中
echo -e '\nexport PATH=~/npm/bin:$PATH' >> ~/.bashrc
# 重新载入.bashrc
source ~/.bashrc
# 安装spmx
npm install -g spmx
```

安装成功后执行 ``spmx -h`` 即可看到相关开发命令帮助

## 获得一个todo样例项目

```bash
# 使用spmx的install命令来获取开发资源
spmx install seajs-todo-demo
```

在当前目录下即可看到令人期待的seajs模块化项目啦！

> 如果你执行 ``spmx install seajs-todo-demo`` 命令没成功，可以git clone 这个项目： [seajs-todo-demo](https://github.com/fouber/seajs-todo-demo)

## 让代码跑起来！

首先，启动内置的调试服务器：

```bash
spmx server start
```

此时spmx会启动一个精巧的jetty服务器，并且打开浏览器访问了 http://127.0.0.1:8080 ，现在这个调试环境什么也没有，接下来，我们在命令行下cd到我们下载的样例项目中：

```bash
cd todo-demo
```

第三步，执行spmx的编译命令：

```bash
spmx release
```

第四步，刷新浏览器，查看我们的项目。

## 各种功能体验

1. 给所有资源加 ``md5版本戳``，执行：

    ```bash
    spmx release -m
    ```
    
    然后刷新浏览器，看一下源码吧！

1. 压缩js、css、图片，执行：

    ```bash
    spmx release -o
    ```

1. 校验js，执行：

    ```bash
    spmx release -l
    ```

1. 自动csssprite，执行：

    ```bash
    spmx release -p
    ```
    
    可以看到 ``#todo-list label`` 的图片都自动合并了哦

1. 所有静态资源加域名，先修改fis-conf.js文件，去除掉 ``第2行`` 的注释，然后执行：

    ```bash
    spmx release -D
    ```

1. 文件监听，执行：

    ```bash
    spmx release -w
    ```
    
    命令行窗口不要关闭，然后去修改源码->保存->刷新浏览器，就能看到更新的效果。

1. 文件上传，执行：

    ```bash
    spmx release -d remote
    ```
    
    就可以发布到我的一个小服务器上，然后浏览器访问： http://vm-1.chongzi.kd.io/

1. 加域名、压缩、加md5戳、校验、csssprite、把编译好的文件产出到output目录中（其实就是随意组合使用前面提到面的参数啦）：

    ```bash
    spmx release -Domlp -d ../output
    ```

1. 文件监听、启动live-reload服务器、同时发布到本地调试目录、outpu目录和远端服务器：

    ```bash
    spmx release -wLd preview,remote,../output
    ```

    使用自动刷新功能需要添加 ``live-reload`` 浏览器插件，请google之，我没做可以自动添加脚本的功能，因为懒。。。

## 目录规范

任何 ``目录规范``、``部署规范``、``编译规范`` 都是可配置的，不过略微麻烦一些，有兴趣的同学可以在issues里留言，这里只介绍内置的规范。

内置的规范包括：

1. ``.less`` 后缀的文件是less文件，编译后得到css文件。
1. ``.minix.less`` 后缀的文件定义less的minix，编译后不会产出。
1. ``.coffee`` 后缀的文件是coffee-script，编译后得到js文件。
1. ``.tmpl`` 后缀的文件是underscore前端模板，左右定界符为 ``<%`` 和 ``%>``，编译后不会产出。在js或coffee中使用 [``__inline('path')``](https://github.com/fouber/seajs-todo-demo/blob/c80f78cd56c2ad31ff344892f7a0dd5648f049d0/sea-modules/views/todos.js#L12) 函数将其嵌入到js或coffee文件中作为模板函数使用。
1. 扔在 ``sea-modules`` 目录下的js、css、less、coffee文件都是模块化文件，会自动包装define，自己就不要写了。使用seajs.use或者require加载模块的时候id与文件的对应规则是这样的：
<table>
    <tr>
        <td>文件</td>
        <td>引用id</td>
        <td>举个例子</td>
    </tr>
    <tr>
        <td>/sea-modules/a.js</td>
        <td>a</td>
        <td>seajs.use('a');</td>
    </tr>
    <tr>
        <td>/sea-modules/b/b.js</td>
        <td>b</td>
        <td>seajs.use('b');</td>
    </tr>
    <tr>
        <td>/sea-modules/b/c.js</td>
        <td>b/c</td>
        <td>seajs.use('b/c');</td>
    </tr>
</table>
1. 扔在 ``lib`` 目录下的文件不被认为是模块化的，请直接在页面上使用script或link标签引用。

> 如果使用中遇到什么觉得诡异的地方，欢迎issues留言
