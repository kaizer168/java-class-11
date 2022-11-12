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

## 启动集群脚本  
[root@mongodb-0 ~]# tee /root/mongocluster/start-mongo-cluster.sh <<-'EOF'  
> #! /bin/bash  
> clear  
> /usr/local/hero/mongodb-linux-x86_64-rhel70-4.1.3/bin/mongod -f /root/mongocluster/mongo_37017.conf  
> /usr/local/hero/mongodb-linux-x86_64-rhel70-4.1.3/bin/mongod -f /root/mongocluster/mongo_37018.conf  
> /usr/local/hero/mongodb-linux-x86_64-rhel70-4.1.3/bin/mongod -f /root/mongocluster/mongo_37019.conf  
> echo "start mongo cluster..."  
> ps -ef | grep mongodb  
> EOF  
#! /bin/bash  
clear  
/usr/local/hero/mongodb-linux-x86_64-rhel70-4.1.3/bin/mongod -f /root/mongocluster/mongo_37017.conf  
/usr/local/hero/mongodb-linux-x86_64-rhel70-4.1.3/bin/mongod -f /root/mongocluster/mongo_37018.conf  
/usr/local/hero/mongodb-linux-x86_64-rhel70-4.1.3/bin/mongod -f /root/mongocluster/mongo_37019.conf  
echo "start mongo cluster..."  
ps -ef | grep mongodb  
[root@mongodb-0 ~]#  
[root@mongodb-0 ~]# chmod 755 /root/mongocluster/start-mongo-cluster.sh  
[root@mongodb-0 ~]#   

## 关闭集群脚本  
[root@mongodb-0 ~]# tee /root/mongocluster/stop-mongo-cluster.sh <<-'EOF'  
> #! /bin/bash  
> clear  
> /usr/local/hero/mongodb-linux-x86_64-rhel70-4.1.3/bin/mongod --shutdown -f /root/mongocluster/mongo_37017.conf  
> /usr/local/hero/mongodb-linux-x86_64-rhel70-4.1.3/bin/mongod --shutdown -f /root/mongocluster/mongo_37018.conf  
> /usr/local/hero/mongodb-linux-x86_64-rhel70-4.1.3/bin/mongod --shutdown -f /root/mongocluster/mongo_37019.conf  
> echo "stop mongo cluster..."  
> ps -ef | grep mongodb  
> EOF  
#! /bin/bash  
clear  
/usr/local/hero/mongodb-linux-x86_64-rhel70-4.1.3/bin/mongod --shutdown -f /root/mongocluster/mongo_37017.conf  
/usr/local/hero/mongodb-linux-x86_64-rhel70-4.1.3/bin/mongod --shutdown -f /root/mongocluster/mongo_37018.conf  
/usr/local/hero/mongodb-linux-x86_64-rhel70-4.1.3/bin/mongod --shutdown -f /root/mongocluster/mongo_37019.conf  
echo "stop mongo cluster..."  
ps -ef | grep mongodb  
[root@mongodb-0 ~]#  
[root@mongodb-0 ~]# chmod 755 /root/mongocluster/stop-mongo-cluster.sh  
[root@mongodb-0 ~]# cd mongocluster 

## 启动集群  
[root@mongodb-0 mongocluster]# ./start-mongo-cluster.sh  
2022-11-12T18:12:38.131+0800 I CONTROL  [main] Automatically disabling TLS 1.0, to force-enable TLS 1.0 specify --sslDisabledProtocols 'none'  
about to fork child process, waiting until server is ready for connections.  
forked process: 51345  
child process started successfully, parent exiting  
2022-11-12T18:12:39.895+0800 I CONTROL  [main] Automatically disabling TLS 1.0, to force-enable TLS 1.0 specify --sslDisabledProtocols 'none'  
about to fork child process, waiting until server is ready for connections.  
forked process: 51390  
child process started successfully, parent exiting  
2022-11-12T18:12:42.786+0800 I CONTROL  [main] Automatically disabling TLS 1.0, to force-enable TLS 1.0 specify --sslDisabledProtocols 'none'  
about to fork child process, waiting until server is ready for connections.  
forked process: 51470  
child process started successfully, parent exiting  
start mongo cluster...  
root      51345      1 21 18:12 ?        00:00:01 /usr/local/hero/mongodb-linux-x86_64-rhel70-4.1.3/bin/mongod -f /root/mongocluster/mongo_37017.conf  
root      51390      1 45 18:12 ?        00:00:02 /usr/local/hero/mongodb-linux-x86_64-rhel70-4.1.3/bin/mongod -f /root/mongocluster/mongo_37018.conf  
root      51470      1 97 18:12 ?        00:00:02 /usr/local/hero/mongodb-linux-x86_64-rhel70-4.1.3/bin/mongod -f /root/mongocluster/mongo_37019.conf  
root      51537  51337  0 18:12 pts/0    00:00:00 grep mongodb  
[root@mongodb-0 mongocluster]#  

