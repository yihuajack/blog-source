---
title: 使用MAMP和Tsugi构建PY4E、XDebug调试PHP总结（以Windows环境为例）
categories: CSDN补档
tags:
  - mysql
  - apache
abbrlink: 14203
date: 2020-04-12 17:12:43
---

1. 安装MAMP。

2. 在E:\MAMP\htdocs下

   ```
   git clone https://github.com/csev/py4e.git
   cd py4e
   git clone https://github.com/csev/tsugi.git
   ```

   项目很大，可以考虑导入到gitee再克隆。

3. 打开MAMP Pro，Hosts->General->Port number改为8888，在Port->MySQL改为8889，启动Apache和MySQL服务器，点击Administer MySQL with: phpMyAdmin，如果没打开查看是不是端口错误，可直接在浏览器输入http://localhost:8888/phpMyAdmin打开。

4. 确认phpMyAdmin顶部显示为Server: localhost:8889，然后点击选项卡SQL，粘贴下列内容：

   ```
   CREATE DATABASE tsugi DEFAULT CHARACTER SET utf8;
   GRANT ALL ON tsugi.* TO 'ltiuser'@'localhost' IDENTIFIED BY 'ltipassword';
   GRANT ALL ON tsugi.* TO 'ltiuser'@'127.0.0.1' IDENTIFIED BY 'ltipassword';
   ```

   并执行。

5. 将E:\MAMP\htdocs\py4e\tsugi下的config-dist.php复制一份重命名为config.php，然后编辑config：

   ```
   $wwwroot = 'http://localhost:8888/py4e/tsugi';   // Embedded Tsugi localhost
    
   ...
    
   $CFG->pdo = 'mysql:host=127.0.0.1;port=8889;dbname=tsugi'; // MAMP
   $CFG->dbuser    = 'ltiuser';
   $CFG->dbpass    = 'ltipassword';
    
   ...
    
   $CFG->adminpw = 'short';
    
   ...
    
   $CFG->apphome = 'http://localhost:8888/py4e';
   $CFG->context_title = "Python for Everybody";
   $CFG->lessons = $CFG->dirroot.'/../lessons.json';
    
   ... 
    
   $CFG->tool_folders = array("admin", "../tools", "../mod");
   $CFG->install_folder = $CFG->dirroot.'./../mod'; // Tsugi as a store
    
   ...
    
   $CFG->servicename = 'PY4E';
   ```

   其中$CFG->adminpw可自己设定。 

6. 打开http://localhost:8888/py4e/tsugi/admin/看到提示需要Upgrade database，既可以直接点击Upgrade database，也可以在E:\MAMP\htdocs\py4e\tsugi\admin下执行php upgrade.php（但这种方法没成功），刷新后显示正常。

7. 在phpMyAdmin左边栏展开tsugi->Tables点击lti_key，点击key_key为google.com的key的Edit，找到secret列确认其值不为空，否则应创建一个十六进制值。

