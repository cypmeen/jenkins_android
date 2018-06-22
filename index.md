## Jenkins Android构建

### Jenkins介绍

Jenkins是一个独立的开源自动化服务器，可用于自动化各种任务，如构建，测试和部署软件。Jenkins可以通过本机系统包Docker安装，甚至可以通过安装Java Runtime Environment的任何机器独立运行

#### 概述
本节是入门指南的一部分。它提供了许多平台上基本的Jenkins配置的说明。但不涵盖安装Jenkins的全部注意事项或选项。

预安装
这些仅仅是入门，有关因素的全面讨论，请参见[硬件建议讨论](https://jenkins.io/doc/book/hardware-recommendations/) 。

系统要求

最小推荐配置：
- Java 8（JRE或JDK）
- 256MB可用内存
- 1GB +可用磁盘空间

推荐配置小团队：
- Java 8
- 1GB +免费内存
- 50GB +可用磁盘空间

#### 安装
**Unix/Linux**
在基于Debian的发行版，如Ubuntu，您可以通过安装Jenkins apt。最近的版本在一个apt存储库中可用。旧的但稳定的LTS版本在这个apt存储库。

### 第一步

 首先，我们把存储库秘钥存在系统中

```c
  wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
```
 当秘钥已经加入，系统返回ok时。下一步我们将增加一个Debian package repository adress到服务段的**sources.list**

```command
  echo deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
```
 当这两个都就绪后，我们将运行更新，以便apt-get将使用新的存储库：

```command
  sudo apt-get update
```
  最后，我们将安装Jenkins和它的依赖库，包含java
  
  ```c
    sudo apt-get install jenkins
  ```
  现在jenkins和其依赖库已经安装完成，如果JDK未安装请参考：[How to install Open JDK](https://stackoverflow.com/questions/14788345/how-to-install-the-jdk-on-ubuntu-linux)
  
### 第二步

使用systemctl启动jenkins，将它列为守护进程:

```command
  sudo systemctl start jenkins
```
由于systemctl命令不会显示输出，我们将使用status命令去验证它已经启动成功:

```command
  sudo systemctl status jenkins
```
如果一切顺利，输出的开始应该显示该服务是活动的，并配置为在启动时启动：

```shell
  Output
● jenkins.service - LSB: Start Jenkins at boot time
  Loaded: loaded (/etc/init.d/jenkins; bad; vendor preset: enabled)
  Active:active (exited) since Thu 2017-04-20 16:51:13 UTC; 2min 7s ago
    Docs: man:systemd-sysv-generator(8)
```

### 第三步

默认的,Jenkins运行在8080端口，所以我们将打开端口使用 ufw:

```command
  sudo ufw allow 8080
```
我们看见可以使用新的规则检查UFW's状态:
```command
  sudo ufw status
```
我们应该能看见traffic从哪儿都允许上报到8080端口:

```command
  Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
8080                       ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
8080 (v6)               we'll   ALLOW       Anywhere (v6)
```

> Note:Note: If the firewall is inactive, the following commands will make sure that OpenSSH is allowed and then enable it.

```command
  sudo ufw allow OpenSSH
  sudo ufw enable
```
从现在开始Jenkins已经安装了，并且防火墙允许去访问它，我们将完成初次安装。

### 第四步

安装完成后，我们将访问默认端口8080，使用服务器域名或者IP地址: 
http://ip_address_or_domain_name:8080

我们将看到"Unlock Jenkins"页面,将显示initial password.

![Jenkins](http://chuantu.biz/t6/331/1529630775x-1404764487.png)

在命令行界面，我们将使用cat命令去显示密码

![Jenkins](http://chuantu.biz/t6/331/1529630932x-1566657621.png)

我们将从终端复制32个字符的字母数字密码，并将其粘贴到“管理员密码”中。字段,然后单击“Continue"。下一个屏幕显示了安装建议插件或选择特定插件的选项。

我们点击“安装建议插件”选项，它将立即开始安装过程：

![Jenkins](http://chuantu.biz/t6/331/1529631043x-1566657621.png)

当安装完成后，我们将被提示设置第一个管理用户。我们可以跳过这一步，继续使用我们上面使用的初始密码作为admin，但是我们将花点时间来创建用户。

Note:The default Jenkins server is NOT encrypted, so the data submitted with this form is not protected. When you're ready to use this installation, follow the guide [How to Configure Jenkins with SSL using an Nginx Reverse Proxy](https://www.digitalocean.com/community/tutorials/how-to-configure-jenkins-with-ssl-using-an-nginx-reverse-proxy). This will protect user credentials and information about builds that are transmitted via the Web interface.

![Img](http://chuantu.biz/t6/331/1529631205x-1566657621.png)

一旦第一个管理员已经完成，你应该看见"Jenkins is ready" 确认界面。

![Img](http://chuantu.biz/t6/331/1529631292x-1566657621.png)

点击"Start using Jenkins"去访问Jenkins面板。

![Img](http://chuantu.biz/t6/331/1529631292x-1566657621.png)

### Support or Contact

