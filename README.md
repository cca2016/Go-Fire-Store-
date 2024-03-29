# Distributed-Go-Fire-Store

基于golang实现的一种分布式云存储服务

## 关于微服务架构交互逻辑

<img src='/doc/microservice_interact_archi.png' width="800px"></img>


## 关于需要手动安装的库

如下：
```shell
go get github.com/garyburd/redigo/redis
go get github.com/go-sql-driver/mysql
#go get github.com/garyburd/redigo/redis
go get github.com/gomodule/redigo/redis
go get github.com/json-iterator/go
go get github.com/aliyun/aliyun-oss-go-sdk/oss
go get gopkg.in/amz.v1/aws
go get gopkg.in/amz.v1/s3
go get github.com/streadway/amqp
go get github.com/gin-gonic/gin
go get github.com/gin-contrib/cors
go get github.com/micro/go-micro
go get github.com/mitchellh/mapstructure
go get github.com/jteeuwen/go-bindata/...
go get github.com/moxiaomomo/go-bindata-assetfs/...
go get github.com/gin-gonic/contrib/static
go get github.com/micro/go-micro/cmd
go get github.com/micro/go-plugins/registry/kubernetes
go get -v github.com/micro/go-plugins/wrapper/breaker/hystrix
go get -v github.com/juju/ratelimit
go get -v github.com/micro/go-plugins/wrapper/ratelimiter/ratelimit
```

## 关于应用启动

- 在加入rabbitMQ实现文件异步转移之前，启动方式：

    - 启动上传应用程序:
```bash
# cd $GOPATH/<你的工程目录>
> cd $GOPATH/filestore-server
> go run main.go
```

- 在加入rabbitMQ实现文件异步转移阶段，启动方式(分裂成了两个独立程序)：

    - 启动上传应用程序:
```bash
# cd $GOPATH/<你的工程目录>
> cd $GOPATH/filestore-server
> go run service/upload/main.go
```

    - 启动转移应用程序:
```bash
# cd $GOPATH/<你的工程目录>
> cd $GOPATH/filestore-server
> go run service/transfer/main.go
```

-  微服务架构下启动方式(非容器化部署):

    - 一键启动微服务(start-all.sh):
```bash
> cd $GOPATH/filestore-server
> ./service/start-all.sh 
 编译完成:  service/bin/dbproxy
 编译完成:  service/bin/upload
 编译完成:  service/bin/download
 编译完成:  service/bin/transfer
 编译完成:  service/bin/account
 编译完成:  service/bin/apigw
 已启动  dbproxy
 已启动  upload
 已启动  download
 已启动  transfer
 已启动  account
 已启动  apigw
微服务启动完毕.
```

    - 一键关闭微服务(stop-all.sh):
```bash
> cd $GOPATH/filestore-server
> ./service/stop-all.sh 
 已关闭:  apigw
 已关闭:  account
 已关闭:  transfer
 已关闭:  download
 已关闭:  upload
 已关闭:  dbproxy
执行完毕.
```

-  微服务架构下启动方式(容器化部署):
```bash
> cd $GOPATH/filestore-server
# 脚本方式启动容器
> ./deploy/start-all.sh
# 脚本方式关闭容器
> ./deploy/stop-all.sh
# docker-compose方式启动容器
> cd ./deploy/service_dc
> sudo docker-compose up -d
# k8s方式启动微服务
> cd ./deploy/service_k8s
> kubectl apply -f svc_account.yaml
> kubectl apply -f svc_apigw.yaml
> kubectl apply -f svc_dbproxy.yaml
> kubectl apply -f svc_download.yaml
> kubectl apply -f svc_transfer.yaml
> kubectl apply -f svc_upload.yaml
> cd ./deploy/traefik_k8s
> kubectl apply -f service-ingress.yaml
```

## 进度说明：
* [x] 简单的文件上传服务
* [x] mysql存储文件元数据
* [x] 账号系统, 注册/登录/查询用户或文件数据
* [x] 基于帐号的文件操作接口
* [x] 文件秒传功能
* [x] 文件分块上传/断点续传功能
* [x] 搭建及使用Ceph对象存储集群
* [x] 使用阿里云OSS对象存储服务
* [x] 使用RabbitMQ实现异步任务队列
* [x] 微服务化(API网关, 服务注册, RPC通讯)
* [x] CI/CD(持续集成)
