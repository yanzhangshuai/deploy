### /verdaccio/conf

在挂在 _/verdaccio/conf_ 目录时，需要挂载的目录下提供 _config.yaml_ 文件的副本，否则容器无法正常启动 [config.yaml](https://github.com/verdaccio/verdaccio/blob/5.x/conf/docker.yaml)

### nginx 代理

设置 nginx 代理时需要设置 **VERDACCIO_PUBLIC_URL(访问域名)**、 **url_prefix（访问地址）**




### 查看日志
> docker logs --tail 20 npm-register


### 设置权限
> sudo chown -R 10001:65533 storage