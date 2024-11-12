# kong 网关神器
kong 组件 
service service对应服务，可以直接指向一个API服务节点，也可以指定一个upstream实现负载均衡
route route 对应路由，通过route可以指定访问的路径，以及访问的服务
upstream对应一组API节点，实现负载均衡
target target对应一个API节点

一个简单指令
curl -x POST http://127.0.0.1:8001/upstreams --data name="minghao"

创建service和route
curl -x POST  http://127.0.0.1:8001/services --data "name=minghao" --data "host=minghao-upstream" --data "path=/minghao"
猜测使用lua完成动态加载配置的能力



安装 Kong
Kong 可以通过多种方式安装，包括 Docker、包管理器（如 APT、YUM）和手动安装。以下是一个使用 Docker 安装的例子：


`# 启动 Kong 的依赖服务（如 Cassandra 或 PostgreSQL）
docker network create kong-net
docker run -d --name kong-database \
--network=kong-net \
-p 5432:5432 \
-e "POSTGRES_USER=kong" \
-e "POSTGRES_DB=kong" \
postgres:9.6

# 等待数据库启动
sleep 10

# 初始化 Kong 的数据库
docker run --rm \
--network=kong-net \
-e "KONG_DATABASE=postgres" \
-e "KONG_PG_HOST=kong-database" \
-e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
kong:latest kong migrations bootstrap

# 启动 Kong
docker run -d --name kong \
--network=kong-net \
-e "KONG_DATABASE=postgres" \
-e "KONG_PG_HOST=kong-database" \
-e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" \
-e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
-e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
-e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
-e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
-e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" \
-p 8000:8000 \
-p 8443:8443 \
-p 8001:8001 \
-p 8444:8444 \
kong:latest

`kong version`

启动Kong
`kong start`

停止kong
`kong stop`

重启kong
`kong restart`

重新加载Kong配置
`kong reload`


API管理
添加API服务
`curl -i -X POST http://localhost:8001/services/ \
  --data 'name=example-service' \
  --data 'url=http://mockbin.org/request'`

添加API路由
`curl -i -X POST http://localhost:8001/services/example-service/routes \
  --data 'paths[]=/example'`


添加插件
`curl -i -X POST http://localhost:8001/services/example-service/plugins/ \
  --data 'name=key-auth'`

插件管理
`curl -i -X GET http://localhost:8001/plugins/`

添加全局插件
`curl -i -X POST http://localhost:8001/plugins/ \
  --data 'name=request-termination' \
  --data 'config.status_code=403' \
  --data 'config.body=Access denied'`

消费者管理
`curl -i -X POST http://localhost:8001/consumers/ \
  --data 'username=my-consumer'`

消费者凭证
`curl -i -X POST URL_ADDRESS \
  --data 'key=my-secret-key'`

日志监控
`docker logs kong`

监控kong的状态
`curl -i -X GET http://localhost:8001/status/`

运行数据库迁移
`kong migrations up`

回滚数据库迁移
`kong migrations down`

配置文件管理
`kong config show`

修改配置文件
`kong reload`

kong的限流配置


kong的限流
