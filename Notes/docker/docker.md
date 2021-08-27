# Docker常用命令

## 帮助命令

```shell
docker version		#查看docker的版本信息
docker info			#查看docker的系统信息，包括镜像和容器的数量
docker 命令 --help	#帮助命令(可查看可选的参数)
```

docker帮助文档地址:https://docs.docker.com/engine/reference/commandline/docker/

## 镜像命令

#### 1. docker images 	//查看所有本地镜像

```shell
[root@healthSH ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    d1165f221234   5 months ago   13.3kB

# 解释
REPOSITORY：仓库源
TAG：镜像的标签
IMAGE ID：镜像的ID
CREATED：镜像的创建时间
SIZE：镜像的大小

# 可选项
-a, --all             # 列出所有镜像
-q, --quiet           # 只显示镜像的ID

```

#### 2. docker search	//搜索镜像（类似 GitHub）

```shell
[root@healthSH ~]# docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   11325     [OK]       
mariadb                           MariaDB Server is a high performing open sou…   4303      [OK]       
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   840                  [OK]
······  


# 可选项
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output

docker search mysql --filter=STARS=3000	# 搜索收藏数目大于3000的

```

#### 3. docker pull	//下载镜像

```shell
# 下载各行介绍
[root@healthSH ~]# docker pull mysql
Using default tag: latest	# 如果不写tag，默认就是latest最新版
latest: Pulling from library/mysql
e1acddbe380c: Pull complete 	# 分层下载，docker image的核心 联合文件系统
bed879327370: Pull complete 
03285f80bafd: Pull complete 
ccc17412a00a: Pull complete 
1f556ecc09d1: Pull complete 
adc5528e468d: Pull complete 
1afc286d5d53: Pull complete 
6c724a59adff: Pull complete 
0f2345f8b0a3: Pull complete 
c8461a25b23b: Pull complete 
3adb49279bed: Pull complete 
77f22cd6c363: Pull complete 
Digest: sha256:d45561a65aba6edac77be36e0a53f0c1fba67b951cb728348522b671ad63f926	# 签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest	#真实地址

# 等价命令
docker pull mysql等价于 docker pull docker.io/library/mysql:latest

# 指定版本下载
docker pull mysql:5.7
```

#### 4. docker rmi	//删除镜像

```shell
docker rmi -f [镜像ID]|[镜像名]		#指定删除镜像
docker rmi -f $(docker images -aq)	#批量删除全部镜像
```



## 容器命令

#### 1. docker run	//新建容器并启动

```shell
docker run [可选参数] 镜像:[版本]

# 参数说明
--name="Name"          指定容器名字
-d                     后台方式运行
-it                    使用交互方式运行（即启动并进入容器）
-p                     指定容器的端口
	-p ip:主机端口:容器端口		配置主机端口映射到容器端口
	-p 主机端口:容器端口（常用）
	-p 容器端口
-P                     随机指定端口(大写的P)
--rm				 容器用完停止后自动删除容器

# 测试
[root@healthSH ~]# docker run -it centos /bin/bash	//运行并进入容器
[root@f02cb37b68c2 /]# exit	//退出容器

[root@healthSH ~]# docker run -it tomcat:9.0	//指定版本创建容器

不用pull拉镜像的话，直接执行run会自动下载镜像
```

#### 2. docker ps	//列出运行的容器

```shell
docker ps [可选参数]

#参数说明
无参     # 列出当前正在运行的容器
-a		# 列出正在运行的容器+历史运行过的容器
-n=? 	# 显示最近创建的n个容器
-q   	# 只显示容器的编号
```

#### 3. 退出容器

```bash
exit	#退出容器，同时容器停止运行
ctrl+P+Q	#退出容器，容器不停止运行
```

#### 4. docker rm	//删除容器

```bash
docker rm 容器ID		#删除指定的容器（删不了正在运行的容器）
docker rm -f 容器ID	#强制删除
docker rm -f $(docker ps -aq)	#删除全部容器
docker ps -a -q|xargs docker rm		#删除所有容器
```

#### 5. 启动和停止容器

```shell
docker start 容器ID	# 启动容器
docker restart 容器ID	# 重启容器
docker stop 容器ID	# 停止当前正在运行的容器
docker kill 容器ID	# 强制停止容器（杀死ta）
docker run -d centos	# 后台启动容器（不进去），如果容器里面没有应用运行该容器就会自动停止运行
```

