# Docker

## 运行



```
// 停止Docker
sudo service docker stop

// 启动Docker
systemctl start docker.service

// 重启Docker
systemctl daemon-reload
systemctl restart docker

// 查询Docker运行状态
systemctl status docker.service

// 配置文件位置
systemctl show --property=FragmentPath docker 

// 开启remote
# vim /usr/lib/systemd/system/docker.service
[Service]
// ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock

// remote测试
docker -H localhost:5678 version

// 常见端口
2375：未加密的docker socket,远程root无密码访问主机
2376：tls加密套接字,很可能这是您的CI服务器4243端口作为https 443端口的修改
2377：群集模式套接字,适用于群集管理器,不适用于docker客户端
5000：docker注册服务
4789和7946：覆盖网络


// netease cloud api
docker pull binaryify/netease_cloud_music_api

docker run -d -p 3000:3000 --name netease_cloud_music_api    binaryify/netease_cloud_music_api

 Ensure that you have started the Portainer container with the following Docker flag:

-v "/var/run/docker.sock:/var/run/docker.sock" (Linux).

or

-v \\.\pipe\docker_engine:\\.\pipe\docker_engine (Windows).

// 进入alpine
docker run -v /var/run/docker.sock:/var/run/docker.sock -ti alpine sh
```















## UI

### DockerUI

#### Quickstart

##### Step 1

Pull the latest image:

```
docker pull abh1nav/dockerui:latest
```

##### Step 2

(Recommand) If you're running Docker using a unix socket (default):

```
docker run -d -p 9000:9000 -v /var/run/docker.sock:/docker.sock \
--name dockerui abh1nav/dockerui:latest -e="/docker.sock"
```

If you're running Docker over tcp:

```
docker run -d -p 9000:9000 --name dockerui \
abh1nav/dockerui:latest -e="http://<dockerd host ip>:4243"
```

##### Step 3

Open your browser to `http://<dockerd host ip>:9000`



## Portainer

```
docker pull portainer/portainer 
docker run -d --name portainerUI -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer
http://192.168.2.119:9000
```