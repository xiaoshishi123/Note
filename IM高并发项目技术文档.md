# IM高并发

#### 1, 环境配置

docker安装mysql



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