## 初始化集群  
[root@mongodb-0 mongocluster]# mongo --host=192.168.1.130 --port=37017  
MongoDB shell version v4.1.3  
connecting to: mongodb://192.168.1.130:37017/  
Implicit session: session { "id" : UUID("957796f5-af98-4063-9231-201104a6108e") }  
MongoDB server version: 4.1.3  
Server has startup warnings:  
2022-11-12T18:12:39.790+0800 I CONTROL  [initandlisten]  
2022-11-12T18:12:39.790+0800 I CONTROL  [initandlisten] ** NOTE: This is a development version (4.1.3) of MongoDB.  
2022-11-12T18:12:39.790+0800 I CONTROL  [initandlisten] **       Not recommended for production.  
2022-11-12T18:12:39.790+0800 I CONTROL  [initandlisten]  
2022-11-12T18:12:39.790+0800 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.  
2022-11-12T18:12:39.790+0800 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.  
2022-11-12T18:12:39.790+0800 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.  
2022-11-12T18:12:39.790+0800 I CONTROL  [initandlisten]  
2022-11-12T18:12:39.790+0800 I CONTROL  [initandlisten]  
2022-11-12T18:12:39.790+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.  
2022-11-12T18:12:39.790+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'  
2022-11-12T18:12:39.790+0800 I CONTROL  [initandlisten]  
2022-11-12T18:12:39.790+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.  
2022-11-12T18:12:39.790+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'  
2022-11-12T18:12:39.790+0800 I CONTROL  [initandlisten]  
\---
Enable MongoDB's free cloud-based monitoring service, which will then receive and display  
metrics about your deployment (disk utilization, CPU, operation statistics, etc).  

The monitoring data will be available on a MongoDB website with a unique URL accessible to you  
and anyone you share the URL with. MongoDB may use this information to make product  
improvements and to suggest MongoDB products and deployment options to you.  

To enable free monitoring, run the following command: db.enableFreeMonitoring()  
To permanently disable this reminder, run the following command: db.disableFreeMonitoring()  
\---  

