---
title: 从零开始的hexo与next主题配置github.io博客（进阶篇）
categories: FrontEnd
tags:
  - hexo
  - valine
  - leancloud
  - next
abbrlink: 53007
date: 2020-09-27 23:05:52
---

1. 注册并登录[LeanCloud](https://leancloud.cn/)。参考官方文档https://valine.js.org/quickstart.html创建开发版应用。

2. 执行

   ```
   npm install valine -g
   ```

   安装valine。

3. 在[Release](https://releases.leanapp.cn/#/leancloud/lean-cli/releases)页面下载安装lean-cli-setup-x64.msi。

4. 如出现

   ```
   获取 Class 列表失败
   The app is archived, please restore in console before use. [400 GET /data/6pVKQMTaE6R44OanMdxlS9Ku-gzGzoHsz/classes]
   ```

   参考[获取 Class 列表失败 - 急在线等](https://forum.leancloud.cn/t/class/22277)激活应用，在存储服务数据恢复中：“归档应用恢复中，该操作大约需要 10-30 分钟”，但实际上很快就能恢复。

5. 使用[Valine-Admin](https://github.com/DesertsP/Valine-Admin)参考官方文档：https://deserts.io/valine-admin-document/。注意第1步新版LeanCloud以迁移到LeanEngine->WEB->部署->部署->部署项目->Git部署，填写对应博客的GitHub仓库的HTTP链接或SSH链接，将其给出的Deploy Key添加到自己GitHub账号的SSH Key中。在“设置”下添加环境变量（包括ADMIN_URL）。返回“部署”，选择手动部署，选择目标环境为生产环境（也仅可选择生产环境），选择分支或提交为默认的"master"，保持hexo服务器处于开启状态，点击部署。

6. 将设置->应用Keys中的AppID和AppKey复制到next主题的_config.yml中的valine项下，enable设置为true，language设置为en, zh-cn，visitor设置为true，recordIP设置为false。重新部署hexo评论系统即成功添加。

7. 【deprecated】安装hexo-admin：

   ```
   npm install hexo-admin --save
   ```

8. 安装[hexo-filter-github-emojis](https://github.com/crimx/hexo-filter-github-emojis)以支持emoji渲染（无需卸载也切勿卸载hexo-renderer-ejs和hexo-renderer-marked，否则可能会出现“1 vulnerability requires manual review. See the full report for details.”这类无法修复的问题）

   ```
npm install hexo-filter-github-emojis --save
   ```

   同样切记一定要使用--save选项，不要全局安装。在Hexo _config.yml中添加
   
   ```
githubEmojis:
     enable: true
  className: github-emoji
     inject: true
   #   unicode: true
     styles:
   #     display: inline
   #     vertical-align: middle 
   #   localEmojis:
     customEmojis:
   ```
   
9. 添加coding.me仓库同步托管：在Hexo _config.yml的deploy项下的repo项下新增

   ```
   coding: git@e.coding.net:ayka/ayka.coding.me.git,master
   ```

   参考[Hexo在GitHub和Coding双线部署教程](https://www.heson10.com/posts/54971.html)设置coding.net项目（ps.在遇到这篇文章之前一度以为coding.net已取消了静态页面功能），如有需要自己配置域名也可参考该文章。
   使用GitHub注册[Vercel](https://vercel.com/)。在Import a Git Repository界面输入github.io仓库地址，FRAMEWORK PRESET选择Hexo，环境变量留空。
   Coding新建仓库，并新建访问令牌。将Hexo _config.yml的deploy项下的repo项下更改为github: https://<GitHub access token>@github.com/<username>/<username>.github.io.git和coding: https://<令牌用户名>:<访问令牌>@e.coding.net/<username>/<reponame>/<reponame>.git。在Hexo blog目录下执行

   ```
   git init
   ```

   执行

   ```
   git remote add origin https://<令牌用户名>:<访问令牌>@e.coding.net/<username>/<reponame>/<reponame>.git
   ```

   如报错

   ```
   fatal: remote origin already exists.
   ```

   则执行

   ```
   git remote rm origin
   ```

   后重新git remote add。执行

   ```
   git submodule add https://github.com/theme-next/hexo-theme-next themes/next
   git commit -m 'Initial commit'
   git push -u origin master
   ```

   打开项目设置->功能开关->持续集成，返回项目页面刷新后选择持续集成->构建计划->新建构建计划->选择构建计划模板->自定义构建过程，填写构建计划名称，构建过程->代码仓库选择之前用来存放博客源码的仓库，配置来源选择使用静态配置的Jenkinsfile确定后在流程配置->文本编辑器中添加：

   ```
   pipeline {
     agent any
     stages {
       stage('克隆项目') {
         steps {
           sh 'git clone https://oNTAWaPDGB:49028867e0b47d1b2a19b128396b89f43cf13df2@e.coding.net/ayka/hexoblog/hexoblog.git'
           sh 'ls -a'
         }
       }
       stage('安装依赖') {
         steps {
             sh 'ls -a'
             sh 'npm install -g hexo-cli'
             sh 'npm install hexo --save'
             sh 'npm install'
         }
       }
       stage("构建发布") {
         steps {
             sh 'hexo clean && hexo g && hexo d'
         }
       }
       stage('检出') {
         steps {
           checkout([
             $class: 'GitSCM',
             branches: [[name: env.GIT_BUILD_REF]],
             userRemoteConfigs: [[
               url: env.GIT_REPO_URL,
               credentialsId: env.CREDENTIALS_ID
             ]]])
           }
         }
         stage('自定义构建过程') {
           steps {
             echo '自定义构建过程开始'
           }
         }
       }
     }
   ```

   触发规则->代码源触发选择推送到master时触发构建保存修改。
   注册[Cloud Studio](https://cloudstudio.net/)（可直接使用coding.net账号登录）。新建工作空间（我自己创建），运行环境->预置环境选择Node,js，代码来源选择仓库，填写博客源码库的SSH。
   进入工作空间，在终端中执行

   ```
   npm install -g hexo-cli
   npm install hexo --save
   npm install
   ```

   即可生成。

10. 设置背景图片、半透明效果、博主头像实现圆形并旋转360°参考[Hexo+Next7.X 博客美化教程合集](https://www.heson10.com/posts/52911.html)。

11. 参考[为Hexo增加algolia搜索功能](https://www.heson10.com/posts/51572.html)。其中设置环境变量时如果使用Windows环境则将export命令改为执行

    ```
    setx HEXO_ALGOLIA_INDEXING_KEY your_api_key
    ```

    后重启Terminal。另可选增加本地搜索功能参考[hexo删除algolia搜索增加本地搜索功能](https://www.heson10.com/posts/2635.html)。

    注意：每次添加新的博文生成前都需执行

    ```
    hexo algolia
    ```

    以刷新索引。

12. 【deprecated】执行

    ```
    git clone https://github.com/theme-next/theme-next-canvas-nest source/lib/canvas-nest
    ```

    并在Next _config.yml中添加相关配置后启动服务器会报错：

    ```
    Unhandled rejection Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\index.swig)
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\index.swig) [Line 36, Column 23]
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\index.swig) [Line 39, Column 18]
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\index.swig)
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\_partials\head\head-unique.swig) [Line 10, Column 23]
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\index.swig) [Line 3, Column 3]
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\index.swig)
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\_partials\header\index.swig) [Line 6, Column 15]
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\index.swig)
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\_partials\header\sub-menu.swig) [Line 2, Column 29]
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\index.swig)
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\_partials\header\sub-menu.swig)
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\index.swig) [Line 5, Column 3]
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\index.swig) [Line 10, Column 14]
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\index.swig)
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\_partials\pagination.swig)
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\index.swig)
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\_partials\comments.swig)
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\index.swig)
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\_partials\languages.swig)
      Template render error: (D:\Documents\Programming\HexoBlog\themes\next\layout\_partials\footer.swig) [Line 63, Column 8]
      parseAggregate: expected comma after expression
        at Object._prettifyError (D:\Documents\Programming\HexoBlog\node_modules\nunjucks\src\lib.js:36:11)
        at D:\Documents\Programming\HexoBlog\node_modules\nunjucks\src\environment.js:561:19
        at Template.root [as rootRenderFunc] (eval at _compile (D:\Documents\Programming\HexoBlog\node_modules\nunjucks\src\environment.js:631:18), <anonymous>:43:3)
        at Template.render (D:\Documents\Programming\HexoBlog\node_modules\nunjucks\src\environment.js:550:10)
        at D:\Documents\Programming\HexoBlog\themes\next\scripts\renderer.js:32:29
        at _View._compiled (D:\Documents\Programming\HexoBlog\node_modules\hexo\lib\theme\view.js:136:50)
        at _View.render (D:\Documents\Programming\HexoBlog\node_modules\hexo\lib\theme\view.js:39:17)
        at D:\Documents\Programming\HexoBlog\node_modules\hexo\lib\hexo\index.js:64:21
        at tryCatcher (D:\Documents\Programming\HexoBlog\node_modules\bluebird\js\release\util.js:16:23)
        at D:\Documents\Programming\HexoBlog\node_modules\bluebird\js\release\method.js:15:34
        at RouteStream._read (D:\Documents\Programming\HexoBlog\node_modules\hexo\lib\hexo\router.js:47:5)
        at RouteStream.Readable.read (_stream_readable.js:467:10)
        at resume_ (_stream_readable.js:981:12)
        at processTicksAndRejections (internal/process/task_queues.js:84:21)
    ```

    Next已将canvas-nest配置废弃。

13. 执行

    ```
    npm install hexo-related-popular-posts --save
    ```

    配置Next _config.yml中的related_posts项如下

    ```
    related_posts:
      enable: true
      title: 相关文章 # Custom header, leave empty to use the default one
      display_in_home: false
      params:
        maxCount: 5
        #PPMixingRate: 0.0
        isDate: true
        #isImage: false
        #isExcerpt: false
    ```

14. 执行

    ```
    git clone https://github.com/theme-next/theme-next-fancybox3 themes/next/source/lib/fancybox
    ```

    安装fancybox。在Next _config.yml中配置fancybox项设置值为true。

15. 参考[Hexo+next主题自定义友情链接页面](https://www.heson10.com/posts/3243.html)。**注意：如果执行hexo clean后重新生成仍然显示之前的页面，需要关闭浏览器页面重新生成后打开。**

16. 参考[SEO优化：Hexo-abbrlink插件生成永久固定链接](https://www.heson10.com/posts/31426.html)。另外可使用[hexo-abbrlink2](https://github.com/rozbo/hexo-abbrlink2)。

17. 使用[PicGo](https://github.com/Molunerfinn/PicGo)图床。安装后设置GitHub图床设定仓库名为<username>/<username>.github.io，设定分支名为master，设定Token需要在GitHub Settings->Developer Settings->Personal access tokens->Generate new token，其中Select repos只需选定repo项即可，将生成的token填入设定Token中。改进：新建GitHub仓库并将仓库名填入设定仓库名中，指定存储路径为img/，设定自定义域名为[https://cdn.jsdelivr.net/gh//@master](https://cdn.jsdelivr.net/gh/yihuajack/pics-for-uploading@master)。

18. 执行

    ```
    npm install hexo-asset-image --save
    ```

    在Hexo _config.yml中的post_asset_folder设为true。执行hexo new <postname>发现其会自动创建一个同名的文件夹。在Typora->偏好设置->图像->插入图片时输入路径为D:\Documents\Programming\HexoBlog\source\\_posts\${filename}，前面的路径改为你所在的博客的路径，注意source与_post直接必须加两个反斜杠，否则hexo会将其转义成source_posts导致图片显示失败。如果要将之前的博文图片添加到asset文件夹中，只需手动创建同名文件夹，并将其中插入的图片放在该文件夹里面，在Typora中修改Markdown文件中图片所在路径即可。这种修改可在服务器运行时生效：

    ```
    update link as:-->/posts/52215/Documents\Programming\HexoBlog\source\_posts\CSDN补档-16\20200315101839605.png
    ```