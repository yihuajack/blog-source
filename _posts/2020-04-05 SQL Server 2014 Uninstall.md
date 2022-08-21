---
title: SQL Server 2014完全卸载与SQL Server 2019安装全记录
categories: CSDN补档
tags: sqlserver
abbrlink: 51259
date: 2020-04-05 01:19:36
---

1. 在服务中停止所有SQL Server相关服务。

2. 打开Visual Studio Installer，取消勾选SQL Server Express 2016 LocalDB、SQL ADAL运行时、SQL Server Data Tools、SQL Server ODBC Driver、SQL Server 命令行实用工具、SQL Server 支持的数据源、SQL Server 的 CLR 数据类型等，当然包括其连带的一大堆组件包括使用C++的桌面开发、C++核心桌面功能、体系结构和分析工具、Windows Communication Foundation、通用Windows平台开发、.NET桌面开发、通用Windows平台工具、.NET桌面开发工具，然后安装（卸载）。

3. 打开控制面板中的卸载程序，将Microsoft SQL Server 2012 Native Client、Microsoft SQL Server 2008 Setup Support Files及其安装程序、主程序、LocalDB、Microsoft SQL Server Compact 3.5 SP2及其x64版本、Microsoft System CLR Types for SQL Server、Microsoft VSS Writer for SQL Server、Microsoft OLE DB Driver for SQL Server等尽数删去，其中先卸载主程序，按步骤卸载掉实例后虽然还在列表中，但是卸载完其他项目后再卸载主程序会显示已卸载。注意如果有SQL Server Management Studio (SSMS)还要卸载SSMS。

4. 打开Windows Installer Clean Up，将上述各程序及其他SQL Server相关程序尽数删去。

5. 打开注册表管理器，在HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft下删除：Microsoft ODBC Driver 11 for SQL Server、Microsoft ODBC Driver 17 for SQL Server、Microsoft OLE DB Driver for SQL Server、Microsoft OLE DB Driver Redist、Microsoft SQL Server 2012 Redist、Microsoft SQL Server 2014 Redist、Microsoft SQL Server 2017 RC0 Redist、Microsoft SQL Server 2019 CTP2.2 Redist、Microsoft SQL Server Compact Edition、Microsoft SQL Server Native Client 11.0、Microsoft SQL Server、MSODBCSQL11、MSODBCSQL17、MSOLEDBSQL、MSSQLServer、SQLNCLI11等；在HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Session Manager下删除PendingFileRenameOperations；在HKEY_CLASSES_ROOT下删除MSOLEDBSQL.1、MSOLEDBSQL.AdvancedPage.1、MSOLEDBSQL.AdvancedPage、MSOLEDBSQL.AdvancedPage、MSOLEDBSQL.ConnectionPage.1、MSOLEDBSQL.ConnectionPage、MSOLEDBSQL.Enumerator.1、MSOLEDBSQL.Enumerator、MSOLEDBSQL.ErrorLookup.1、MSOLEDBSQL.ErrorLookup、MSOLEDBSQL、MSSQL.VDI.Client.2、MSSQL.VDI.Client、MSSQL.VDI.Server.2、MSSQL.VDI.Server、SQLNCLI11.1、SQLNCLI11.AdvancedPage.1、SQLNCLI11.AdvancedPage、SQLNCLI11.ConnectionPage.1、SQLNCLI11.ConnectionPage、SQLNCLI11.Enumerator.1、SQLNCLI11.Enumerator、SQLNCLI11.ErrorLookup.1、SQLNCLI11.ErrorLookup、SQLNCLI11、SQLOLEDB Enumerator.1、SQLOLEDB Enumerator、SQLOLEDB ErrorLookup.1、SQLOLEDB ErrorLookup、SQLOLEDB.1、SQLOLEDB、SQLServerProfilerTraceData、SQLServerProfilerTraceDef、SQLXMLX.1、SQLXMLX等；在HKEY_USERS\S-1-5-21-xxxxxxxxxx-xxxxxxxx-xxxxxxxxxx-xxxx\Software\Microsoft下删除Microsoft SQL Server；在HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft下删除Microsoft ODBC Driver 11 for SQL Server、Microsoft ODBC Driver 17 for SQL Server、Microsoft OLE DB Driver for SQL Server、Microsoft SQL Server Compact Edition、Microsoft SQL Server Native Client 11.0、Microsoft SQL Server、MSODBCSQL11、MSODBCSQL17、MSOLEDBSQL、MSSQLServer、SQLNCLI11；HKEY_USERS\S-1-5-18到S-1-5-8-21-...及S-1-5-80-...\Software下SQL Server相关注册表项。