\>  
\> var cfg ={"_id":"heroMongoCluster",  
... "protocolVersion" : 1,  
... "members":[  
... {"_id":1,"host":"192.168.1.130:37017","priority":10},  
... {"_id":2,"host":"192.168.1.130:37018"}  
... ]  
... }  
\> rs.initiate(cfg)  
{  
        "ok" : 1,  
        "$clusterTime" : {  
                "clusterTime" : Timestamp(1668248698, 1),  
                "signature" : {  
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),  
                        "keyId" : NumberLong(0)  
                }  
        },  
        "operationTime" : Timestamp(1668248698, 1)  
}  
heroMongoCluster:SECONDARY> rs.status()  
{  
        "set" : "heroMongoCluster",  
        "date" : ISODate("2022-11-12T10:25:44.356Z"),  
        "myState" : 1,  
        "term" : NumberLong(1),  
        "syncingTo" : "",  
        "syncSourceHost" : "",  
        "syncSourceId" : -1,  
        "heartbeatIntervalMillis" : NumberLong(2000),  
        "optimes" : {  
                "lastCommittedOpTime" : {  
                        "ts" : Timestamp(1668248740, 1),  
                        "t" : NumberLong(1)  
                },  
                "readConcernMajorityOpTime" : {  
                        "ts" : Timestamp(1668248740, 1),  
                        "t" : NumberLong(1)  
                },
                "appliedOpTime" : {  
                        "ts" : Timestamp(1668248740, 1),  
                        "t" : NumberLong(1)  
                },  
                "durableOpTime" : {  
                        "ts" : Timestamp(1668248740, 1),  
                        "t" : NumberLong(1)  
                }  
        },  
        "lastStableRecoveryTimestamp" : Timestamp(1668248710, 1),  
        "lastStableCheckpointTimestamp" : Timestamp(1668248710, 1),  
        "members" : [  
                {  
                        "_id" : 1,  
                        "name" : "192.168.1.130:37017",  
                        "health" : 1,  
                        "state" : 1,  
                        "stateStr" : "PRIMARY",  
                        "uptime" : 786,  
                        "optime" : {  
                                "ts" : Timestamp(1668248740, 1),  
                                "t" : NumberLong(1)  
                        },  
                        "optimeDate" : ISODate("2022-11-12T10:25:40Z"),  
                        "syncingTo" : "",  
                        "syncSourceHost" : "",  
                        "syncSourceId" : -1,  
                        "infoMessage" : "could not find member to sync from",  
                        "electionTime" : Timestamp(1668248709, 1),  
                        "electionDate" : ISODate("2022-11-12T10:25:09Z"),  
                        "configVersion" : 1,  
                        "self" : true,  
                        "lastHeartbeatMessage" : ""  
                },  
                {  
                        "_id" : 2,  
                        "name" : "192.168.1.130:37018",  
                        "health" : 1,  
                        "state" : 2,  
                        "stateStr" : "SECONDARY",  
                        "uptime" : 45,  
                        "optime" : {  
                                "ts" : Timestamp(1668248740, 1),  
                                "t" : NumberLong(1)  
                        },  
                        "optimeDurable" : {  
                                "ts" : Timestamp(1668248740, 1),  
                                "t" : NumberLong(1)  
                        },  
                        "optimeDate" : ISODate("2022-11-12T10:25:40Z"),  
                        "optimeDurableDate" : ISODate("2022-11-12T10:25:40Z"),  
                        "lastHeartbeat" : ISODate("2022-11-12T10:25:43.181Z"),  
                        "lastHeartbeatRecv" : ISODate("2022-11-12T10:25:43.881Z"),  
                        "pingMs" : NumberLong(0),  
                        "lastHeartbeatMessage" : "",  
                        "syncingTo" : "192.168.1.130:37017",  
                        "syncSourceHost" : "192.168.1.130:37017",  
                        "syncSourceId" : 1,  
                        "infoMessage" : "",  
                        "configVersion" : 1  
                }  
        ],  
        "ok" : 1,  
        "$clusterTime" : {  
                "clusterTime" : Timestamp(1668248740, 1),  
                "signature" : {  
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),  
                        "keyId" : NumberLong(0)  
                }  
        },  
        "operationTime" : Timestamp(1668248740, 1)  
}  
heroMongoCluster:PRIMARY>  
heroMongoCluster:PRIMARY> rs.status()  
{  
        "set" : "heroMongoCluster",  
        "date" : ISODate("2022-11-12T10:37:44.858Z"),  
        "myState" : 1,  
        "term" : NumberLong(1),  
        "syncingTo" : "",  
        "syncSourceHost" : "",  
        "syncSourceId" : -1,  
        "heartbeatIntervalMillis" : NumberLong(2000),  
        "optimes" : {  
                "lastCommittedOpTime" : {  
                        "ts" : Timestamp(1668249461, 1),  
                        "t" : NumberLong(1)  
                },  
                "readConcernMajorityOpTime" : {  
                        "ts" : Timestamp(1668249461, 1),  
                        "t" : NumberLong(1)  
                },  
                "appliedOpTime" : {  
                        "ts" : Timestamp(1668249461, 1),  
                        "t" : NumberLong(1)  
                },  
                "durableOpTime" : {  
                        "ts" : Timestamp(1668249461, 1),  
                        "t" : NumberLong(1)  
                }  
        },  
        "lastStableRecoveryTimestamp" : Timestamp(1668249431, 1),  
        "lastStableCheckpointTimestamp" : Timestamp(1668249431, 1),  
        "members" : [  
                {  
                        "_id" : 1,  
                        "name" : "192.168.1.130:37017",  
                        "health" : 1,  
                        "state" : 1,  
                        "stateStr" : "PRIMARY",  
                        "uptime" : 1506,  
                        "optime" : {  
                                "ts" : Timestamp(1668249461, 1),  
                                "t" : NumberLong(1)  
                        },  
                        "optimeDate" : ISODate("2022-11-12T10:37:41Z"),  
                        "syncingTo" : "",  
                        "syncSourceHost" : "",  
                        "syncSourceId" : -1,  
                        "infoMessage" : "",  
                        "electionTime" : Timestamp(1668248709, 1),  
                        "electionDate" : ISODate("2022-11-12T10:25:09Z"),  
                        "configVersion" : 1,  
                        "self" : true,  
                        "lastHeartbeatMessage" : ""  
                },  
                {  
                        "_id" : 2,  
                        "name" : "192.168.1.130:37018",  
                        "health" : 1,  
                        "state" : 2,  
                        "stateStr" : "SECONDARY",  
                        "uptime" : 766,  
                        "optime" : {  
                                "ts" : Timestamp(1668249461, 1),  
                                "t" : NumberLong(1)  
                        },  
                        "optimeDurable" : {  
                                "ts" : Timestamp(1668249461, 1),  
                                "t" : NumberLong(1)  
                        },  
                        "optimeDate" : ISODate("2022-11-12T10:37:41Z"),  
                        "optimeDurableDate" : ISODate("2022-11-12T10:37:41Z"),  
                        "lastHeartbeat" : ISODate("2022-11-12T10:37:44.064Z"),  
                        "lastHeartbeatRecv" : ISODate("2022-11-12T10:37:44.797Z"),  
                        "pingMs" : NumberLong(0),  
                        "lastHeartbeatMessage" : "",  
                        "syncingTo" : "192.168.1.130:37017",  
                        "syncSourceHost" : "192.168.1.130:37017",  
                        "syncSourceId" : 1,  
                        "infoMessage" : "",  
                        "configVersion" : 1  
                }  
        ],  
        "ok" : 1,  
        "$clusterTime" : {  
                "clusterTime" : Timestamp(1668249461, 1),  
                "signature" : {  
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),  
                        "keyId" : NumberLong(0)  
                }  
        },  
        "operationTime" : Timestamp(1668249461, 1)  
}  
heroMongoCluster:PRIMARY>  
heroMongoCluster:PRIMARY> rs.add("192.168.1.130:37019")  
{  
        "ok" : 1,  
        "$clusterTime" : {  
                "clusterTime" : Timestamp(1668249711, 2),  
                "signature" : {  
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),  
                        "keyId" : NumberLong(0)  
                }  
        },  
        "operationTime" : Timestamp(1668249711, 2)  
}  
heroMongoCluster:PRIMARY>  
heroMongoCluster:PRIMARY> rs.status()  
{  
        "set" : "heroMongoCluster",  
        "date" : ISODate("2022-11-12T10:43:40.182Z"),  
        "myState" : 1,  
        "term" : NumberLong(1),  
        "syncingTo" : "",  
        "syncSourceHost" : "",  
        "syncSourceId" : -1,  
        "heartbeatIntervalMillis" : NumberLong(2000),  
        "optimes" : {  
                "lastCommittedOpTime" : {  
                        "ts" : Timestamp(1668249811, 1),  
                        "t" : NumberLong(1)  
                },  
                "readConcernMajorityOpTime" : {  
                        "ts" : Timestamp(1668249811, 1),  
                        "t" : NumberLong(1)  
                },  
                "appliedOpTime" : {  
                        "ts" : Timestamp(1668249811, 1),  
                        "t" : NumberLong(1)  
                },  
                "durableOpTime" : {  
                        "ts" : Timestamp(1668249811, 1),  
                        "t" : NumberLong(1)  
                }  
        },  
        "lastStableRecoveryTimestamp" : Timestamp(1668249791, 1),  
        "lastStableCheckpointTimestamp" : Timestamp(1668249791, 1),  
        "members" : [  
                {  
                        "_id" : 1,  
                        "name" : "192.168.1.130:37017",  
                        "health" : 1,  
                        "state" : 1,  
                        "stateStr" : "PRIMARY",  
                        "uptime" : 1862,  
                        "optime" : {  
                                "ts" : Timestamp(1668249811, 1),  
                                "t" : NumberLong(1)  
                        },  
                        "optimeDate" : ISODate("2022-11-12T10:43:31Z"),  
                        "syncingTo" : "",  
                        "syncSourceHost" : "",  
                        "syncSourceId" : -1,  
                        "infoMessage" : "",  
                        "electionTime" : Timestamp(1668248709, 1),  
                        "electionDate" : ISODate("2022-11-12T10:25:09Z"),  
                        "configVersion" : 2,   
                        "self" : true,  
                        "lastHeartbeatMessage" : ""  
                },  
                {  
                        "_id" : 2,  
                        "name" : "192.168.1.130:37018",  
                        "health" : 1,  
                        "state" : 2,  
                        "stateStr" : "SECONDARY",  
                        "uptime" : 1121,  
                        "optime" : {  
                                "ts" : Timestamp(1668249811, 1),  
                                "t" : NumberLong(1)  
                        },  
                        "optimeDurable" : {  
                                "ts" : Timestamp(1668249811, 1),  
                                "t" : NumberLong(1)  
                        },  
                        "optimeDate" : ISODate("2022-11-12T10:43:31Z"),  
                        "optimeDurableDate" : ISODate("2022-11-12T10:43:31Z"),  
                        "lastHeartbeat" : ISODate("2022-11-12T10:43:39.869Z"),  
                        "lastHeartbeatRecv" : ISODate("2022-11-12T10:43:38.871Z"),  
                        "pingMs" : NumberLong(0),  
                        "lastHeartbeatMessage" : "",  
                        "syncingTo" : "192.168.1.130:37017",  
                        "syncSourceHost" : "192.168.1.130:37017",  
                        "syncSourceId" : 1,  
                        "infoMessage" : "",  
                        "configVersion" : 2  
                },  
                {  
                        "_id" : 3,  
                        "name" : "192.168.1.130:37019",  
                        "health" : 1,  
                        "state" : 2,  
                        "stateStr" : "SECONDARY",  
                        "uptime" : 108,  
                        "optime" : {  
                                "ts" : Timestamp(1668249811, 1),  
                                "t" : NumberLong(1)  
                        },  
                        "optimeDurable" : {  
                                "ts" : Timestamp(1668249811, 1),  
                                "t" : NumberLong(1)  
                        },  
                        "optimeDate" : ISODate("2022-11-12T10:43:31Z"),  
                        "optimeDurableDate" : ISODate("2022-11-12T10:43:31Z"),  
                        "lastHeartbeat" : ISODate("2022-11-12T10:43:39.868Z"),  
                        "lastHeartbeatRecv" : ISODate("2022-11-12T10:43:38.651Z"),  
                        "pingMs" : NumberLong(0),  
                        "lastHeartbeatMessage" : "",  
                        "syncingTo" : "192.168.1.130:37017",  
                        "syncSourceHost" : "192.168.1.130:37017",  
                        "syncSourceId" : 1,  
                        "infoMessage" : "",  
                        "configVersion" : 2  
                }  
        ],  
        "ok" : 1,  
        "$clusterTime" : {  
                "clusterTime" : Timestamp(1668249811, 1),  
                "signature" : {  
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),  
                        "keyId" : NumberLong(0)  
                }  
        },  
        "operationTime" : Timestamp(1668249811, 1)  
}  
heroMongoCluster:PRIMARY>  