8. 点击[Manage Installed Modules](http://localhost:8888/py4e/tsugi/admin/install)发现报错Fatal error: in E:\MAMP\htdocs\py4e\tsugi\vendor\tsugi\lib\src\Util\GitRepo.php on line 217，在MAMP ->Languages->PHP->Extensions中启用XDebug (Debugger)后显示了更详细的报错信息指向E:\MAMP\htdocs\py4e\tsugi\admin\install\index.php第24行：

   ```
   $git_version = $repo->run('--version');
   ```

   以及上述GitRepo.php的第217行及其前是

   ```
   $resource = proc_open($command, $descriptorspec, $pipes, $cwd, $env);
    
   $stdout = stream_get_contents($pipes[1]);
   $stderr = stream_get_contents($pipes[2]);
   foreach ($pipes as $pipe) {
   			fclose($pipe);
   }
    
   $status = trim(proc_close($resource));
   if ($status) throw new \Exception($stderr);
   ```

   发现是之前删注册表删错了把Git for Windows的PATH删掉了，补上以后在MAMP->Settings->Hosts->Databases中发现已经找到Tsugi了，勾选好以后重启服务器后刷新恢复正常，显示：This screen is a wrapper for the git command if it is installed in your system. This screen runs git commands on your behalf. It will only handle the normal operations and assume that they work. If you log in and edit the files after they are checked out, this tool might not be able to upgrade some of these git repos.
   Using: git version 2.26.0.windows.1

9. 打开Available Modules选项卡发现提示Cannot be installed via this page，点击锁的图标弹出报错信息Readonly Detail
   This server does not appear to allow the git command to make changes to the web files. So this tool will not be able to install, update, or reconfigure any of these tools. You will have to update these files some other way. Alternatively, you may be able to configure a copy of git that can update the file system - see the documentation for $CFG->git_command in the config-dist.php file.打开config.php，找到第347-358行：

   ```
   // On a Mac localhost, if you have git installed, you should be able to
   // use the auto-install with no further configuration.
    
   // On Windows, to run the automatic install of modules:
   // (1) Make sure Git is installed (https://git-scm.com/download/win
   // Maybe also install a GIT GUI https://git-scm.com/downloads/guis
   // (2) Open "cmd" and type "git --version"
   // this should give you the current version of git. If this fails
   // then git is not setup in your path
   // (Control Panel > System and Security > System > Advanced System Settings > Environment Variables)
   // (3) Then here in "config.php":
   // $CFG->git_command = 'git'
   ```

   后来Available Modules选项卡干脆显示No available modules
   Unable to decode https://api.github.com/users/tsugitools/repos?language=PHP。把第358行的注释去掉后记得加分号后刷新。

10. 为找出Cannot be installed via this page原因，重新配置git环境变量和重装git没有效果，为MAMP配置XDebug。MAMP->Languages->PHP->Default version->Open，在打开的php7.3.7.ini里的[XDebug]下添加zend_extension="E:\MAMP\bin\php\php7.3.7\ext\php_xdebug.dll"，在E:\MAMP\conf\php7.3.7\php.ini中也这样添加。注意：MAMP无需自己创建php文件输出phpinfo或者用php -i和php -m，只需打开http://localhost:8888/MAMP/（或者相应的主机和端口）并点击顶部菜单栏的phpInfo即可，在phpInfo网页中搜索找到XDebug。使用php -i然后将输出结果复制到https://xdebug.org/wizard分析的结果为
    Summary
    Xdebug installed: no
    Server API: Command Line Interface
    Windows: yes - Compiler: MS VC 15 - Architecture: x86
    Zend Server: no
    PHP Version: 7.3.7
    Zend API nr: 320180731
    PHP API nr: 20180731
    Debug Build: no
    Thread Safe Build: yes
    OPcache Loaded: no
    Configuration File Path: unknown
    Configuration File: unknown
    Extensions directory: C:\php\ext
    Instructions
    Download php_xdebug-2.9.4-7.3-vc15.dll
    Move the downloaded file to C:\php\ext
    Create php.ini in the same folder as where php.exe is and add the line
    zend_extension = C:\php\ext\php_xdebug-2.9.4-7.3-vc15.dll

11. 在VSCode中安装PHP Debug，配置

    ```
    "debug.allowBreakpointsEverywhere": true,
    "php.validate.executablePath": "E:\\MAMP\\bin\\php\\php7.3.7\\php.exe"
    ```

    运行->Launch currently open script，当前脚本为E:\MAMP\htdocs\py4e\tsugi\admin\install\index.php，生成launch.json（E:\MAMP\htdocs\py4e\tsugi\.vscode\launch.json）保持默认即可。打开

    http://localhost:8888/py4e/tsugi/admin/install/

    ，更多工具->开发人员工具->Sources，刷新网页。调试控制台显示

    ```
    Error: spawn php ENOENT
        at Process.ChildProcess._handle.onexit (internal/child_process.js:264:19)
        at onErrorNT (internal/child_process.js:456:16)
        at processTicksAndRejections (internal/process/task_queues.js:77:11) {
      errno: 'ENOENT',
      code: 'ENOENT',
      syscall: 'spawn php',
      path: 'php',
      spawnargs: [ 'e:\\MAMP\\htdocs\\py4e\\tsugi\\admin\\install\\index.php' ]
    }
    ```

    调用堆栈->Launch currently open script在{main}（E:\MAMP\htdocs\py4e\tsugi\admin\install\repos_json.php）中第58行

    ```
    addRepoInfo($tsugi, $repo);
    ```

    =>addRepoInfo（E:\MAMP\htdocs\py4e\tsugi\admin\install\install_util.php）第146行

    ```
    $update = $repo->run('remote update');
    ```

    =>Tsugi\Util\GitRepo->run（E:\MAMP\htdocs\py4e\tsugi\vendor\tsugi\lib\src\Util\GitRepo.php）第232行

    ```
    return $this->run_command(Git::get_bin()." ".$command);
    ```

    =>Tsugi\Util\GitRepo->run_command（E:\MAMP\htdocs\py4e\tsugi\vendor\tsugi\lib\src\Util\GitRepo.php）第217行抛出异常：

    ```
    出现异常。
    Exception: Logon failed, use ctrl+c to cancel basic credential prompt.
    bash: /dev/tty: No such device or address
    error: failed to execute prompt script (exit code 1)
    fatal: could not read Username for 'https://gitee.com': No such file or directory
    error: Could not fetch origin
    ```

    由于GitHub克隆很慢，我的tsugi是导出到Gitee再克隆到本地的。

12. 本机对于github.com、coding.net、gitee.com等配置了同一个SSH-key，所以只需在Git Bash输入ssh -T git@gitee.com然后输入yes即可添加gitee.com到known_hosts，然后显示Hi <username>! You've successfully authenticated, but GITEE.COM does not provide shell access.成功。

13. 重试debug发现还是同样的问题，给Gitee重新添加SSH公钥也没有用，尝试在Git Bash中执行git clone git@gitee.com:<username>/<gitname>.git报错：

    ```
    CreateProcessW failed error:193
    ssh_askpass: posix_spawn: Unknown error
    Host key verification failed.
    ```

    发现可能原因是由于HOME目录被更改过所以Git Bash使用的.ssh在另外一个目录E:\Cadence\SPB_Data，而刚刚执行的操作只更新了这个目录中的known_hosts而没有改变C:\Users\<username>\.ssh\known_hosts，于是将两个known_hosts文件手动同步。然后再次执行git clone（SSH方式）发现成功。

14. 重试debug发现还是同样的问题，参考[fatal: could not read Username for 'https://github.com': No such device or address #20](https://github.com/markomarkovic/simple-php-git-deploy/issues/20)将tsugi目录下的.git目录下的config中URL改成git形式，使用ssh -T git@gitee.com成功，确定git config user.name为账户名称，重试debug，没有抛出任何通知、警告、错误或异常。
