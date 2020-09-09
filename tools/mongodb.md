# Mongodb

## 运行



```
# yum 配置
# /etc/yum.repos.d/mongodb-org-4.2.repo
[mongodb-org-4.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.2.asc

# 安装
sudo yum install -y mongodb-org

systemctl status mongod　　# 查看mongod状态
systemctl status mongod.service　　# 查看mongod状态
systemctl start mongod.service　　# 启动
systemctl stop mongod.service 　　# 停止
systemctl enable mongod.service 　　# 自启
```