[root@mongodb-0 ~]# mongo --host=192.168.1.130 --port=37018  
MongoDB shell version v4.1.3  
connecting to: mongodb://192.168.1.130:37018/  
Implicit session: session { "id" : UUID("15dd1c07-4927-4da2-bd97-6fd35169383f") }  
MongoDB server version: 4.1.3  
Server has startup warnings:  
2022-11-12T18:12:42.317+0800 I CONTROL  [initandlisten]   
2022-11-12T18:12:42.317+0800 I CONTROL  [initandlisten] ** NOTE: This is a development version (4.1.3) of MongoDB.  
2022-11-12T18:12:42.318+0800 I CONTROL  [initandlisten] **       Not recommended for production.  
2022-11-12T18:12:42.318+0800 I CONTROL  [initandlisten]  
2022-11-12T18:12:42.318+0800 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.  
2022-11-12T18:12:42.319+0800 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.  
2022-11-12T18:12:42.319+0800 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.  
2022-11-12T18:12:42.319+0800 I CONTROL  [initandlisten]  
2022-11-12T18:12:42.319+0800 I CONTROL  [initandlisten]  
2022-11-12T18:12:42.319+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.  
2022-11-12T18:12:42.319+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'  
2022-11-12T18:12:42.319+0800 I CONTROL  [initandlisten]  
2022-11-12T18:12:42.319+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.  
2022-11-12T18:12:42.320+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'  
2022-11-12T18:12:42.320+0800 I CONTROL  [initandlisten]  
\---  
Enable MongoDB's free cloud-based monitoring service, which will then receive and display  
metrics about your deployment (disk utilization, CPU, operation statistics, etc).  

