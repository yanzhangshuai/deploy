
##  概念

**Dockerfile** 是一个文本文件，其内包含了一条条 **指令(instruction)**，每一条指令构建一层，因此每一条指令的内容，就是描述该层应当如何构建。



#### 构建上下文

**Docker** 在运行时分为 **Docker daemon(Docker 引擎)** 和客户端工具。 **Docker 引擎** 提供了一组 `REST API`，被称为 **Docker Remote API**，而如 `docker` 命令这样的客户端工具，则是通过 这组API 与 **Docker daemon** 交互，从而完成各种功能。
    因此，虽然表面上好像是在本机执行的各种功能，但实际上，一切都是使用远程调用形式在 **服务端（Docker daemon）** 完成。也因为这种 **C/S** 设计，使得操作远程服务器的 **Docker daemon** 变得轻而易举
    当构建镜像时，经常会需要将一些本地文件复制进镜像。比如通过 `COPY`、`ADD` 等指令。而 `docker build` 命令构建镜像却是在远程 **服务端(Docker daemon)**，为了能让 **服务端(Docker daemon)** 获得本地文件，因此引入了 **上下文(Context)** 的概念
    当构建镜像的时候，用户会指定构建镜像上下文的路径， `docker build` 命令得知这个路径后，会将路径下的所有内容打包，然后上传给 **服务端(Docker daemon)**，这样 **Docker daemon** 收到上下文包后，展开就会获得构建镜像所需的一切文件。


例如：
> COPY ./package.json /app/
这个并不是复制执行 `docker build`命令所在的目录下的`package.json`，也不是复制 `Dockerfile` 所在目录下的 `package.json`，而是复制 **上下文(Context)** 目录下的 `package.json`
因此 `COPY` 这类命令中源文件路径都是 *相对路径*


> `.dockerignore`文件用来剔除不需要传递给 **Docker daemon** 的文件

> 默认情况下，如果不额外指定 `Dockerfile` ，会将上下文目录的名为 *Dockerfile* 文件作为 **Dockerfile**



### 指令

* FROM：    指定**基础镜像** ， 根据基础镜像去定制镜像。  `scratch` 一个空白的基础镜像

<br/>

* RUN：     执行命令行命令。 此命令具有两种格式
    1. shell：  此格式就像命令行输入的命令一样。
    2. exec：   `RUN ["可执行文件", "参数1", "参数2"]`，这更像是函数调用中的格式。

<br/>

* COPY：    将从构建上下文目录中 *<源路径>* 的文件/目录复制到镜像内部新一层的 *<目标路径>*
                *<源路径>*：可以为多个，也可以为通配符，通配符规则要满足 **Go** 的 `filepath.Match` 规则。
                *<目标路径>*：可以是容器内的绝对路径，也可以是相对于工作目录的相对路径（工作目录可以用 `WORKDIR` 指定），目标路径不需要事先创建，如果目标不存在会在复制文件前先行创建缺失目录。
            此命令，源文件的各种元数据都会保留。比如读、写、执行权限、文件变更时间等。这个特性对于镜像定制很有用。特别是构建相关文件都在使用 Git 进行管理的时候。
            `--chown=<user>:<group>` 选项用来改变文件的所属用户及所属组。

    1. COPY [--chown=<user>:<group>] <源路径>... <目标路径> ：
    2. COPY [--chown=<user>:<group>] ["<源路径1>",... "<目标路径>"]

<br/>

* ADD：   此指令与 `COPY` 功能类似，只是增加了一些功能 [ADD 更高级的复制文件](https://yeasy.gitbook.io/docker_practice/image/dockerfile/add)

<br/>

* CMD：     指定默认容器主进程的启动命令。 此命令具有两种格式，该命令只允许出现一次，如果写了多个，只有最后一个生效。
                1.shell：   CMD命令
                2.exec：    CMD ["可执行文件", "参数1", "参数2"...]，这类格式解析时会被解析为JSON数组，所有需要使用双引号，推荐使用此格式
            
<br/>

* ENTRYPOINT：  此命令功能与 `CMD` 一样，都是指定容器启动程序及参数。 该命令只允许出现一次，如果写了多个，只有最后一个生效。
                    当指定了 `ENTRYPOINT` 后， `CMD`的含义就会改变，不再是执行的运行其命令，而是将 `CMD` 的内容作为参数传给 `ENTRYPOINT`指令，
                    > <ENTRYPOINT> "<CMD>"
                此命令具有 shell 和 exec 两种格式

<br/>

* ENV：         设置环境变量，后面层的其它指令和运行时的应用都可以使用定义的环境变量

<br/>

* ARG：         设置环境变量，此命令设置的库变量在容器运行时是不存在的。并且此指令受到 `FROM` 影响，再 `FROM`指令之后环境变量会消失。

<br/>

* VOLUME：      定义匿名数据卷

<br/>

* EXPOSE：      声明容器运行时提供服务的端口。这只是一个声明，在容器运行时并不会因为这个声明应用就会开启这个端口的服务

<br/>

* WORKDIR：     指定工作目录（当前目录），以后各层的当前目录就被改为指定的目录，如该目录不存在，`WORKDIR` 会帮你建立目录。

<br/>

* USER：        切换到指定用户，此命令会影响以后的层。此命令只是帮助切换到指定用户而已，这个用户必须是事先建立好的，否则无法切换。

<br/>

* HEALTHCHECK： 设置检查容器健康状态， 该命令只允许出现一次，如果写了多个，只有最后一个生效。

<br/>

* ONBUILD：     添加一个触发器，该命令参数是任意一个 dockerfile 命令，此命令并不会在此 镜像 中执行，而是执行在以 此镜像构建的新镜像中执行。
                    当在一个Dockerfile文件中加上ONBUILD指令，该指令对利用该Dockerfile构建镜像（比如为A镜像）不会产生实质性影响。     
                    但是当我们编写一个新的Dockerfile文件来基于A镜像构建一个镜像（比如为B镜像）时，这时构造A镜像的Dockerfile文件中的ONBUILD指令就生效了，在构建B镜像的过程中，首先会执行ONBUILD指令指定的指令，然后才会执行其它指令。

<br/>

* LABEL：       给镜像以键值对的形式添加一些元数据（metadata）。

<br/>

* SHELL：       此指令可以指定 `RUN`、`ENTRYPOINT`、`CMD` 指令的 shell，Linux 中默认为 ["/bin/sh", "-c"]。在windows中可以修改 cmd和 powershell

## Build

`docker build` 命令依据 **DockerFile** 和上下文构建镜像。构建的上下文是指定 **PATH** 或 **URL** 的一组文件，构建过程中可以引用上下文中的任何文件。

**URL** 参数可以引用三种资源
1. GIT存储库
系统会递归获取存储库及子模块,随后存储库首先被拉到本地主机上的临时目录,然后,此目录作为上下文被发送到Docker daemon 

2. 给定的压缩包
URL直接发送给 Docker daemon，下载操作将在 Docker daemon 所运行的主机上执行， Docker daemon将获取文件并且使用它作为构建上下文

3. Text file


>  docker build [OPTIONS] PATH | URL | -
options：
* --add-host：              添加自定义主机与IP的映射
* 



* [docker build](https://docs.docker.com/engine/reference/commandline/build/)

* [使用 Dockerfile 定制镜像](https://yeasy.gitbook.io/docker_practice/image/build)