# sonarqube

命令

```shell
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube:7.7-community
docker cp sonarqube:/opt/sonarqube/conf /data/sonar
docker cp sonarqube:/opt/sonarqube/extensions /data/sonar
```

## docker-compose 方式

```shell
docker-compose up -d
```

参考：
- [dockerhub-sonarqube](https://hub.docker.com/_/sonarqube/)