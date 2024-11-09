Docker compose是一个用于定义和运行多容器Docker应用程序的工具。通过一个YAML文件，你可以配置应用程序、网络、卷。使用 Docker Compose，你可以使用单个命令来启动、停止和管理所有的服务。
主要功能：
1、定义服务
2、网络配置
3、卷管理
4、依赖关系
5、环境变量
6、构建镜像

基本用法
```YAML
version : '3.9'
services: 
  web:  
    image: nginx:last
    ports: 
      - "8080"
    volumes:
      - ./html:/usr/share/nginx/html
  db:
    image: postgres:latest
    environment:
      POSTGRES_USER:  example
      POSTGRES_PASSWORD: example
    volumes:
      -  pgdata:/var/lib/postgressql/data

    
```
通过上述两个文件可以轻松启动和管理这两个服务