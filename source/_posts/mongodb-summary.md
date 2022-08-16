title: mongodb 总结

date: 2022-08-15 17:00:42

categories:
- 数据库

tags:
- database
- mongodb
---

## 本地安装

### Downlaod and Install

```bash
mongodb-org-server_5.0.7_rc0_amd64.deb
mongodb-org-shell_5.0.7_rc0_amd64.deb
```

### Setup
Init mongodb server:
```bash
sudo mkdir -p /var/lib/mongo
sudo mkdir -p /var/log/mongodb
sudo chown `whoami` /var/lib/mongo
sudo chown `whoami` /var/log/mongodb
```

Start mongodb server:
```bash
mongod --bind_ip_all --dbpath /var/lib/mongo --logpath /var/log/mongodb/mongod.log --fork
```

Note：如果 web server 在同一台机器上，启动的时候不要 --bind_ip_all，默认只允许本地访问。

Stop mongodb server:
```mongodb
> use admin
> db.shutdownServer();
```

## Docker

```bash
docker run -d \
    -p 27017:27017 \
    --name test-mongo \
    -v data-vol:/data/db \
    mongo:latest
```

```bash
docker exec -it <CONTAINER_NAME> bash
```

```bash
mongo
```

```mongo shell
> help
```

参考：https://earthly.dev/blog/mongodb-docker/

