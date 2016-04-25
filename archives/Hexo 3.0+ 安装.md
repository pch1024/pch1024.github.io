---
title: 2016-3-19 Hexo 3.0+ 安装
tags: Hexo,Git Blog,Guong
grammar_cjkRuby: true
---
***
# 准备工作哦
***

**注意：** 
本文针对windows 10平台和Hexo 3.x版本

**了解Hexo**
> A fast, simple & powerful blog framework——[Hexo.io](http://hexo.io/)

Hexo 是一个快速、简洁且高效的博客框架。
Hexo 使用markdown(或者其他渲染引擎)解析文章，在几秒内即可利用漂亮的主题生成静态网页。

**安装Git**—[GitHub for windows](https://windows.github.com/)
简单可依赖，默认提示安装即可，So Easy！

**安装Node.JS**—[Node](http://nodejs.org/)
**注意：** 旧版安装需要手动配置Path环境变量，使`npm`  命令生效。新版已自动配置Path

***
## Git初始化
***
(任意界面，鼠标右击 Git Bash here，进入Git命令窗口)

1.  **检测SSH keys ，如果存在跳过第2部分**
    ```
    cd ~/.ssh
    ```

2.  **存在SSH Keys，备份并删除**
    ```
    mkdir key_backup
    cp id_rsa* key_backup
    rm id_rsa*
    ```
    
3.  **创建新的SSH Keys**
    ```
    ssh-keygen -t rsa -C "your_email@youreamil.com"
    Creates a new ssh key using the provided email Generating public/private rsa key pair.
    #此处输入将要保存的路径，默认为当前路径
    Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<press enter>
    输入回车后提示输入一个类似于密码的自定义的通行证号，如果直接回车则为空
    Enter passphrase (empty for no passphrase):<enter a passphrase>
    #提示重新输入以便确认输入是否正确
    Enter same passphrase again:<enter passphrase again>
    ```
    如果看到Your public key has been saved等信息则说明保存成功!
    

4.  **注册并设置GiitHub**
    * 登录GitHub，注册自定义用户名，例如：`guong`
    * 创建 New repository，name必须和用户名一样，例如：`guong.github.io`
    
5.  **将SSH key输入到GitHub网站中**
    * 在：Account Settings->SSH Pbulic Keys>单击Add another public key
    * 将刚才新建的key输入到key中并且添加一个标题，例如：git-tutorial。即/Users/your_user_directory/.ssh/id_rsa。
    * 默认情况下.ssh是隐藏文件，需要将系统设置成显示隐藏文件才能看到。输入完成后单击Add key后，会看到git-tutorial已经被添加进去了。

6.  **测试是否能够正确链接到github中，输入以下命令：**
    ```
    ssh -T git@github.com
    #将会显示信息
    The authenticity of host 'github.com (207.97.227.239)' can't be established. RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48. Are you sure you want to continue connecting (yes/no)?
    #输入yes后，显示出下列信息表示连接成功
    Hi username! You've successfully authenticated, but GitHub does not provide shell access.
    ```

7.  **配置个人信息**
    当上面步骤完成后，就可以设置一些基本的个人信息了
    Git通过检测用户名和邮箱来跟踪进行commit的用户
    ```
    git config --global user.name "Firstname Lastname"
    git config --global user.email "your_email@youremail.com"
    ```
    设置GitHub网站标记
    单击网站中的Account Settings>Account Admin,将APT Token中的那串字符串记录下来，输入到下列命令中：
    ```
    git config --global github.user username
    git config --global github.token 获取到的token
    ```
    至此，Git的本地环境已将搭建完毕！
***
# 创建Hexo代码库
***
1. **安装Hexo**
    ```
    npm install hexo-cli -g
    npm install hexo --save
    
    #如果命令无法运行，可以尝试更换taobao的npm源
    npm install -g cnpm --registry=https://registry.npm.taobao.org
    ```

2. **Hexo初始化配置**
    创建Hexo文件，根据自己需要创建文件路径（如`E:\kuaipan\GitHub\hexo`）进入Git Shell切换到该路径下`E:\kuaipan\GitHub\hexo`执行以下指令
    ```
    hexo init
    
    #安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。
    $ hexo init <folder>
    $ cd <folder>
    $ npm install
    
    #新建完成后，指定文件夹的目录如下
    .
    ├── _config.yml
    ├── package.json
    ├── scaffolds
    ├── scripts
    ├── source
    |      ├── _drafts
    |      └── _posts
    └── themes
    ```
    
3.  **安装Hexo插件**
    ```
    npm install hexo-generator-index --save
    npm install hexo-generator-archive --save
    npm install hexo-generator-category --save
    npm install hexo-generator-tag --save
    npm install hexo-server --save
    npm install hexo-deployer-git --save
    npm install hexo-deployer-heroku --save
    npm install hexo-deployer-rsync --save
    npm install hexo-deployer-openshift --save
    npm install hexo-renderer-marked@0.2 --save
    npm install hexo-renderer-stylus@0.2 --save
    npm install hexo-generator-feed@1 --save
    npm install hexo-generator-sitemap@1 --save
    ```
    
4.  **本地查看效果**
    继续执行以下命令，成功后可登录localhost:4000查看效果
    ```
    hexo server
    ```
    **补充：Hexo简写命令**
    ```
    hexo n #生成文章，或者source\_posts手动编辑
    hexo s #本地发布预览效果
    hexo g #生成public静态文件
    ```
    
***
# 发布到GitHub
***

1.  **部署到Github前需要配置`_config.yml`文件**
    ```
    # Deployment
    ## Docs: http://hexo.io/docs/deployment.html
    deploy:
      type:
    ```
    然后将它们修改 (Repository：必须是SSH形式的url)
    ```
    # Deployment
    ## Docs: http://hexo.io/docs/deployment.html
    deploy:
      type: git
      repository: git@github.com:username/username.github.io.git
      branch: master
    ```
    注意：如果你是为一个项目制作网站，那么需要把`branch`设置为gh-pages。
    
2.  **部署到Github**
    在blog目录里执行 
    ```
    hexo clean  #清理
    hexo generate   #生成静态页面到public目录
    hexo deploy     #将.deploy 目录部署到GitHub
    ```
    如果hexo部署失败` ERROR Deployer not found: git`执行下列命令 
    ```
    npm install hexo-deployer-git --save 
    ```
    首次创建耐心等待10分钟左右审核，之后即可访问静态主页如[http://guong.github.io](http://guong.github.io)
3.  **同步内容至GitHub**
    * 下载GitHub Windows,设置Local path,如E:\快盘\GitHub\
    * 运行Git Shell切换到如E:\快盘\GitHub\hexo路径下
    * 执行hexo g命令生成public文件夹
    * 把生成的内容全部拷贝到Local path或其子目录
    * 运行GitHub确认修改信息后执行右上角的Sync同步
    * 最后访问主页观察效果:[http://guong.github.io](http://guong.github.io)

***
# 其他相关
***
