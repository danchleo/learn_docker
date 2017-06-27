
### 创建linux bridge
```
brctl addbr br0
brctl addif br0 enp0s1
brctl setfd br0 0
ifconfig br0 10.0.189.109 netmask 255.255.0.0
```

### 创建br0的配置文件

### 使docker使用br0
#### 方法一
```
docker network create --driver=bridge --ip-range=10.0.190.0/24 --subnet=10.0.0.0/16 --aux-address='ip1=10.0.190.1' --aux-address='ip2=10.0.190.2' --aux-address='ip3=10.0.190.3' -o "com.docker.network.bridge.name=br0" br0
docker run --net=br-admin -it ubuntu-16.04

#另外一种
docker network create --driver=bridge --ip-range=172.16.51.0/16 --subnet=172.16.0.0/16 --gateway=172.16.51.10 -o "com.docker.network.bridge.name=br0" br0
```
#### 方法二
编辑/etc/docker/daemon.json
```
{
    "bridge": "br0",
    "fixed-cidr": "172.16.51.0/24",
    "default-gateway": "172.16.0.1"
}
```
然后重启docker

### 验证
```
brctl show br0
docker inspect container0
```
