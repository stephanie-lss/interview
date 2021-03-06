## 本节介绍如何从零开始构建一本书籍

### 组件介绍

1. GitBook 包含 gitbook 命令行工具，文本编辑器，还有它的git仓库用来存md文件，以及通过git仓库得到的html文件。

2. Github集成，可以将Github的仓库作为GitBook的源git仓库，原因就是在Github上可以方便地与其他人交流。还有一个原因就是GitBook的git仓库是很封闭的，每次你要对仓库进行修改都需要输入你的用户名和token。token在[这里](https://www.gitbook.com/@kangqf/settings/tokens)查看。

3. GitBook Editor 是上面提到的文本编辑器，登录后可以直接同步自己的书籍，而且还可以导入本地的gitbook仓库，但是美中不足的是只支持https不支持ssh来提交修改。不过编辑器可以记住你的用户名和密码，然后就不用每次都输用户名和密码了。

### GitBook workflow

首先我们的需求包括以下几个方面

1. 方便地管理我们地源文件，可以对其进行版本控制和协作编辑 -> 用Github托管md源文件
2. 能够在本地查看渲染效果 -> 用`gitbook serve`来查看
3. 因为GitBook的编辑器很方便，我们要用上 -> 从Github导入进编辑器
4. 能够在GitBook的网站上看见我们的书籍 -> Github 与 GitBook 集成

这样我们就得到了我们的大致的workflow

1. 我们用GitBook Editor 来编辑和管理我们的Github仓库，包括分支管理
2. 修改后我们在本地进行预览，没有问题就同步到我们的Github
3. GitBook通过hook更新其内容

### 新建书籍

根据上面的workflow我们就能大致知道新建一本书籍的大概流程

1. 安装 gitbook -> `npm install gitbook-cli -g`，安装编辑器
2. 在Github新建一个仓库，然后clone到本地，注意这里需要使用https，如上所述，gitbook editor 不支持ssh只支持https
3. `gitbook init`初始化一本书籍，然后提交本地的修改，并push到github
4. 用编辑器导入，导入后源目录就失效了，新的目录在`C:\Users\kqf\GitBook\Library\Import`下面并且每次的提交也是提交在该目录下面，然后通过editor修改(每一次的Ctrl+s 都是一次commit，一次同步就是一次push和pull)，保存章节结构相关内容，提交之后用vim添加文章内容
5. `gitbook server`预览书籍，没有问题就同步到Github
6. 在GitBook通过Github新建一本书籍，然后它就会自动Github同步

### 绑定域名

通过上面的步骤你就可以通过 类似 [https://kangqf.gitbooks.io/interviewnotes/](https://kangqf.gitbooks.io/interviewnotes/)的链接来阅读你的书籍，然后通过类似 [https://www.gitbook.com/book/kangqf/interviewnotes/details](https://www.gitbook.com/book/kangqf/interviewnotes/details)的链接来查阅一些细节，或是下载pdf。

但是我们可能会要使用自己的域名 类似 [int.kangqingfei.cn](http://int.kangqingfei.cn)，我们可以很方便的通过CNAME来满足要求。

1. 在你的域名解析服务商处新加一条 int 的CNAME类型子域，指向 www.gitbooks.io 
2. 在你的书籍管理处设置域名，链接[https://www.gitbook.com/book/kangqf/interviewnotes/settings/domains](https://www.gitbook.com/book/kangqf/interviewnotes/settings/domains)
3. 然后设置域名为int.kangqingfei.cn save成功就ok啦 
