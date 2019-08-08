# 参考
 [参考一](https://www.jianshu.com/p/315da3fd6950)
 [参考二](https://www.jianshu.com/p/d06087ff9bf9)
# 常用命令
- 创建虚拟主机节点 
 docker-machine create 虚拟主机名
- 查看虚拟机节点信息
 docker-machine ls
- 停止虚拟主机节点
 docker-machine stop 虚拟主机名
- 初始化docker swarm集群
docker swarm init --advertise-addr master的IP地址
- 在master上获取加入集群的token
docker swarm join-token worker
- slave节点加入集群
docker swarm join --token [token] [master的IP]:[master的端口]
- slave节点主动离开集群
docker swarm leave
- docker swarm创建service
docker service create --replicas 2 -d -p 8080:80 --name 服务名 镜像名
- 在master查看service信息
docker service ls
docker service ps 你所创建的服务的ID
- 在master上进行服务扩容
docker service scale 你的service name=你要的副本数目

