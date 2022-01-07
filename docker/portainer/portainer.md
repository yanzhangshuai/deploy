### 安装

> docker run -d -p 9000:9000 --restart=always -v /srv/portainer/run/docker.sock:/var/run/docker.sock --name prtainer portainer/portainer

### 编辑配置文件内容，接收所有 ip 请求

> sudo vi /lib/systemd/system/docker.service
> ExecStart=/usr/bin/dockerd -H unix:///var/run/docker.sock -H tcp://0.0.0.0:2375

### 重新加载配置文件，重启 docker daemon

> sudo systemctl daemon-reload
> sudo systemctl restart docker