6. 删除C:\Program Files和Program Files (x86)\Common Files\System\Ole DB中的sqloledb.dll、sqloledb.rll、sqlxmlx.dll、sqlxmlx.rll、msdasql.dll、msdasqlr.dll以及en-US、ja-JP、zh-CN\sqloledb.rll.mui、msdasqlr.dll.mui、sqlxmlx.rll.mui；删除C:\WINDOWS\system32\sqlcecompact40.dll、sqlceoledb40、sqlceqp40.dll、sqlcese40.dll、sqlncli11.dll、SQLServerManager12.msc、SqlServerSpatial120.dll、SqlServerSpatial130.dll、SqlServerSpatial150.dll、sqlsrv32.dll、sqlsrv32.rll；删除C:\WINDOWS\SysWOW64\sqlcecompact40.dll、sqlceoledb40、sqlceqp40.dll、sqlcese40.dll、sqlsrv32.dll、sqlsrv32.rll、sqlunirl.dll、sqlwid.dll、sqlwoa.dll等。

7. 使用Registrar Registry Manager搜索C:\Program Files\Microsoft SQL Server及C:\Program Files (x86)\Microsoft SQL Server，只选择数据，并选定全字匹配，搜索结果全部删除。

8. 删除C:\Program Files\Microsoft SQL Server、C:\Program Files (x86)\Microsoft SQL Server、C:\Program Files (x86)\Microsoft Analysis Services、C:\Users\<username>\AppData\Roaming\Microsoft\Microsoft SQL Server、C:\Users\<username>\AppData\Local\Microsoft\Microsoft SQL Server（把C:\Program Files\Microsoft SQL Server\150\Setup Bootstrap\Log留下）。

9. 打开SQL Server 2019安装程序，选择全新SQL Server独立安装或向现有安装添加功能，然后选择执行全新安装，指定可用版本为Developer，选择实例功能：数据库引擎服务、SQL Server复制、全文和语义提取搜索、Data Quality Services、Analysis Services和共享功能：Data Quality Client、客户端工具连接、客户端工具向后兼容性、客户端工具SDK、SQL Server客户端连接SDK（至此我们终于可以为实例功能、共享功能、共享功能（x86）都选择自己的安装目录，并且所有功能都没有变成灰色不可选或已选择的状态），选择命名实例为MSSQLSERVER，数据库引擎配置->服务器配置->身份验证模式为混合模式】设定密码并添加当前用户，再添加Administrators检查名称后的结果。

10. 第一次报错：注册表权限问题，将提示的注册表十六进制代码在注册表中搜索定位到所在上一级Components（我的在HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Installer\UserData\S-1-5-18\Components）设置权限所有者为Administrators，启用继承，设置权限为Administrators和Users都可完全控制（记得之后改回原样，该注册表权限与Windows搜索结果菜单和开始菜单图标右键菜单高度相关）。

11. 第二次报错：服务没有及时响应启动或控制请求。查询C:\Program Files\Microsoft SQL Server\150\Setup Bootstrap\Log下相应文件夹里的Details.txt找到这条报错信息向上溯源，发现很多feature都failed in result Result或ValidateResult，向上查找到最早的报错Result为E:\Program Files\Microsoft SQL Server\MSAS15.MSSQLSERVER\OLAP\和E:\Program Files\Microsoft SQL Server\MSAS15.MSSQLSERVER\OLAP\bin\，发现服务中的MSSQLServerOLAPService启动后迅速停止。再向上寻找发现报错信息：Slp: Sco: File 'G:\x64\setup\x64\RsFx.msi' does not exist、Slp: Sco: File 'G:\2052_CHS_LP\x64\setup\x64\sql_as_loc.msi' does not exist等，由于此时尚未安装MSSQLSERVER服务所以不能在服务上做文章，多次重试无用后取消，最后安装失败，共享功能全部成功安装，实例功能全部安装失败，最终错误代码为0x80004005。于是将ISO镜像文件解压并将其从移动硬盘复制到本地硬盘。试图使用修复但是现实修复程序没有作出更改，因为数据库引擎服务本就没有配置好，好在我们之前的安装是按照安装程序一步步安装的，我们可以在卸载程序中右键Microsoft SQL Server (64位)卸载/更改然后选择卸载所有实例功能。同时，安装还试图重装IIS，但是需要重启而未重启，总之没能解决该错误。试图通过命令行启动SQL Server返回错误信息服务没有响应控制功能。搜索报错信息Configuration action failed for feature SQL_Engine_Core_Inst during timing Startup and scenario Startup也没有得到解决办法。重启后通过本地硬盘中的安装程序重新安装。

