# Alist💾


<!-- Through Our Darkest Days - Mercenary -->
{{< music server="netease" type="song" id="27507087" autoplay="true" >}}

[Alist](https://alist.nn.ci/)是一个使用 [Gin](https://gin-gonic.com/zh-cn/) 和 [Solidjs](https://www.solidjs.com/) 开发的支持多种存储的文件列表程序，其具有优秀的性能和美观的用户界面，能够很好地对各种网盘以及本地存储文件进行管理和多平台访问（主要是视频、图像和音频）。

欢迎访问我的 Alist 站点：[sereinme's alist](http://59.66.175.32:5244)。

## 安装

由于本机用 Windows 系统作为服务主机，直接利用 [scoop](https://scoop.sh/) 进行安装，参考其官网中的介绍，在 `PowerShell` 中输入如下命令：

```pwsh
scoop bucket add main
scoop install main/alist
```

待安装完成后输入 `alist version` 来查看是否正确安装。


## 配置

Alist 具体配置在其官网有着极其详细的描述，在此只进行简单的配置和操作介绍，以下的步骤都是在安装了 Alist 的服务器主机中进行。

### 用户名和密码

刚完成 Alist 安装后，在终端输入 `alist server` 将开启 alist 服务，默认的服务是在本机的 5244 端口，初始用户名为 `admin` ，同时在终端输出中会提示用户的初始密码，利用此用户名和密码可以在浏览器的 `localhost:5244` 中登录 alist ，如果成功登录，则说明 alist 初始化成功。

之后在下方点击管理可以进入到 Alist 的配置，在其中的个人资料中修改用户名和登录密码，点击保存，就完成了用户名和密码的设置。

### 云盘和存储

添加各种存储的具体方式见 Alist 官网，其就是在 Alist 管理中选取存储，然后添加存储类型，之后按照官网中的描述进行配置，完成后就会在主页出现相应添加的存储部分。

### 开机自启

在 Windows 系统中实现开机自启有多种方法，笔者采用的是 Task Schedular (任务计划程序) 进行实现。

任务计划程序中创建任务的方式见参考资料，需要注意的是，其启动程序的脚本应是

```powershell
alist start
```

这样会打开之前配置完成的 Alist 服务，而如果用 `alist server` 的话，会打开一个全新未经过配置的 Alist 服务。

## 使用

在其他设备上使用 Alist 时，需要知道服务器主机的 IP 地址，可以通过 `ipconfig` 命令进行查看。

### 浏览器

对于没有经过 https 和端口配置等的 Alist 服务，直接在浏览器中输入

```url
http://<domain>:5244
```

就可以进入服务器主机的 Alist 界面中，在进行了登录之后能够对其中的文件进行查看和修改操作。

### WebDav

Alist 支持利用 WebDav 协议进行文件共享，其 WebDav 配置如下
                     
 Name      | Value                      
:---------:|:--------------------------:
 Url       | http[s]://domain:port/dav/ 
 Host      | domain                     
 路径        | dav                        
 协议        | http/https                 
 端口        | 与网页端一致                     
 WebDAV用户名 | 与网页端用户名一致                  
 WebDAV密码	 | 与网页端密码一致                                   



## 参考资料

- [Alist 官网](https://alist.nn.ci/)
- [Alist · Github](https://github.com/alist-org/alist)
- [用Windows自带的Task Scheduler部署一个定时任务启动一个程序](https://cloud.tencent.com/developer/article/1520728)


