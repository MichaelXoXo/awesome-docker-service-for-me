# sonarqube

命令

```shell
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube:7.7-community
docker cp sonarqube:/opt/sonarqube/conf /data/sonar
docker cp sonarqube:/opt/sonarqube/extensions /data/sonar
wget -P /data/sonar/extensions/plugins https://github.com/SonarQubeCommunity/sonar-l10n-zh/releases/download/sonar-l10n-zh-plugin-1.27/sonar-l10n-zh-plugin-1.27.jar
docker stop sonarqube
docker rm sonarqube
```

```shell
docker run -d --name sonarqube \
    -p 9000:9000 \
    -e sonar.jdbc.username=root \
    -e sonar.jdbc.password=123456 \
    -e sonar.jdbc.url="jdbc:mysql://172.20.0.2:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance&useSSL=false" \
    -v /data/sonar/extensions:/opt/sonarqube/extensions \
    sonarqube:7.7-community
```

## docker-compose 方式

```shell
docker-compose up -d
```

参考：
- [dockerhub-sonarqube](https://hub.docker.com/_/sonarqube/)