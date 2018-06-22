## Jenkins Android安装和配置说明

### Jenkins介绍

Jenkins是一个独立的开源自动化服务器，可用于自动化各种任务，如构建，测试和部署软件。Jenkins可以通过本机系统包Docker安装，甚至可以通过安装Java Runtime Environment的任何机器独立运行

#### 概述
本节是入门指南的一部分。它提供了Android平台基本的Jenkins配置的说明。但不涵盖安装Jenkins的全部注意事项或选项。

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




### Jenkins Android配置

先看看效果：

构建历史与归档:
![Img](http://chuantu.biz/t6/331/1529632872x-1566657621.png)

分支选择与环境选择:
![Img](http://chuantu.biz/t6/331/1529632910x-1566657621.png)

#### 开始配置:

#### 分支选择配置
使用插件: Git Parameter

进入项目的configure，在General页面的Description里面编写打包说明。

勾选 This project is parameterized在选框里点击 Add parameter并选择Git parameter。

![Img](http://chuantu.biz/t6/331/1529633418x-1566657621.png)

Name:随便命一个名，将作为分支选择的环境变量

Description:分支说明

Parameter Type:选择Branch (按照个人或者公司需要配置)

#### 打包环境选择

![Img](http://chuantu.biz/t6/331/1529633621x-1566657621.png)

和上述一样选择 Add Parameter的选择框选择Choice Parameter的选项

Name:ENVIRONMENT(环境变量)

Choices:项目gradle 里配置的打包环境

#### Git仓库配置

选择Git,
Repository URL: 填写仓库地址
Credentials: 选择SSH秘钥

Branches to build:

Branch Specifier (blank for 'any'): refs/remotes/${GIT_BRANCH} 注:GIT_BRANCH之前配置的环境变量


#### 构建环境配置

![Img](http://chuantu.biz/t6/331/1529634300x-1566657621.png)

勾选Set Build Name(这里使用**Build Name Setter Plugin**)

#### [Build Name Setter Plugin]()

搜 `build-name-setter`，允许自定义构建名。

例1：移动端 `#编号 - 选项(Git分支名) by 构建人` 格式

自动化测试job往往设了参数，一个job对应多套环境/多种情况    
原生的构建名只有 #1、#2……之类，无法直观的看到每次构建用了什么参数

【配置】

job设置页，`Build Environment -> Set Build Name`

例：

`#${BUILD_NUMBER} - ${ENV,var="BUILD_TYPE"}(${GIT_BRANCH}) by ${ENV,var="BUILD_USER"}`

输出：

高级选项里默认选中构建前后都改名，只保留构建后就行。  
（构建前就改的话，左下进度栏显示不下挤变形不好看）

PS：`Build`里也有`Update build name`

#### Build 

![Img](http://chuantu.biz/t6/331/1529635137x-1404764331.png)

- 勾选 Invoke Gradle
- Execute shell
  shell是构建完成后的脚本，将apk和mapping文件复制到一个新目录中以做归档使用，每次判断归档前2min与上一次文件有没有移动，移动过则删除。
  根据情况可以酌情参考或更改。
  
```shell
   # delete old apk (before 1 min) in sub folders
  test $? -eq 0 && find ${WORKSPACE}/app/build/outputs/apk -mindepth 1 -maxdepth 3 -type f -mmin +2 -exec rm -f {} \;

  # move mapping.txt to archive dir
  archive_dir=${WORKSPACE}/app/build/outputs/archive
  test -d ${archive_dir} && rm -rf "${archive_dir}"

  mkdir -p ${archive_dir}
  build_dir=$(echo ${ENVIRONMENT} | tr '[:upper:]' '[:lower:]')  # to lower case
  cp ${WORKSPACE}/app/build/outputs/apk/${build_dir}/*.apk ${archive_dir}

  debugMode="debug"
  if [ ${build_dir} != $debugMode ]
  then
     cp ${WORKSPACE}/app/build/outputs/mapping/${build_dir}/mapping.txt ${archive_dir}
  fi
```

### Post-build Actions 构建后动作
![Img](http://chuantu.biz/t6/331/1529636795x-1404764331.png)

这里主要是对APK和Mapping文件进行归档：

app/build/outputs/archive/**/*.apk,app/build/outputs/archive/**/mapping.txt






### Global roles
### [Role-based Authorization Strategy]()
按项目授权

首先任何人都必须有Overall - Read  

- 最小必要权限：能不给就不给，能在项目设置的就不给全局
- 权限跟职位分离：方便看人看需要给权限

参考角色：

- admin
    - 全部
- jenkins_manager（不怕手滑的admin）
    - Overall: Administer, ConfigureUpdateCenter, Read
    - Credentials：除了Delete外全部
    - Agent(Slave): 除了Delete外全部
    - Job：除了Delete外全部
    - Run：除了Delete外全部
    - View：除了Delete外全部
    - SCM：全部
- job_manager
    - Overall: Read
    - Agent: Build, Connect
    - Job：除了Delete外全部
    - Run: 除了Delete外全部
    - View：除了Delete外全部
- user
    - Overall: Read
    - Agent：Build, Connect
- read_only
    - Overall: Read

#### SDK和Gradle的下载与配置

![Img](http://chuantu.biz/t6/331/1529637911x-1566657621.png)

SDK下载完成在系统设置里配置环境变量**Global properties**选项里勾选
Environment variables配置SDK的环境变量。

![Img](http://chuantu.biz/t6/331/1529637911x-1566657621.png)

### Global Tool Configuration

**JDK installations** JDK配置环境变量

**Gradle installation** 配置环境变量

### Support or Contact

任何问题可以投递我邮箱flycai016@gmail.com或建立issues。

相关文献参考:

[How to install "Open JDK" (Java developement kit) in Ubuntu (Linux)?](https://stackoverflow.com/questions/14788345/how-to-install-the-jdk-on-ubuntu-linux)

[How To Install Jenkins on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-16-04)



