## 1. 设置 volumes 存储路径

这一步可以省略,如果省略,启动时需设置正确路径
Linux 用户：

```js
export GITLAB_HOME=/srv/gitlab
```

`$GITLAB_HOME`为环境变量名称，可以使用此名称指代路径

## 2.

### https 自签名证书

> sudo openssl req -new -x509 -days 36500 -nodes -out config/nginx.pem \

         -keyout config/nginx.key -subj "/C=US/CN=gitlab/O=gitlab.com"

#nginx['redirect_http_to_https'] = true
#letsencrypt['enable'] = false
#nginx['ssl_certificate'] = "/etc/gitlab/nginx.pem"
#nginx['ssl_certificate_key'] = "/etc/gitlab/nginx.key"

### 查看初始密码

> docker exec -it $(docker ps | grep gitlab | awk '{print $1}') grep 'Password:' /etc/gitlab/initial_root_password
