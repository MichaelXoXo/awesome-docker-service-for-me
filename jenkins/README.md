# 使用容器 Docker 创建开发环境

## docker-compose

```shell
# 启动所有服务
docker-compose up -d
# 暂停 jenkins
docker-compose stop devops-jenkins
# 重启 jenkins
docker-compose stop devops-jenkins
# 删除所有容器和镜像
docker-compose down
```

## docker

```
docker run \
  -u root \
  --name devops-jenkins \
  -d \
  -p 8080:8080 \
  -p 50000:50000 \
  -v /data/docker_local/jenkins-data:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkinsci/blueocean:1.15.1
```

更多细节，访问原文：
- [使用容器 Docker 创建开发环境](https://michael728.github.io/2019/06/02/docker-create-develop-environment/)