#### 6. 进入当前正在运行的容器

```shell
docker exec -it 容器ID /bin/bash	# 进入正在运行的容器，开启一个新的终端，可以在里面操作（常用）
docker attach 容器ID		# 进入正在运行的容器，进的是正在执行的终端，不会启动新的进程
```

#### 7. 拷贝容器内文件到主机上

```shell
docker cp 容器ID:容器内路径 目的主机路径
docker cp 容器ID:/home/test.java /home	# 拷贝容器内的/home/test.java到主机的/home目录中（命令在主机执行的）
```



## 其他常用命令

#### 1. 查看日志

```shell
docker logs -tf 容器ID	#显示日志（f：格式化显示）
docker logs --tail number 容器id #num为要显示的日志条数
```

#### 2. 查看容器中的进程信息

```shell
docker top 容器ID		#查看容器内部的进程信息
```

#### 3. 查看容器的元数据

```shell
docker inspect 容器ID
```

#### 4.查看各个容器cpu状态

```shell
docker status
```



## 小结图例

![image-20210827173129646](C:\Users\xiaoxi\AppData\Roaming\Typora\typora-user-images\image-20210827173129646.png)



## 实例演示

### 1. 部署Nginx

查看nginx镜像：https://hub.docker.com/

```shell
docker search nginx		# 搜索镜像（网站和命令都可以）
docker pull nginx		# 下载镜像
docker images		    # 查看镜像
docker run -d --name nginx01 -p:3344:80	nginx		# -d:后台启动容器并且返回容器ID
												# --name：为容器取名
												# -p:暴漏到外部的端口号:自己的端口号（通过公网的3344可以访问到docker的80）
												# nginx 镜像名
docker ps				# 查看容器
curl localhost:3344		 # 本机测试（上述命令都是在host上执行）

docker exec -it nginx01 /bin/bash	# 进入容器
```

-p 端口暴漏图解

![image-20210827175614261](C:\Users\xiaoxi\AppData\Roaming\Typora\typora-user-images\image-20210827175614261.png)





### 2. 部署tomcat

```shell
docker pull tomcat:9.0		# 下载镜像
docker run -d tomcat9.0 -p:4444:8080 --name	tomcat	# 启动运行
# 此时已经成功了，但访问会显示404

docker exec -it tomcat9.0 /bin/bash		# 进入tomcat

# 问题：1.linux命令少了 2.webapps里面没有东西
# 原因：镜像是最小镜像，所有不必要的东西都剔除了，只保证最小可运行环境
# 解决：tomcat容器的话 webapps.dist中有webapps应该有的东西，可以移过去或重命名
```



### 3. 部署 es+kibana

```shell
# es暴漏的端口多
# es十分耗内存
# es的数据一般放置在安全目录！挂载
# --net somenetwork ？ 网络配置

docker run -d --name elasticsearch -p:9200:9200 -p:9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2		# 启动es

curl localhost:9200			#测试

# 设置内存限制启动，修改配置文件 -e 环境配置修改
docker run -d --name elasticsearch -p:9200:9200 -p:9300:9300 -e "discovery.type=single-node" ES_JAVA_OPPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2

curl localhost:9200		#测试
```



##  图形化管理工具Portaniner安装

```shell
docker run -d -p 8088:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer	# 安装
docker ps	# 查看容器
curl localhost:8088		# 测试
外网访问路径：http://47.100.81.153:8088/
1.首次进入需设置admin的密码（也可以创建其他用户）
2.选择Local进行连接进入
3.一般不使用，自测玩玩即可
```



## Commit镜像

```shell
docker commit		# 提交容器成为一个新的副本

# 命令类似于git
docker commit -m="描述信息" -a="作者" 容器ID 自定义镜像名:[TAG] 		# 提交容器成为一个新的副本，tag一般为版本号
```

#### commit 实例

```shell
docker run -d tomcat9.0 -p:8080:8080 --name	tomcat01	# 创建、启动容器
# 进入容器并对此进行修改
docker ps		# 拿到容器ID
docker commit -m="我的tomcat" -a="health" 容器ID tomcatMe:11.0		# 提交镜像
docker images		# 此时可以看到刚刚提交的tomcatMe镜像
docker run -it tomcatMe:11.0 /bin/bash		# 用刚才创建的镜像新建容器
```