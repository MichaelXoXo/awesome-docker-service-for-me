## nginx

使用 Nginx 容器搭建文件服务区

file.conf：

```shell
server {
        client_max_body_size 4G;
        listen  8889;  ## listen for ipv4; this line is default and implied
        server_name <host_ip or domain_name>; # 换成你服务器的 IP 或绑定的域名
        charset utf-8;
        root /data;
        location / {
            autoindex on; # 要想设置nginx的目录浏览功能，必须要打开下面这个参数
            autoindex_exact_size off; # 默认是开，以b为单位，关闭后，会显示MB GB
            autoindex_localtime on;
        }
}
```

docker-compose.yml：

```shell
services:
    nginx:
        image: nginx
        container_name: nginx # 容器名
        ports:
            - "8889:8889"
        volumes:
            - "/data/nginx/conf.d:/etc/nginx/conf.d"
            - "/data/nginx/data:/data"
        restart: always
```

为了避免中文乱码，需要配置 `charset utf-8;`

### 怎么给文件服务器设置访问密码呢？只需要修改一下配置即可：

```shell
server {
        client_max_body_size 4G;
        listen  8889;  ## listen for ipv4; this line is default and implied
        server_name 207.148.104.42;
        charset utf-8;
        root /data;
        location / {
            auth_basic "Restricted";
            auth_basic_user_file /data/pass_file;
            autoindex on;
            autoindex_exact_size off;
            autoindex_localtime on;
        }
}
```

生成用户名和密码：

```shell
htpasswd -c -d pass_file michael
```

- `htpasswd` 命令没有的话，需要：`yum install httpd-tools -y` 安装；
- `-c` 创建一个文件；
- `-d` 强制 CRYPT 加密密码（最多8个字符，不安全）
- `-s` Force SHA encryption of the password (insecure)

参考：[Nginx配置静态文件服务](https://blog.csdn.net/dutsoft/article/details/55004064)

### 控制访问 nginx 的 IP

网段的写法是这样的：`192.168.1.0/24` 这样的形式。

```shell
deny 192.168.1.11;
deny 192.168.1.123;
deny 192.168.1.0/24; # 屏蔽整个192.168.1.* C段IP
```

允许某几个 IP 能访问，其他 IP 均不能访问：

```shell
allow 192.168.1.1;
allow 192.168.1.2;
deny all;
```

示例：

```shell
server {
        client_max_body_size 4G;
        listen  8889;  ## listen for ipv4; this line is default and implied
        server_name 207.148.104.42;
        charset utf-8;
        root /data;
        location / {
            allow 112.10.84.226;
            deny all;
            auth_basic "Restricted";
            auth_basic_user_file /data/pass_file;
            autoindex on;
            autoindex_exact_size off;
            autoindex_localtime on;
        }
}
```

- [[原创]nginx如何禁止指定IP或IP段访问](http://www.21yunwei.com/archives/2540)
- [Nginx基础篇（4）- Nginx请求限制和访问控制](https://segmentfault.com/a/1190000015591117)

## 原文

- [使用容器 Docker 创建开发环境](https://michael728.github.io/2019/06/02/docker-create-develop-environment/)