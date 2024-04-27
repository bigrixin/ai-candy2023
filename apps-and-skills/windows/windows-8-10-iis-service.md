---
description: IIS Service Setting & .NET Core Application in Windows 8/10
---

# Windows 8/10, IIS Service

### Step one:

Control Panel -> 程序 -》程序特性(Programs and Features) -》启用或关闭Windows功能(Turn Windows features on or off) -》Internet Information Services-> -> Web 管理工具（Web Management Tools） ->>IIS 管理控制台(IIS Management Service) //【On】 -> 万维网服务(World Wide Web Services) ->> 应用开发功能 (Application Development Features) ->>>ASP.NET 4.8

浏览器 输入 http://localhost/ 看到页面，说明正常，否则需要设置

***

### Step two:

桌面-鼠标右键【管理】-> 【A】->【服务和应用程序】->IIS 管理器 【B】->【系统工具】\【事件查看器】\【日志】\【应用程序】->查看日志

1.  进入【A】-> Default Web Site. 点击右边【模块】： 是否有 AspNetCoreModuleV2，

    若没有需要下载和安装 dotnet-hosting-3.1.5-win.exe

    https://docs.microsoft.com/en-au/aspnet/core/host-and-deploy/iis/?view=aspnetcore-3.1#install-the-net-core-hosting-bundle
2. .NET Core Application 发布到Local: 方法:文件系统 Location: F:.NET\Publish\myshop\
   Configuration: Release - Any CPU Target Framework: netcoreapp3.1
3.  添加网站：

    网站名称物理地址 F:.NET\Publish\myshop\
    端口：8090
4. 应用程序池-》先选应用，双击， .NET CLR 版本改为：无托管代码
5. 浏览器 输入 http://localhost/8090

***

调试：

【B】日志，改日志事件目标为： 【日志文件和 ETW 事件】，应用

浏览器 输入 http://localhost/8090 有问题查 【B】- \[来源]：IIS AspNetCore Module

***

Reference:

Host ASP.NET Core on Windows with IIS: https://docs.microsoft.com/en-au/aspnet/core/host-and-deploy/iis/?view=aspnetcore-3.1#install-the-net-core-hosting-bundle

在IIS上部署你的ASP.NET Core项目 https://www.cnblogs.com/wangjieguang/p/core-iis.html