12. 第三次报错：服务没有及时响应启动或控制请求。这次更奇怪的是与第二次不同，没有文件does not exist，也没有Result和ValidateResult的报错信息，安装顿时陷入最艰难的情形。不过这次MSSQLSERVER已安装好，打开计算机管理（本地）->系统工具->事件查看器->Windows 日志->系统大量报错：等待……服务的连接超时(60000 毫秒)、由于下列错误，——服务启动失败: 服务没有及时响应启动或控制请求，更改MSSQLSERVER为本地系统登录后事件信息是SQL Server (MSSQLSERVER) 服务由于下列服务特定错误而终止: %%945。更改E:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Log的权限后进入该目录打开ERRORLOG及ERRORLOG.x查看报错physical file "d:\dbs\sh\s19s\0924_133725\cmd\2\obj\x64retail\sql\mkmastr\databases\mkmastr.proj\MSDBData.mdf、model.mdf". Operating system error 3: "3(系统找不到指定的路径。)"、MSDBLog.ldf不正确以及spid23s   SQL Server failed to communicate with filter daemon launch service  (OS error: 服务没有及时响应启动或控制请求等，搜索错误代码9954也没有结果。结束本次安装，发现Analysis Service成功安装。

13. 受[SqlServer 错误1053：服务并未及时响应启动或控制请求](https://www.cnblogs.com/woxpp/p/5607908.html)启发，在安装到最后正在注册服务将要报错时在服务中依次将注册的SQL Server Full-text Filter Daemon Launcher (MSSQLSERVER)、SQL Server (MSSQLSERVER)、SQL Server Analysis Services (MSSQLSERVER)、SQL Server Analysis Services CEIP (MSSQLSERVER)、SQL Server Browser、SQL Server CEIP 服务 (MSSQLSERVER)、SQL Server VSS Writer、SQL Server 代理 (MSSQLSERVER)改为本地系统账户登录。安装成功。

14. 另外一种更靠谱的方法：在安装失败后，SQL Server各服务都已经被添加好，只是无法启动而已，在计算机管理（本地）->系统工具->本地用户和组->组->Administrators->添加到组->添加NT Service\MSSQLSERVER、NT Service\MSSQLServerOLAPService、NT Service\MSSQLFDLauncher、NT Service\SQLSERVERAGENT，然后搜索本地安全策略->安全设置->本地策略->用户权限分配->允许本地登录->属性->添加用户或组中添加上述四项，然后卸载数据库引擎服务、SQL Server复制、全文和语义提取搜索、Data Quality Services、Analysis Services，然后重新安装时在数据库引擎配置->服务器配置->指定SQL Server管理员添加当前用户和Administrators，在下一步Analysis Services配置->指定哪些用户具有对Analysis Services的管理权限中也同样如此指定。顺利安装，并且服务登录为NT Service\...。

15. 为解决SQL Server配置管理器中SQL Server网络配置和SQL Native Client 11.0配置只有32位的问题重新安装，这次清理了所有SQL Server、SQLServer、MSSQL、Microsoft ODBC、Microsoft OLE DB、MSODBCSQL、SQLOLEDB、SQLNCLI、SQLXMLX、sqlboot、SqlDumper、sqlmgmprovider、svrenumapi、instapi150等注册表，教训是搜索注册表时尽量不要勾选搜索数据，只需要勾选项和值即可，否则必然会导致误删，如Autorecover MOFs、环境变量、AppCompatCache等。

16. 第一次报错：已添加了具有相同键的项。解决方法：删除C:\Users\<username>\AppData\Local\Microsoft_Corporation\LandingPage.exe*文件夹。

17. 第二次报错：注册表权限问题，参考[Windows 10无法打开注册表 由于某个错误无法打开该密钥(详细信息:拒绝访问)且无法在注册表上设置新的所有者拒绝访问的解决方案](https://blog.csdn.net/yihuajack/article/details/104395132)更改计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Installer\UserData\S-1-5-18\Components权限。

18. 第三次卡在SqlEngineDBStartConfigAction_install_configrc_Cpu64报错（SQLEngine: --SqlServerServiceSCM: Waiting for nt event 'Global\sqlserverRecComplete' to be created、SQLEngine: --SqlServerServiceSCM: Wait for creation of event handle 'Global\sqlserverRecComplete' has timed out）：AS或SQLEngine: --SqlEngineSetupPrivate: Failed to set register with Software Usage Metrics: System.IO.FileNotFoundException: 系统找不到指定的文件。 (异常来自 HRESULT:0x80070002)、SQLEngine: --SqlServerServiceSCM: Exception happens at start SQL Engine attempt 1. Exception: 找不到数据库引擎启动句柄之解决方法：安装时在服务器配置->服务账户->SQL Server数据库引擎->账户名称修改为NT AUTHORITY\SYSTEM，注意不用查找账户直接输入该名称确定。然后顺利安装。安装后MSSQLSERVER采用的是本地系统登录。

19. 第四次发现SQL Server主服务启动不了，修复又报错找不到数据库引擎启动句柄，发现SQL Server服务的登录方式被改回了NT Service\MSSQLSERVER并且无法启动，报错启动失败或请求失败或服务未及时响应，解决方法是在服务中将其登录的登录身份改为本地系统账户即可。

20. 打开SSMS，注意输入的是计算机名称，而不是MSSQLSERVER等乱七八糟的名称，否则会报错：

    ```
    ===================================
     
    无法连接到 MSSQLSERVER。
     
    ===================================
     
    在与 SQL Server 建立连接时出现与网络相关的或特定于实例的错误。未找到或无法访问服务器。请验证实例名称是否正确并且 SQL Server 已配置为允许远程连接。 (provider: Named Pipes Provider, error: 40 - 无法打开到 SQL Server 的连接) (.Net SqlClient Data Provider)
     
    ------------------------------
    有关帮助信息，请单击: http://go.microsoft.com/fwlink?ProdName=Microsoft%20SQL%20Server&EvtSrc=MSSQLServer&EvtID=53&LinkId=20476
     
    ------------------------------
    错误号: 53
    严重性: 20
    状态: 0
     
     
    ------------------------------
    程序位置:
     
       在 System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString connectionOptions, SqlCredential credential, Object providerInfo, String newPassword, SecureString newSecurePassword, Boolean redirectedUserInstance, SqlConnectionString userConnectionOptions, SessionData reconnectSessionData, DbConnectionPool pool, String accessToken, Boolean applyTransientFaultHandling, SqlAuthenticationProviderManager sqlAuthProviderManager)
       在 System.Data.SqlClient.SqlConnectionFactory.CreateConnection(DbConnectionOptions options, DbConnectionPoolKey poolKey, Object poolGroupProviderInfo, DbConnectionPool pool, DbConnection owningConnection, DbConnectionOptions userOptions)
       在 System.Data.ProviderBase.DbConnectionFactory.CreateNonPooledConnection(DbConnection owningConnection, DbConnectionPoolGroup poolGroup, DbConnectionOptions userOptions)
       在 System.Data.ProviderBase.DbConnectionFactory.TryGetConnection(DbConnection owningConnection, TaskCompletionSource`1 retry, DbConnectionOptions userOptions, DbConnectionInternal oldConnection, DbConnectionInternal& connection)
       在 System.Data.ProviderBase.DbConnectionInternal.TryOpenConnectionInternal(DbConnection outerConnection, DbConnectionFactory connectionFactory, TaskCompletionSource`1 retry, DbConnectionOptions userOptions)
       在 System.Data.ProviderBase.DbConnectionClosed.TryOpenConnection(DbConnection outerConnection, DbConnectionFactory connectionFactory, TaskCompletionSource`1 retry, DbConnectionOptions userOptions)
       在 System.Data.SqlClient.SqlConnection.TryOpenInner(TaskCompletionSource`1 retry)
       在 System.Data.SqlClient.SqlConnection.TryOpen(TaskCompletionSource`1 retry)
       在 System.Data.SqlClient.SqlConnection.Open()
       在 Microsoft.SqlServer.Management.SqlStudio.Explorer.ObjectExplorerService.ValidateConnection(UIConnectionInfo ci, IServerType server)
       在 Microsoft.SqlServer.Management.UI.ConnectionDlg.Connector.ConnectionThreadUser()
    ===================================
    找不到网络路径。
    ```

21. 在SQL Server配置管理器仍然没有64位的节点的情况下，可尝试修改计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\150和计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL15.MSSQLSERVER下的注册表，例如TCP/IP是在计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQLServer\SuperSocketNetLib\Tcp。将其Enabled值设为1，查看其子项IP列表，如IP14的IpAddress为127.0.0.1，TcpPort为1433，那么就可以通过服务器名称为127.0.0.1,1433登录，注意如果通过Windows身份验证会报错：

    ```
    ===================================
     
    无法连接到 127.0.0.1,1433。
     
    ===================================
     
    登录失败。该登录名来自不受信任的域，不能与集成身份验证一起使用。 (.Net SqlClient Data Provider)
     
    ------------------------------
    有关帮助信息，请单击: http://go.microsoft.com/fwlink?ProdName=Microsoft%20SQL%20Server&EvtSrc=MSSQLServer&EvtID=18452&LinkId=20476
     
    ------------------------------
    服务器名称: 127.0.0.1,1433
    错误号: 18452
    严重性: 14
    状态: 1
    行号: 65536
     
     
    ------------------------------
    程序位置:
     
       在 System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString connectionOptions, SqlCredential credential, Object providerInfo, String newPassword, SecureString newSecurePassword, Boolean redirectedUserInstance, SqlConnectionString userConnectionOptions, SessionData reconnectSessionData, DbConnectionPool pool, String accessToken, Boolean applyTransientFaultHandling, SqlAuthenticationProviderManager sqlAuthProviderManager)
       在 System.Data.SqlClient.SqlConnectionFactory.CreateConnection(DbConnectionOptions options, DbConnectionPoolKey poolKey, Object poolGroupProviderInfo, DbConnectionPool pool, DbConnection owningConnection, DbConnectionOptions userOptions)
       在 System.Data.ProviderBase.DbConnectionFactory.CreateNonPooledConnection(DbConnection owningConnection, DbConnectionPoolGroup poolGroup, DbConnectionOptions userOptions)
       在 System.Data.ProviderBase.DbConnectionFactory.TryGetConnection(DbConnection owningConnection, TaskCompletionSource`1 retry, DbConnectionOptions userOptions, DbConnectionInternal oldConnection, DbConnectionInternal& connection)
       在 System.Data.ProviderBase.DbConnectionInternal.TryOpenConnectionInternal(DbConnection outerConnection, DbConnectionFactory connectionFactory, TaskCompletionSource`1 retry, DbConnectionOptions userOptions)
       在 System.Data.ProviderBase.DbConnectionClosed.TryOpenConnection(DbConnection outerConnection, DbConnectionFactory connectionFactory, TaskCompletionSource`1 retry, DbConnectionOptions userOptions)
       在 System.Data.SqlClient.SqlConnection.TryOpenInner(TaskCompletionSource`1 retry)
       在 System.Data.SqlClient.SqlConnection.TryOpen(TaskCompletionSource`1 retry)
       在 System.Data.SqlClient.SqlConnection.Open()
       在 Microsoft.SqlServer.Management.SqlStudio.Explorer.ObjectExplorerService.ValidateConnection(UIConnectionInfo ci, IServerType server)
       在 Microsoft.SqlServer.Management.UI.ConnectionDlg.Connector.ConnectionThreadUser()
    ```

    改为通过SQL Server身份验证登录即可。