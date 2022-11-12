# 搭建MongoDB分片集群  
[root@mongodb-0 ~]# mkdir -p /data/mongo/logs  
[root@mongodb-0 ~]# mkdir -p /data/mongo/data/server1  
[root@mongodb-0 ~]# mkdir -p /data/mongo/data/server2  
[root@mongodb-0 ~]# mkdir -p /data/mongo/data/server3  
[root@mongodb-0 ~]# touch /data/mongo/logs/server1.log  
[root@mongodb-0 ~]# touch /data/mongo/logs/server2.log  
[root@mongodb-0 ~]# touch /data/mongo/logs/server3.log  
[root@mongodb-0 ~]# ls -R /data  
/data:  
mongo  

/data/mongo:  
data  logs  

/data/mongo/data:  
server1  server2  server3  

/data/mongo/data/server1:  

/data/mongo/data/server2:  

/data/mongo/data/server3:  

/data/mongo/logs:  
server1.log  server2.log  server3.log  

[root@mongodb-0 ~]# mkdir /root/mongocluster  

## 主节点配置 mongo_37017.conf  
[root@mongodb-0 ~]# tee /root/mongocluster/mongo_37017.conf <<-'EOF'  
> \# 主节点配置  
> dbpath=/data/mongo/data/server1  
> bind_ip=0.0.0.0  
> port=37017  
> fork=true  
> logpath=/data/mongo/logs/server1.log  
> \# 集群名称  
> replSet=heroMongoCluster  
> EOF  
\# 主节点配置  
dbpath=/data/mongo/data/server1  
bind_ip=0.0.0.0  
port=37017  
fork=true  
logpath=/data/mongo/logs/server1.log  
\# 集群名称  
replSet=heroMongoCluster  
[root@mongodb-0 ~]#  

## 从节点1配置 mongo_37018.conf  
[root@mongodb-0 ~]# tee /root/mongocluster/mongo_37018.conf <<-'EOF'  
> dbpath=/data/mongo/data/server2  
> bind_ip=0.0.0.0  
> port=37018  
> fork=true  
> logpath=/data/mongo/logs/server2.log  
> replSet=heroMongoCluster  
> EOF  
dbpath=/data/mongo/data/server2  
bind_ip=0.0.0.0  
port=37018  
fork=true  
logpath=/data/mongo/logs/server2.log  
replSet=heroMongoCluster  
[root@mongodb-0 ~]#  

## 从节点2配置 mongo_37019.conf  
[root@mongodb-0 ~]# tee /root/mongocluster/mongo_37019.conf <<-'EOF'  
> dbpath=/data/mongo/data/server3  
> bind_ip=0.0.0.0  
> port=37019  
> fork=true  
> logpath=/data/mongo/logs/server3.log  
> replSet=heroMongoCluster  
> EOF  
dbpath=/data/mongo/data/server3  
bind_ip=0.0.0.0  
port=37019  
fork=true  
logpath=/data/mongo/logs/server3.log  
replSet=heroMongoCluster  
[root@mongodb-0 ~]#  


