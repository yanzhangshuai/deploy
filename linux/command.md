- 查看 Linux 版本
  > lsb_release -a lsb(Linux Standard Base)

> cat /etc/\*release\* 查看 linux 版本文件

<hr/>

## 环境变量

在基于 Linux 和 Unix 的系统中，环境变量是一组动态命名的值，存储在系统中，在 shell 或子 shell 中启动的应用程序中使用。简单来说，环境变量是指代路径的变量

### 环境变量和 shell 变量

变量可以分为两大类，环境变量和 shell 变量。
环境变量是系统范围内可用的变量，由生成的子进程和 shell 继承。
Shell 变量是仅适用于当前 shell 实例的变量。每个 shell 如 zsh 和 bash，都有自己的一组内部 shell 变量。

<br/>

- 设置环境变量

> export [envName] = [envValue]

<br/>

- 打印指定环境变量，如果未指定参数，则会打印所有环境变量列，此命令可以指定多个参数

> printenv [...envName]

<br/>

- 删除 shell 和环境变量
  > unset [envName]

<hr/>

## 服务管理

<br/>

- 启用服务
  > service [serviceName] start

<br/>

- 停止服务
  > service [serviceName] stop

<br/>

- 重启服务
  > service [serviceName] restart

<br/>

- 查看服务状态
  > service [serviceName] status

<hr/>

## apt

apt 是 Debian Linux 发行版中的 APT 软件包管理工具。所有基于 Debian 的发行都使用这个包管理系统。deb 包可以把一个应用的文件包在一起，大体就如同 Windows 上的安装文件。

<br/>

- 安装软件包
  > apt install [package]

<br/>

- 卸载软件包，保留软件包配置文件
  > apt remove [package]

<br/>

- 卸载软件包 清除软件包配置文件
  > apt purge [package]

<br/>

- 更新软件包列表
  > apt update

<br/>

- 更新已安装的软件包

> apt upgrade [package]

<br/>

- 自动删除不需要的软件包
  > apt autoremove

<hr/>

## apt-mark

**apt-mark** 用来设置、显示、取消软件包的各种设置

> apt-mark [options] [package]

options:

- auto 标记软件包为自动安装，当没有手动安装的软件包依赖此包时，将导致该包被删除
- manual 标记软件包为手动安装，当没有手动安装的软件包依赖此包时，将防止该包被删除
- minimize-manual
- hold 标记软件包为保留(held back)，阻止软件自动更新
- unhold 取消软件包的保留(held back)标记，解除阻止自动更新
- showauto 列出自动安装的软件包，如果没有指定软件包，则会列出所有的软件包
- showmanual 列出所有手动安装的软件包
- showhold 列出设为保留的软件包

## PS

该能够给出当前系统中进程的快照。它能捕获系统在某一事件的进程状态。

查看进程

> ps [pid]

查看所有进程

> ps -ax BSD 语法

> ps -elf 以标准格式显示进程信息

进程数量

> ps -e | wc -l

> ps axu | wc -l BSD 语法

创建新用户并授权

> adduser newuser

> passwd newuser

> usermod -aG sudo newuser
