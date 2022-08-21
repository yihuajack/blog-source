---
title: 从零开始的hexo与next主题配置github.io博客（入门篇）
categories: FrontEnd
tags:
  - hexo
  - github
abbrlink: 36835
date: 2020-09-27 18:47:19
---

1. 新建GitHub仓库名为<username>.github.io。

2. 安装Node.js及Git。

3. 安装hexo：

   ```
   npm install -g hexo-cli
   ```

4. 切换到要保存hexo博客的目录<blog>，执行

   ```
   hexo init
   ```

   或直接执行

   ```
   hexo init <folder>
   cd <folder>
   ```

   然后执行

   ```
   npm install
   ```

5. 安装主题，以Next为例：切换到博客文件夹下后执行

   ```bash
   git clone https://github.com/theme-next/hexo-theme-next themes/next
   ```

6. 编辑<blog>/_config.yml，填写title、subtitle、description、keywords、author、language。将url填写为http://<username>.github.io。如果之前使用的是旧版hexo需要参考[Hexo版本升级和Next主题升级之坑](https://whjkm.github.io/2018/07/17/Hexo版本升级和Next主题升级之坑/)更新后在time_format后添加update_option: 'mtime'。将theme改为next。添加plugin：

   ```
   plugin:
   - hexo-generator-feed
   feed:
     type: atom
     path: atom.xml
     limit: 20
   search:
     path: search.xml
     field: post
     format: html
     limit: 10000
   ```

   添加Deployment配置：

   ```
   deploy:
     type: git
     repository: https://github.com/<username>/<username>.github.io.git
     branch: master
   ```

