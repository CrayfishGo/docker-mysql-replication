# docker-mysql-replication
基于docker 的MySQL主从配置


# Running with docker-compose
Step1: 启动容器
```
docker-compose up --build -d
```

Step2: 连接master，并运行以下命令，创建一个用户用来同步数据
```
GRANT REPLICATION SLAVE ON *.* to 'backup'@'%' identified by '123456';
```

Step3: 查看master status, 记住File、Position的值，如果没查到数据，请检查第一、第二步，配置问题。 
我查出来的是mysql-bin.000004、312
````
show master status;
````

Step4: 连接slave, 运行以下命令连接master
````
change master to master_host='<master-ip>',master_user='backup',master_password='123456', master_log_file='mysql-bin.000004',master_log_pos=312,master_port=3307;
````

Step5: 启动slave
````
start slave;
````

Step6: 查看slave status。
````
show slave status;
````

如果看到Waiting for master send event 什么的就成功了，你现在在主库上的修改，都会同步到从库上。