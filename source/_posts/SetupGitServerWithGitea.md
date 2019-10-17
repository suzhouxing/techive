---
title: 用 Gitea 搭建自己的 Git 服务器
date: 2019-10-17 14:18:36
categories:
- 技术
tags:
- 技术
- Git
- Gitea
---
在永恒之蓝等通过网络文件共享传播的病毒肆虐之后, 很多网络供应商都封禁了文件共享相关端口, 以至于无法简单地通过映射网络驱动器来搭建 Git 的远端仓库, 然后使用 "哑协议" 执行 clone, push, pull 等操作. 因此, 我们可能需要使用支持 http 或者 ssh 的 Git 服务器.
目前功能最强大的免费 Git 服务器为 [Gitlab](https://about.gitlab.com/install/), 它可以让用户自己搭建与 https://github.com 或者 https://gitlab.com 一样的网站. 但 Gitlab 不支持免费用于商业用途. 此外, 同类开源或免费软件还有 [Bonobo](https://bonobogitserver.com/), [Gogs](https://gogs.io/), [SCM-Manager](https://www.scm-manager.org/), [Gitblit](http://gitblit.com/) 等.



# Windows 环境下操作步骤

## 第三方库依赖

1. 安装 Git
    1. 在[官方网站](https://git-scm.com/)下载最新版安装包
    2. 双击运行安装包按照安装向导的提示点下一步即可


## 基本流程

1. 在[官方网站](https://gitea.io)或者 [Github 的 release 页面](https://github.com/go-gitea/gitea/releases)下载最新版本
    - 截至本文发布时最新版本为 `gitea-1.9.4-windows-4.0-amd64.exe`

2. 将下载的文件重命名为 `gitea.exe` 并移动至任意目录下 (本文后续将使用 `C:\YOUR\INSTALL\PATH` 作为安装目录, 读者请根据自己的情况将其替换为实际的路径)

3. 以管理员身份打开命令行, 执行以下命令添加 Gitea 为 Windows 服务
    `sc create gitea start= auto binPath= ""C:\YOUR\INSTALL\PATH\gitea.exe" web --config "C:\YOUR\INSTALL\PATH\custom\conf\app.ini""`
    - 详情请见 https://docs.gitea.io/zh-cn/windows-service/
    - 在 PowerShell 中执行上述命令可能会失败, 需要使用经典的 cmd
    - 假设使用的管理员账号为 `admin`, 密码为 `123`, 后面其他地方要用时最好保持一致

4. 如果该服务未自动启动, 在 Windows 的服务列表中找到 `gitea` 并启动
    1. 以管理员身份打开服务列表
        - 开始按钮上点右键 -> 计算机管理 -> 服务和应用程序 -> 服务
        - Ctrl+Shift+Esc 打开任务管理器 -> 服务 -> 打开服务
        - 控制面板 -> 系统和安全 -> 管理工具 -> 服务
    2. 建议在 `gitea` 服务上点右键 -> 属性 -> 登陆 -> 此账户, 设置账户为之前使用的管理员账户 `admin`, 然后重启服务

5. 在浏览器中访问默认的地址 `http://localhost:3000` 打开网页端, 点击注册按钮
    1. 第一次点击注册时将进入初始配置页面
        1. 数据库推荐选择 SQLite3, 数据库路径建议使用默认的安装目录下的子目录
        2. `站点名称` 随便填
        3. `仓库根目录` 建议更改至安装目录下, 例如  `C:\YOUR\INSTALL\PATH\data\repo`
        4. `以用户名运行` 建议使用上面用过的管理员账号 `admin`
        5. 把 `SSH 服务域名` 和 `Gitea 基本 URL` 中的 `localhost` 替换为服务器的 IP 地址或域名
        6. 端口根据个人偏好设置, 设为 80 的话以后的仓库链接可以省略端口号, 但是有和其他 web 服务冲突的可能性, 建议使用默认端口
        7. 可选设置中的 `管理员账号设置` 为 Gitea 网页端的管理员, 不是操作系统的管理员, 如果此时不设置则第一个注册的用户将自动成为管理员
    2. 再次点击注册, 完成用户注册


## 疑难解答

1. 更改配置
    1. 在 `C:\YOUR\INSTALL\PATH\custom\conf\app.ini` 文件中修改相关项目
        - 网页端地址或者代码仓库地址前缀: `ROOT_URL`, `DOMAIN`, `SSH_DOMAIN`
        - 端口: `HTTP_PORT`, `SSH_PORT`
    2. 参考资料
        - https://docs.gitea.io/zh-cn/customizing-gitea/
        - https://docs.gitea.io/zh-cn/config-cheat-sheet/
        - https://github.com/go-gitea/gitea/blob/master/custom/conf/app.ini.sample

2. 局域网或者互联网上其他机器无法访问网页端
    1. 添加防火墙放行规则
        1. 控制面板 -> 系统和安全 -> Windows Defender 防火墙 -> 允许应用或功能通过 Windows Defender 防火墙 -> 更改设置 -> 允许其他应用 -> 浏览 -> 选中 `C:\YOUR\INSTALL\PATH\gitea.exe` -> 网络类型 -> 确保公用和专用均被勾选
    2. 更换端口
    3. 咨询单位网管或互联网服务提供商

3. 初始化设置时无法创建数据库
    1. 设置 `gitea` 服务以管理员账号启动
    2. 安装 SQLite3
        1. 从[官方网站](https://sqlite.org)下载 SQLite3
            - 截至本文发布时最新版本为 `sqlite-dll-win64-x64-3300100.zip`
        2. 将压缩包内的文件解压至任意目录下
            - 本文后续将使用 `C:\SQLITE3\INSTALL\PATH` 作为安装目录, 读者请根据自己的情况将其替换为实际的路径
    3. 在环境变量 `Path` 中添加安装目录
        1. 控制面板 -> 系统和安全 -> 系统 -> 高级系统设置 -> 环境变量 -> 系统变量 -> 双击变量 `Path`
        2. 新建一行 `C:\SQLITE3\INSTALL\PATH` 或者在变量值字符串开头添加 `C:\SQLITE3\INSTALL\PATH;`
