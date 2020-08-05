title: Keycloak

date: 2020-08-05 22:40:53

categories:
- kubernetes

tags:
- Keycloak
- oidc
---

## 介绍

KeyCloak是Redhat开发的SSO服务程序。可以提供OpenID Connect服务。

<!-- more -->

## 安装

从官网下载压缩包，解压缩

## 运行

```sh
bin/standalone.sh
```

## 配置

只能在本机访问GUI，不能通过网络远程访问

### 登录

```
http://localhost:8080
```

### 创建管理员账户和密码

### 创建realm

### 配置SSL访问

#### 创建CA和证书

使用openssl工具，创建自签名的根证书和证书

```bash
#!/bin/bash

mkdir -p ssl

cat << EOF > ssl/ca.cnf
[req]
req_extensions = v3_req
distinguished_name = req_distinguished_name

[req_distinguished_name]

[ v3_req ]
basicConstraints = CA:TRUE
EOF

cat << EOF > ssl/req.cnf
[req]
req_extensions = v3_req
distinguished_name = req_distinguished_name

[req_distinguished_name]

[ v3_req ]
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName = @alt_names

[alt_names]
IP.1 = 1.2.3.4
EOF

openssl genrsa -out ssl/ca-key.pem 2048
openssl req -x509 -new -nodes -key ssl/ca-key.pem -days 365 -out ssl/ca.pem -subj "//CN=keycloak-ca" -extensions v3_req -config ssl/ca.cnf

openssl genrsa -out ssl/keycloak.pem 2048
openssl req -new -key ssl/keycloak.pem -out ssl/keycloak-csr.pem -subj "//CN=keycloak" -config ssl/req.cnf
openssl x509 -req -in ssl/keycloak-csr.pem -CA ssl/ca.pem -CAkey ssl/ca-key.pem -CAcreateserial -out ssl/keycloak.crt -days 365 -extensions v3_req -extfile ssl/req.cnf

```

#### 生成keystore

因为Keycloak是Java开发的，所以只能接受Java keystore (jks)的密钥对。

```bash
openssl pkcs12 -export -out keycloak.p12 -inkey keycloak.pem -in keycloak.crt -certfile ca.pem

keytool -importkeystore -deststorepass 'passw0rd' -destkeystore keycloak.jks -srckeystore keycloak.p12 -srcstoretype PKCS12
```

#### 配置Keycloak使用SSL

使用Keycloak CLI:

```bash
bin\jboss-cli.bat

[disconnected /] connect

[standalone@localhost:9990 /] /core-service=management/security-realm=UndertowRealm:add()
{"outcome" => "success"}

[standalone@localhost:9990 /] /core-service=management/security-realm=UndertowRealm/server-identity=ssl:add(keystore-path=keycloak.jks, keystore-relative-to=jboss.server.config.dir, keystore-password=passw0rd)
{
    "outcome" => "success",
    "response-headers" => {
        "operation-requires-reload" => true,
        "process-state" => "reload-required"
    }
}

[standalone@localhost:9990 /] /subsystem=undertow/server=default-server/https-listener=https:write-attribute(name=security-realm, value=UndertowRealm)
{
    "outcome" => "success",
    "response-headers" => {
        "operation-requires-reload" => true,
        "process-state" => "reload-required"
    }
}

```

#### 重新启动 Keycloak

访问
https://ip:8443

可以看到关于https证书的警告。代表配置成功。

接下来，就可以配置Keycloak给kubernetes提供OIDC认证服务了。
