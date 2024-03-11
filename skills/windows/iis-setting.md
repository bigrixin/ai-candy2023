# IIS setting

https://www.youtube.com/watch?v=dZAbdmPrU4g

开始-控制面板-默认程序-程序和功能-打开或关闭windows功能 Control Panel\All Control Panel Items\Programs and Features Turn

select all Internet information service

注册asp.net 管理员身份打开命令行窗口。 输入cd C:\Windows\Microsoft.NET\Framework64\v4.0.30319 执行 aspnet\_regiis.exe -i

输入IIS， 进入IIS Manager (不要选IIS6） 选择左侧 应用程序池【Application Pools】, 鼠标右键选增加新 Application Pool 输入名称， 选择 .Net Framework v4.0.30319

选择刚添加的新Application Pool, 鼠标右键里的Advaned Settings 将标识【Identity】 里改为NetworkService

选择左侧网站【 Sites】, 添加 网站\[new Web Site], 输入名称，按选择，选刚才的添加的新Application Pool 输入发布的程序路径, 可以修改端口号

进入应用程序目录，选属性，添加新用户组 IIS\_IUSRS , 并给予Full control

若有500.24错误，打开身份验证【Authentication 】将 ASP.NET 模拟 \[Impersonation] 禁用

若局域网无法访问，打开Win防火墙，高级设置-入站规则，入站规则窗口中找到BranchCache内容检索(http-in)选项， 开启。

![](../../.gitbook/assets/IIS\_setup.JPG)
