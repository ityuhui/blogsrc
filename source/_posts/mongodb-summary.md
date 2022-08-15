title: mongodb 总结

date: 2022-08-15 17:00:42

categories:
- 数据库

tags:
- database
- mongodb
---

## Downlaod and Install

```bash
mongodb-org-server_5.0.7_rc0_amd64.deb
mongodb-org-shell_5.0.7_rc0_amd64.deb
```

## Setup
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

Stop mongodb server:
```mongodb
> use admin
> db.shutdownServer();
```

