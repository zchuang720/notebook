一些项目的框架和技术栈

# mgvcore

这个项目最终会替换 Python 版的 megaview 项目。
该项目既提供 HTTP 接口，也提供 gRPC 接口。HTTP 接口供 Web/App 使用，gRPC 接口将来供开放平台使用。

## 生产控制
- 运维平台：Spug
- SQL审核平台：Archery

## 设计

### 依赖

HTTP 框架：[echo](https://echo.labstack.com)

RPC 框架：[gRPC](https://grpc.io)

链路跟踪：[jaeger](https://github.com/jaegertracing/jaeger)

数据库操作：[gorm](https://gorm.io)

ES 操作：[elastic](github.com/olivere/elastic/v7)

command line app：[cli](https://github.com/urfave/cli)

日志库：[zerolog](github.com/rs/zerolog)

限流：[raltelimit](https://github.com/uber-go/ratelimit)

熔断器：[circuitbreaker](https://github.com/rubyist/circuitbreaker)

优雅重启：[tableflip](https://github.com/cloudflare/tableflip)

### 注册发现

考虑使用 consul。

<details>

### 目录结构

```bash
.
├── README.md
├── api                 // 存放 API，包括文档（看是否有必要引入 swagger）、pb 定义
│   └── proto           // pb 定义放在该目录下
├── cmd
│   └── mgvcore         // 最终可执行文件名，放 main.go
├── config              // 配置文件存放目录
│   ├── banner.txt      // 启动的 banner
│   ├── config_pro.toml // 生产环境配置文件
│   ├── config_test.toml // 测试环境配置文件
│   └── config_dev.toml  // 测试环境配置文件
├── gen                 // 根据 pb 生成的 python 等其他语言的代码
│   ├── helloworld_pb2.py
│   ├── helloworld_pb2_grpc.py
│   └── test.py
├── global              // 全局使用的项目相关信息
│   └── app.go
├── go.mod
├── go.sum
├── internal            // 核心业务逻辑代码
│   ├── grpc            // grpc 服务
│   │   ├── interceptor // grpc 的自定义拦截器
│   │   └── server      // grpc 的 Server 实现
│   ├── http            // HTTP 服务
│   │   ├── controller
│   │   ├── internal
│   │   ├── middleware  // Web 中间件
│   │   └── render
│   ├── dao             // 数据库操作层
│   ├── logic           // 业务逻辑层
│   ├── model           // model 层       
└── ssl                 // grpc 需要的 ssl 文件
    ├── ca.crt
    ├── ca.csr
    ├── ca.key
    ├── ca.srl
    ├── server.crt
    ├── server.csr
    └── server.key
```

此外，有一个隐藏文件：`.air.conf`，这是 https://github.com/cosmtrek/air 工具的配置文件，方便开发时实时调试。

## 启动、测试

根据环境将对应配置文件做一个软链：（如果有针对自己环境的修改，建议 copy 一份，而不是做软链）

```bash
$ ln -s config_dev.toml config.toml
```

执行如下命令启动服务：

```bash
$ go run ./cmd/mgvcore

Build Info:
        Git Commit Log: unknown
        Git Release Info: unknown
        Build Time: unknown
        Go Version: go1.18.3
        gRPC Version: 1.47.0
        PID: 13616
        Enviroment: dev
 _____ ______   ________  ___      ___ ________  ________  ________  _______      
|\   _ \  _   \|\   ____\|\  \    /  /|\   ____\|\   __  \|\   __  \|\  ___ \     
\ \  \\\__\ \  \ \  \___|\ \  \  /  / \ \  \___|\ \  \|\  \ \  \|\  \ \   __/|    
 \ \  \\|__| \  \ \  \  __\ \  \/  / / \ \  \    \ \  \\\  \ \   _  _\ \  \_|/__  
  \ \  \    \ \  \ \  \|\  \ \    / /   \ \  \____\ \  \\\  \ \  \\  \\ \  \_|\ \ 
   \ \__\    \ \__\ \_______\ \__/ /     \ \_______\ \_______\ \__\\ _\\ \_______\
    \|__|     \|__|\|_______|\|__|/       \|_______|\|_______|\|__|\|__|\|_______|
                                                                                  
=> grpc server started on: :2022

   ____    __
  / __/___/ /  ___
 / _// __/ _ \/ _ \
/___/\__/_//_/\___/ v4.8.0
High performance, minimalist Go web framework
https://echo.labstack.com
____________________________________O/_______
                                    O\
⇨ http server started on [::]:2023
```

## 优雅重启

```bash
$ kill -USR2 `cat /data/run/mgvcore.pid`
```

## 开发指南

基于 gRPC，因此你应该学习 gRPC 相关知识。

[mgvcore 项目开发指南](https://geimkewdfm.feishu.cn/wiki/wikcnRRvd3vTfNUOfXc6Hmv24gf)

HTTP 框架，Echo 和 Gin 没有太多区别，很类似。

</details>

# abc