7. 编辑<blog>/themes/next。更改scheme: Pisces。启用Dark Mode：将darkmode设置为true。启用commonweal等目录，将相应句子前的注释井号删掉。修改social配置，其中Google一栏plus.google已停用。links: Title:可添加其他地址。follow_me配置会在每篇文章末尾添加一个图标指向各个网站的链接，没必要。calendar配置将calendar_id改为Google邮箱，api_key需要在[Google API Console](https://console.developers.google.com/)，新建项目，然后在“API和服务”一栏下的“信息中心”一栏的页面中选择“ENABLE APIS AND SERVISES"如图：![img](https://img-blog.csdnimg.cn/20200927172709621.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lpaHVhamFjaw==,size_16,color_FFFFFF,t_70)
   搜索API和服务：![img](https://img-blog.csdnimg.cn/20200927172858560.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lpaHVhamFjaw==,size_16,color_FFFFFF,t_70)
   搜索Google Calendar：![img](https://img-blog.csdnimg.cn/20200927173001814.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lpaHVhamFjaw==,size_16,color_FFFFFF,t_70)
   点击Google Calendar API后点击“启用”，然后“创建凭据”：![img](https://img-blog.csdnimg.cn/2020092717313054.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3lpaHVhamFjaw==,size_16,color_FFFFFF,t_70)
   点击“选择”选择Google Calendar API，然后点击“API密钥”创建。
   github_banner配置中将enable设置为true，permalink设置为https://github.com/<username>。将pjax设置为true。

8. 新建博文：执行

   ```
   hexo new CSDN-1
   ```

   会显示

   ```
   INFO  Created: D:\Documents\Programming\Aykablog\source\_posts\CSDN-1.md
   ```

   其中标准命令为

   ```
   hexo new [layout] <title>
   ```

   [layout]可选项默认为post。

9. 执行

   ```
   hexo generate
   ```

   或

   ```
   hexo g
   ```

   显示

   ```
   INFO  Start processing
   INFO  Files loaded in 370 ms
   INFO  Deleted: images/arrow-right-32x32-next.png
   INFO  Deleted: images/arrow-right-16x16-next.png
   INFO  Deleted: images/arrow-right.eps
   INFO  Deleted: images/arrow-right.png
   INFO  Deleted: lib/font-awesome/HELP-US-OUT.txt
   INFO  Deleted: lib/font-awesome/css/font-awesome.css.map
   INFO  Deleted: images/arrow-right.svg
   INFO  Deleted: images/quote-l.svg
   INFO  Deleted: images/quote-r.svg
   INFO  Deleted: lib/font-awesome/bower.json
   INFO  Deleted: lib/font-awesome/fonts/fontawesome-webfont.woff
   INFO  Deleted: lib/font-awesome/fonts/fontawesome-webfont.woff2
   INFO  Deleted: lib/font-awesome/css/font-awesome.css
   INFO  Deleted: lib/font-awesome/css/font-awesome.min.css
   INFO  Deleted: lib/font-awesome/fonts/fontawesome-webfont.eot
   INFO  Deleted: images/arrow-right.ico
   INFO  Deleted: images/41254968094E73320D03EFF9B33277C03C1168186.jpg
   INFO  Generated: content.json
   INFO  Generated: board/index.html
   INFO  Generated: about/index.html
   INFO  Generated: tags/index.html
   INFO  Generated: categories/index.html
   INFO  Generated: archives/index.html
   INFO  Generated: archives/2020/index.html
   INFO  Generated: archives/2020/03/index.html
   INFO  Generated: tags/tutorial/index.html
   INFO  Generated: 2020/03/18/hello-world/index.html
   INFO  Generated: 2020/03/19/paper/index.html
   INFO  Generated: index.html
   INFO  Generated: js/algolia-search.js
   INFO  Generated: js/schemes/muse.js
   INFO  Generated: js/next-boot.js
   INFO  Generated: js/bookmark.js
   INFO  Generated: js/local-search.js
   INFO  Generated: css/main.css
   INFO  Generated: js/schemes/pisces.js
   INFO  Generated: js/utils.js
   INFO  Generated: lib/pjax/index.d.ts
   INFO  Generated: lib/font-awesome/webfonts/fa-regular-400.woff2
   INFO  Generated: lib/pjax/LICENSE
   INFO  Generated: lib/pjax/lib/abort-request.js
   INFO  Generated: lib/pjax/example/example.js
   INFO  Generated: lib/pjax/lib/execute-scripts.js
   INFO  Generated: lib/pjax/package.json
   INFO  Generated: lib/pjax/lib/is-supported.js
   INFO  Generated: lib/font-awesome/css/all.min.css
   INFO  Generated: lib/pjax/index.js
   INFO  Generated: lib/pjax/lib/foreach-selectors.js
   INFO  Generated: lib/pjax/lib/foreach-els.js
   INFO  Generated: lib/pjax/CHANGELOG.html
   INFO  Generated: lib/pjax/lib/parse-options.js
   INFO  Generated: lib/pjax/lib/eval-script.js
   INFO  Generated: lib/pjax/example/page2.html
   INFO  Generated: lib/pjax/lib/send-request.js
   INFO  Generated: lib/pjax/example/page3.html
   INFO  Generated: lib/pjax/example/forms.html
   INFO  Generated: lib/pjax/pjax.js
   INFO  Generated: lib/font-awesome/webfonts/fa-brands-400.woff2
   INFO  Generated: lib/pjax/example/index.html
   INFO  Generated: lib/pjax/README.html
   INFO  Generated: lib/font-awesome/webfonts/fa-solid-900.woff2
   INFO  Generated: lib/pjax/pjax.min.js
   INFO  Generated: lib/pjax/tests/setup.js
   INFO  Generated: lib/pjax/lib/switches-selectors.js
   INFO  Generated: lib/pjax/lib/uniqueid.js
   INFO  Generated: lib/pjax/lib/events/trigger.js
   INFO  Generated: lib/pjax/lib/switches.js
   INFO  Generated: lib/pjax/tests/test.ts
   INFO  Generated: lib/pjax/lib/events/on.js
   INFO  Generated: lib/pjax/lib/proto/parse-element.js
   INFO  Generated: lib/pjax/lib/events/off.js
   INFO  Generated: lib/pjax/lib/proto/log.js
   INFO  Generated: lib/pjax/lib/proto/attach-form.js
   INFO  Generated: lib/pjax/lib/util/extend.js
   INFO  Generated: lib/pjax/lib/proto/attach-link.js
   INFO  Generated: lib/pjax/lib/proto/handle-response.js
   INFO  Generated: lib/pjax/lib/util/contains.js
   INFO  Generated: lib/pjax/lib/util/clone.js
   INFO  Generated: lib/pjax/lib/util/update-query-string.js
   INFO  Generated: lib/pjax/lib/util/noop.js
   INFO  Generated: lib/pjax/tests/lib/proto/attach-form.js
   INFO  Generated: lib/pjax/tests/lib/util/clone.js
   INFO  Generated: lib/pjax/tests/lib/eval-scripts.js
   INFO  Generated: lib/pjax/tests/lib/abort-request.js
   INFO  Generated: lib/pjax/tests/lib/is-supported.js
   INFO  Generated: lib/pjax/tests/lib/execute-scripts.js
   INFO  Generated: lib/pjax/tests/lib/foreach-els.js
   INFO  Generated: lib/pjax/tests/lib/foreach-selectors.js
   INFO  Generated: lib/pjax/tests/lib/parse-options.js
   INFO  Generated: lib/pjax/tests/lib/events.js
   INFO  Generated: lib/pjax/tests/lib/switches.js
   INFO  Generated: lib/pjax/tests/lib/util/contains.js
   INFO  Generated: lib/pjax/tests/lib/switch-selectors.js
   INFO  Generated: lib/pjax/tests/lib/uniqueid.js
   INFO  Generated: lib/pjax/tests/lib/send-request.js
   INFO  Generated: lib/pjax/tests/lib/util/noop.js
   INFO  Generated: lib/pjax/tests/lib/util/extend.js
   INFO  Generated: lib/pjax/tests/lib/proto/attach-link.js
   INFO  Generated: lib/pjax/tests/lib/proto/parse-element.js
   INFO  Generated: lib/pjax/tests/lib/util/update-query-string.js
   INFO  Generated: lib/pjax/tests/lib/proto/handle-response.js
   INFO  84 files generated in 932 ms
   ```

10. 执行

    ```
    hexo server
    ```

    或

    ```
    hexo s
    ```

    显示

    ```
    INFO  Start processing
    INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
    ```

    打开浏览器打开该网址即可预览。

11. 如果没有更新，则需要执行

    ```
    hexo clean
    ```

    后重新生成并启动服务器。

12. 执行

    ```
    npm install hexo-deployer-git --save
    ```

    注意此处必须为--save选项，全局安装无效报错：

    ```
    INFO  Validating config
    ERROR Deployer not found: git
    ```

    执行

    ```
    hexo deploy
    ```

    或

    ```
    hexo d
    ```

    将博客部署到GitHub上：

    ```
    INFO  Validating config
    INFO  Deploying: git
    INFO  Setting up Git deployment...
    Initialized empty Git repository in D:/Documents/Programming/HexoBlog/.deploy_git/.git/
    [master (root-commit) 9cfa1ad] First commit
     1 file changed, 0 insertions(+), 0 deletions(-)
     create mode 100644 placeholder
    INFO  Clearing .deploy_git folder...
    INFO  Copying files from public folder...
    INFO  Copying files from extend dirs...
    warning: LF will be replaced by CRLF in 2019/07/27/CSDN-1/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in 2020/03/19/paper/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in 2020/09/27/CSDN-2/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in 2020/09/28/hello-world/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in about/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in archives/2019/07/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in archives/2019/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in archives/2020/03/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in archives/2020/09/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in archives/2020/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in archives/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in board/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in categories/CSDN/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in categories/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in css/main.css.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in js/algolia-search.js.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in js/bookmark.js.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in js/local-search.js.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in js/motion.js.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in js/next-boot.js.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in js/schemes/muse.js.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in js/schemes/pisces.js.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in js/utils.js.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in lib/anime.min.js.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in lib/font-awesome/css/all.min.css.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in lib/velocity/velocity.min.js.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in lib/velocity/velocity.ui.min.js.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in tags/Adobe/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in tags/Adobe安装错误/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in tags/Visual-Studio-Installer-Error/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in tags/index.html.
    The file will have its original line endings in your working directory
    warning: LF will be replaced by CRLF in tags/tutorial/index.html.
    The file will have its original line endings in your working directory
    [master 1d6cbde] Site updated: 2020-09-28 00:53:37
     50 files changed, 15669 insertions(+)
     create mode 100644 2019/07/27/CSDN-1/index.html
     create mode 100644 2020/03/19/paper/index.html
     create mode 100644 2020/09/27/CSDN-2/index.html
     create mode 100644 2020/09/28/hello-world/index.html
     create mode 100644 about/index.html
     create mode 100644 archives/2019/07/index.html
     create mode 100644 archives/2019/index.html
     create mode 100644 archives/2020/03/index.html
     create mode 100644 archives/2020/09/index.html
     create mode 100644 archives/2020/index.html
     create mode 100644 archives/index.html
     create mode 100644 board/index.html
     create mode 100644 categories/CSDN/index.html
     create mode 100644 categories/index.html
     create mode 100644 css/main.css
     create mode 100644 images/algolia_logo.svg
     create mode 100644 images/apple-touch-icon-next.png
     create mode 100644 images/avatar.gif
     create mode 100644 images/cc-by-nc-nd.svg
     create mode 100644 images/cc-by-nc-sa.svg
     create mode 100644 images/cc-by-nc.svg
     create mode 100644 images/cc-by-nd.svg
     create mode 100644 images/cc-by-sa.svg
     create mode 100644 images/cc-by.svg
     create mode 100644 images/cc-zero.svg
     create mode 100644 images/favicon-16x16-next.png
     create mode 100644 images/favicon-32x32-next.png
     create mode 100644 images/logo.svg
     create mode 100644 index.html
     create mode 100644 js/algolia-search.js
     create mode 100644 js/bookmark.js
     create mode 100644 js/local-search.js
     create mode 100644 js/motion.js
     create mode 100644 js/next-boot.js
     create mode 100644 js/schemes/muse.js
     create mode 100644 js/schemes/pisces.js
     create mode 100644 js/utils.js
     create mode 100644 lib/anime.min.js
     create mode 100644 lib/font-awesome/css/all.min.css
     create mode 100644 lib/font-awesome/webfonts/fa-brands-400.woff2
     create mode 100644 lib/font-awesome/webfonts/fa-regular-400.woff2
     create mode 100644 lib/font-awesome/webfonts/fa-solid-900.woff2
     create mode 100644 lib/velocity/velocity.min.js
     create mode 100644 lib/velocity/velocity.ui.min.js
     delete mode 100644 placeholder
     create mode 100644 tags/Adobe/index.html
     create mode 100644 "tags/Adobe\345\256\211\350\243\205\351\224\231\350\257\257/index.html"
     create mode 100644 tags/Visual-Studio-Installer-Error/index.html
     create mode 100644 tags/index.html
     create mode 100644 tags/tutorial/index.html
    Enumerating objects: 91, done.
    Counting objects: 100% (91/91), done.
    Compressing objects: 100% (66/66), done.
    Writing objects: 100% (91/91), 260.02 KiB | 6.84 MiB/s, done.
    Total 91 (delta 24), reused 0 (delta 0), pack-reused 0
    remote: Resolving deltas: 100% (24/24), done.
    To https://github.com/yihuajack/yihuajack.github.io.git
     + dd9b4c5...1d6cbde HEAD -> master (forced update)
    Branch 'master' set up to track remote branch 'master' from 'https://github.com/yihuajack/yihuajack.github.io.git'.
    INFO  Deploy done: git
    ```

13. 编辑<blog>/source/about/index.md即为“关于”页面。

14. 编辑<blog>/source/_posts下的相应Markdown文件即可编辑博文。在开头的Front-matter部分可以通过title编辑标题、date编辑日期、tags编辑标签、categories编辑分类。如果多条tags或categories使用语法：

    ```
    categories:
    - Diary
    tags:
    - PS3
    - Games
    ```

    参考官方文档：https://hexo.io/zh-cn/docs/。
