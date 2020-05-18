# docker
解决环境的不一致性

## 使用
启动docker
`systemctl start docker`

配置docker镜像地址
在`/etc/docker/`目录下创建`daemon.json`
```bash
{
  "registry-mirrors": ["https://a5vuu60b.mirror.aliyuncs.com"]
}
```

然后输入命令`systemctl daemon-reload`和`systemctl restart docker`

## 镜像与容器
镜像：是一个文件的副本，可以被特定的软件识别。一个镜像可以生成很多个容器

容器：运行镜像。一个容器对应一个镜像。一个镜像可以生成很多容器

如果一个启动的容器没有事情做，docker会直接将他杀死

- docker pull ... 拉取一个镜像
- docker run ... 把镜像启动成容器
  - docker run -it ... 将终端绑定这个容器的交互
  - docker run -it -d ... 后台的方式运行，通过-it，绑定交互让没事做的容器不会被杀死
- docker images 查看docker中所有的镜像
- docker ps -a 查看所有运行的容器
- docker rmi ... 删除镜像
- docker rm ... 删除容器
- docker rm $(docker ps -pa) 批量删除运行中的容器
- docker stop id 关闭容器
- docker exec -it id /bin/bash 进入到已经开启的容器中，指定命令行的解释器

## docker安装mysql、jdk、tomcat
- `docker pull mysql:版本`
  - `docker -e MYSQL_ROOT_PASSWORD=123456 -p 3307:3306 mysql` 运行mysql容器。宿主机3307映射到容器中的3306端口。别的程序要用mysql时先定位到宿主机3307端口。进入容器后`mysql -uroot -p123456`运行mysql
- `docker pull mcr.microsoft.com/java/jdk:8u192-zulu-alpine`
  - `docker run -it mcr.microsoft.com/java/jdk`
- `docker pull tomcat`
  - `docker run -p8080:8080 tomcat`

## 数据卷
实现数据的持久化。让容器中的数据保存到宿主机中。

`docker run -it -v /mydatas:/container/datas centos` 容器中/containers/datas中产生的数据会自动复制到宿主机的/mydatas中。宿主机中的数据也会同步到容器中

原理是 这两个文件的inode是一样的，其实就是同一个文件

## dockerfile
镜像的描述文件。

关键字
- FROM 我们构建的镜像是基于哪个镜像来的
- MAINTAINER 定义作者是谁
- ADD 拷贝文件并解压
- COPY 拷贝文件
- RUN 运行shell命令
- ENV 定义环境变量
- CMD 镜像在启动容器的时候执行的命令
- WORKDIR 进入到容器之后的落脚点
- ENTRYPOINT 在启动容器的时候执行的命令
- EXPOSE 容器对外保留的端口

创建一个`dockerfile`文件
```bash
FROM centos:latest
MAINTAINER wyj
COPY ./a.text / # 将宿主下的文件拷贝到容器中
CMD ["/bin/bash"]
```

`docker build -t mycentos1 .` 构建一个自定义容器，-t为命名，.代表上下文选择为当前文件夹。

### 配置jdk环境
```bash
FROM centos:latest
MAINTAINER wyj
# COPY ./jdk.tar.gz / # 将宿主下的文件拷贝到容器中
# RUN tar -zxf /jdk.tar.gz
ADD ./jdk.tar.gz / # 可以代替COPY和RUN
ENV JAVA_HOME=... # 这里是jdk解压后的文件名
ENV PATH=$JAVA_HOME/BIN:$PATH
CMD ["/bin/bash"]
```