The monitoring data will be available on a MongoDB website with a unique URL accessible to you  
and anyone you share the URL with. MongoDB may use this information to make product  
improvements and to suggest MongoDB products and deployment options to you.  

To enable free monitoring, run the following command: db.enableFreeMonitoring()  
To permanently disable this reminder, run the following command: db.disableFreeMonitoring()  
\---

heroMongoCluster:SECONDARY> rs.slaveOk()  
heroMongoCluster:SECONDARY> exit  

[root@mongodb-0 ~]# mongo --host=192.168.1.130 --port=37019  
MongoDB shell version v4.1.3  
connecting to: mongodb://192.168.1.130:37019/  
Implicit session: session { "id" : UUID("04cc87f8-9b5a-4134-97af-c9a12731d6dc") }  
MongoDB server version: 4.1.3  
Server has startup warnings:  
2022-11-12T18:12:45.548+0800 I CONTROL  [initandlisten]  
2022-11-12T18:12:45.549+0800 I CONTROL  [initandlisten] ** NOTE: This is a development version (4.1.3) of MongoDB.  
2022-11-12T18:12:45.549+0800 I CONTROL  [initandlisten] **       Not recommended for production.  
2022-11-12T18:12:45.549+0800 I CONTROL  [initandlisten]  
2022-11-12T18:12:45.549+0800 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.  
2022-11-12T18:12:45.549+0800 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.  
2022-11-12T18:12:45.549+0800 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.  
2022-11-12T18:12:45.549+0800 I CONTROL  [initandlisten]  
2022-11-12T18:12:45.549+0800 I CONTROL  [initandlisten]  
2022-11-12T18:12:45.549+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.  
2022-11-12T18:12:45.549+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'  
2022-11-12T18:12:45.549+0800 I CONTROL  [initandlisten]  
2022-11-12T18:12:45.550+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.  
2022-11-12T18:12:45.550+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'  
2022-11-12T18:12:45.550+0800 I CONTROL  [initandlisten]  
\---
Enable MongoDB's free cloud-based monitoring service, which will then receive and display  
metrics about your deployment (disk utilization, CPU, operation statistics, etc).  

The monitoring data will be available on a MongoDB website with a unique URL accessible to you  
and anyone you share the URL with. MongoDB may use this information to make product  
improvements and to suggest MongoDB products and deployment options to you.  

To enable free monitoring, run the following command: db.enableFreeMonitoring()  
To permanently disable this reminder, run the following command: db.disableFreeMonitoring()  
\---

heroMongoCluster:SECONDARY> rs.slaveOk()  
heroMongoCluster:SECONDARY>  

