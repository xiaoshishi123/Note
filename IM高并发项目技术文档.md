# IM高并发

## 一，第一周--基本框架搭建

#### 1, 环境配置

##### docker安装mysql



1 这两个文件夹照常即可

在根目录创建文件夹的

```
// 配置文件挂载点
mkdir %USERPROFILE%\docker\mysql\config\conf.d

// 数据文件挂载点
mkdir %USERPROFILE%\docker\mysql\data
```

2，拉取镜像

```
docker pull mysql:latest
```

3，下面的启动容器更改了一下密码为1234  同时路径也修改了

```
docker run -p 49152:3306 --name mysql -d --restart=always -e MYSQL_ROOT_PASSWORD="1234" -v "%USERPROFILE%\docker\mysql\data:/var/lib/mysql" -v "%USERPROFILE%\docker\mysql\config\conf.d:/etc/mysql/conf.d" mysql:latest





```



##### docker安装redis

1,拉取镜像（不变）

```
docker pull redis:latest
```

2，创建文件夹 和配置  这里密码改成了1234

```
mkdir "%USERPROFILE%\docker\redis\config"
mkdir "%USERPROFILE%\docker\redis\data"

echo # Redis 配置文件示例 > "%USERPROFILE%\docker\redis\config\redis.conf"
echo bind 0.0.0.0 >> "%USERPROFILE%\docker\redis\config\redis.conf"
echo port 6379 >> "%USERPROFILE%\docker\redis\config\redis.conf"
echo requirepass 1234 >> "%USERPROFILE%\docker\redis\config\redis.conf"  
echo save 900 1 >> "%USERPROFILE%\docker\redis\config\redis.conf"
echo save 300 10 >> "%USERPROFILE%\docker\redis\config\redis.conf"
echo save 60 10000 >> "%USERPROFILE%\docker\redis\config\redis.conf"
echo loglevel notice >> "%USERPROFILE%\docker\redis\config\redis.conf"
echo logfile "" >> "%USERPROFILE%\docker\redis\config\redis.conf"
echo databases 16 >> "%USERPROFILE%\docker\redis\config\redis.conf"
echo notify-keyspace-events Ex >> "%USERPROFILE%\docker\redis\config\redis.conf"

```

3，启动容器

```
 
docker run ^
  -p 59000:6379 ^
  --name redis ^
  -d --restart=always ^
  -e REDIS_PASSWORD=1234 ^
  -v "%USERPROFILE%\docker\redis\data:/data" ^
  -v "%USERPROFILE%\docker\redis\config:/etc/redis" ^
  redis:latest ^
  redis-server /etc/redis/redis.conf --requirepass 1234 --notify-keyspace-events Ex
```

##### docker minio

这个好像没啥区别 密码也用他本身的密码得了

```
// 拉取镜像
docker pull minio/minio 

// 配置文件挂载点
mkdir %USERPROFILE%\docker\minio\config

// 数据文件挂载点
mkdir %USERPROFILE%\docker\minio\data

// 启动 minio 容器
docker run ^
-p 9000:9000 ^
-p 9090:9090 ^
--name minio ^
-d --restart=always ^
-e "MINIO_ACCESS_KEY=minioadmin" ^
-e "MINIO_SECRET_KEY=minioadmin" ^
-v "%USERPROFILE%\docker\minio\data:/data" ^
-v "%USERPROFILE%\docker\minio\config:/root/.minio" ^
minio/minio:RELEASE.2025-02-18T16-25-55Z server ^
/data --console-address ":9090" -address ":9000"

// 验证是否启动成功
docker ps
```



http://localhost:9090/

注意这里是localhost 因为你没上云



#### 2，新建一个项目

这个其实很疑惑啊 就是新建一个微服务的样本项目而已 其实没啥 你装好了之后跟着走就行

这个的父项目的配置文件 子项目的有点问题但是现在也不需要管  后面再说吧

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.shanyangcode.infinitechat</groupId>
    <artifactId>InfiniteChat</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>InfiniteChat</name>
    <description>千言</description>

    <packaging>pom</packaging>

    <modules>
        <module>AuthenticationService</module>
    </modules>


</project>

```

