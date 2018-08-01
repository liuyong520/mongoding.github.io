docker-compose
===

# Docker Compose

Docker提供一个容器编排工具，Docker Compose，允许用户在一个模板（YAML）中定义一组相关联的应用容器，这组容器会根据模板中的“--link”等参数，对启动的优先级自动排序，简单的执行一条“docker-compose up”，就可以把同一个服务中的多个容器依次创建和启动。

## 安装docker-compose

`curl -L https:``//github.com/docker/compose/releases/download/1.8.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose`

查看compose是否安装成功：

`docker-compose --version`

## Compose 命令
`Docker Compose`是一个编排多容器分布式部署的工具，提供命令集管理容器化应用的完整开发周期，包括服务构建，启动和停止。

首先看一下`Compose`命令的格式：

```bash
Usage:
  docker-compose [-f=<arg>...] [options] [COMMAND] [ARGS...]12
```

其中options有如下选项：

```bash
Options:
  -f, --file FILE           Specify an alternate compose file (default: docker-compose.yml)
  -p, --project-name NAME   Specify an alternate project name (default: directory name)
  --x-networking            (EXPERIMENTAL) Use new Docker networking functionality.
                            Requires Docker 1.9 or later.
  --x-network-driver DRIVER (EXPERIMENTAL) Specify a network driver (default: "bridge").
                            Requires Docker 1.9 or later.
  --verbose                 Show more output
  -v, --version             Print version and exit
12345678910
```

首先运行`docker-compose`命令需要指定服务(service)名称，可以同时指定多个service，也可以不指定，当不指定service名称时，默认对配置中的所有service执行命令。

其中`-f`标识用于指定Compose的配置文件，可以指定多个，当没有使用`-f`标识时，默认在项目跟目录及其子目录下寻找`docker-compose.yml`和`docker-compose.override.yml`文件，至少需要存在`docker-compose.yml`文件。

当指定了多个文件时(包括没指定`-f`但同时存在`docker-compose.yml`和`docker-compose.override.yml`文件)，Compose会将多个文件合并成一个配置文件，合并的结果与指定文件的顺序有关。合并有两种操作，或者添加，或者覆盖。具体的合并规则以后会单独用一篇文章介绍。

`-p`标识用于给项目指定一个名称，如过没有指定，默认使用项目根目录的名称作为项目名称。 
`-v`显示版本号

Compose的Commands有如下几个，一一介绍： 
* [build](https://blog.csdn.net/vchy_zhao/article/details/70238432#1) 
* [kill](https://blog.csdn.net/vchy_zhao/article/details/70238432#2) 
* [logs](https://blog.csdn.net/vchy_zhao/article/details/70238432#3) 
* [pause & unpause](https://blog.csdn.net/vchy_zhao/article/details/70238432#4) 
* [port](https://blog.csdn.net/vchy_zhao/article/details/70238432#5) 
* [ps](https://blog.csdn.net/vchy_zhao/article/details/70238432#6) 
* [pull](https://blog.csdn.net/vchy_zhao/article/details/70238432#7) 
* [restart](https://blog.csdn.net/vchy_zhao/article/details/70238432#8) 
* [rm](https://blog.csdn.net/vchy_zhao/article/details/70238432#9) 
* [run](https://blog.csdn.net/vchy_zhao/article/details/70238432#10) 
* [scale](https://blog.csdn.net/vchy_zhao/article/details/70238432#11) 
* [start & stop](https://blog.csdn.net/vchy_zhao/article/details/70238432#12) 
* [up](https://blog.csdn.net/vchy_zhao/article/details/70238432#13)

### build

```bash
Usage: build [options] [SERVICE...]

Options:
    --force-rm  Always remove intermediate containers.
    --no-cache  Do not use cache when building the image.
    --pull      Always attempt to pull a newer version of the image.
1234567
```

`docker-compose build`命令用来创建或重新创建服务使用的镜像，后面指定的是服务的名称，创建之后的镜像名为`project_service`,即项目名后跟服务名。比如项目名称为composeset，其中的一个服务名称为web，则`docker-compose build web`创建的镜像的名称为composeset_web。

和`docker build`一样，执行此命令也需要`Dockerfile`文件。当修改了Dockerfile文件或它的上下文之后，可以运行`docker-compose build`重新创建镜像，此时无需指定服务名称。

### kill

```bash
Usage: kill [options] [SERVICE...]

Options:
-s SIGNAL         SIGNAL to send to the container. Default signal is SIGKILL.1234
```

`docker-compose kill`命令用于通过向容器发送`SIGKILL`信号强行停止服务。 
`-s`标识用于覆盖默认发送的信号

### logs

```bash
Usage: logs [options] [SERVICE...]

Options:
--no-color  Produce monochrome output.1234
```

`docker-compose logs`命令用于展示service的日志 
`--no-color`标识使日志显示为单色

### pause & unpause

`docker-compose pause`暂停服务； 
`docker-compose unpause`恢复被暂停的服务；

### port

```bash
Usage: port [options] SERVICE PRIVATE_PORT

Options:
--protocol=proto  tcp or udp [default:  tcp]
--index=index     index of the container if there are multiple
                  instances of a service [default: 1]123456
```

`docker-conpose port`命令用于查看服务中的端口被映射到了宿主机的哪个端口上，使用这条命令时必须通知指定服务名称和内部端口号，完整命令示例：

```bash
$ docker-compose port web 5000  #查看web服务中5000端口被映射到宿主机的哪个端口上

0.0.0.0:5000123
```

### ps

`docker-compose ps`用于显示当前项目下的容器。注意，执行此命令时必须`cd`到项目的根目录下，否则提示如下错误：

```bash
ERROR: 
        Can't find a suitable configuration file in this directory or any parent. Are you in the right directory?

        Supported filenames: docker-compose.yml, docker-compose.yaml, fig.yml, fig.yaml1234
```

与`docker ps`不同，`docker-compose`会显示停止后的容器(即状态为`Exited`的容器)；

`docker-compose ps`只能查看当前项目的容器，如果要显示本机上所有的容器，请使用`docker ps -a`。

### pull

```bash
Usage: pull [options] [SERVICE...]

Options:
--ignore-pull-failures  Pull what it can and ignores images with pull failures.1234
```

`docker-compose pull`用于；拉取服务依赖的镜像；

### restart

`docker-compose restart`用于重启某个服务的所有容器，后跟服务名。

只有正在运行的服务才能重启，停止的服务不能使用`restart`命令。

### rm

```bash
Usage: rm [options] [SERVICE...]

Options:
-f, --force   Don't ask to confirm removal
-v            Remove volumes associated with containers12345
```

`docker-compose rm`删除停止的服务(容器) 
`-f`表示强制删除 
`-v`标识表示删除与容器相关的卷(volumes)

### run

```bash
Usage: run [options] [-p PORT...] [-e KEY=VAL...] SERVICE [COMMAND] [ARGS...]

Options:
    --allow-insecure-ssl  Deprecated - no effect.
    -d                    Detached mode: Run container in the background, print
                          new container name.
    --name NAME           Assign a name to the container
    --entrypoint CMD      Override the entrypoint of the image.
    -e KEY=VAL            Set an environment variable (can be used multiple times)
    -u, --user=""         Run as specified username or uid
    --no-deps             Don't start linked services.
    --rm                  Remove container after run. Ignored in detached mode.
    -p, --publish=[]      Publish a container's port(s) to the host
    --service-ports       Run command with the service's ports enabled and mapped
                          to the host.
    -T                    Disable pseudo-tty allocation. By default `docker-compose run`
                          allocates a TTY.1234567891011121314151617
```

`docker-compose run`命令用于在服务中运行一个一次性的命令。使用这个命令会新建一个容器，其配置和service的配置一样，也就是说新建的容器和service启动的容器有相同的volumes，links等。仅管如此，还是有两点不一样： 
- `run`指定的命令会覆盖service配置中指定的命令 
- `run`命令启动的容器不会创建任何在service配置中指定的端口，这避免了端口的冲突。如果你确实想创建端口并映射到宿主机上，可以使用`--service-ports`，例如： 
`bash 
$ docker-compose run --service-ports web python manage.py shell` 
此外，端口的映射也可以改变，通过`-p`(`--publish`)标识。例如： 
`bash 
$ docker-compose run -d -p 7001:8000 web python manage.py runserver 0.0.0.0:8000` 
上面的命令创建一个新的容器，其配置与web service一样，且将其8000端口映射到宿主机的7001端口上。

使用`docker-compose run`启动一个容器时，如果service中有`--link`指定的其他服务没有运行，会先运行这些服务，`--link`依赖的服务都运行成功后，再执行指定的命令。如果你不想启动这些依赖的容器，可以使用`--no-deps`标识。

```bash
$ docker-compose run --no-deps web python manage.py shell1
```

此外: 
`-e`用来添加环境变量； 
`--rm`指定在`run`命令之后删除容器，如果指定了`-d`则忽略`--rm`标识； 
`-d`指定后台运行； 
`--name`指定容器的名字；

### scale

`docker-compose scale`指定某一个服务启动的容器的个数，其参数格式为`[service=num]`，例如：

```bash
$ docker-compose scale web=2 worker=31
```

这条命令可以使某项服务启动多个容器，但当容器有到主机的端口映射时，因为所有容器都指向一个宿主机的端口，所以只能启动一个容器，其他的会失败。

### start & stop

`docker-compose start`命令启动运行某个服务的所有容器； 
`docker-compose stop`命令停止运行一个服务的所有容器；

### up

```bash
Usage: up [options] [SERVICE...]

Options:
    --allow-insecure-ssl   Deprecated - no effect.
    -d                     Detached mode: Run containers in the background,
                           print new container names.
    --no-color             Produce monochrome output.
    --no-deps              Don't start linked services.
    --force-recreate       Recreate containers even if their configuration and
                           image haven't changed. Incompatible with --no-recreate.
    --no-recreate          If containers already exist, don't recreate them.
                           Incompatible with --force-recreate.
    --no-build             Don't build an image, even if it's missing
    -t, --timeout TIMEOUT  Use this timeout in seconds for container shutdown
                           when attached or when containers are already
                           running. (default: 10)12345678910111213141516
```

`docker-compose up`创建并运行作为服务的容器，并将其输入输出重定向到控制台(attach)，并将所有容器的输出合并到一起。命令退出后，所有的容器都会停止。

如果`--link`依赖的容器没有运行则运行依赖的容器； 
`-d`标识指定容器后台运行； 
如果已经存在服务的容器，且容器创建后服务的配置有变化，就重新创建容器。如果没有变化，默认不会重新创建容器； 
`--force-recreate`标识指定即使服务配置没有变化，也重新创建容器； 
`--no-recreate`标识表示如果服务的容器已经存在，不要重新创建它们；

# docker-compose 配置指令

先来看一份 docker-compose.yml 文件，不用管这是干嘛的，只是有个格式方便后文解说：

```
version: '2'
services:
  web:
    image: dockercloud/hello-world
    ports:
      - 8080
    networks:
      - front-tier
      - back-tier

  redis:
    image: redis
    links:
      - web
    networks:
      - back-tier

  lb:
    image: dockercloud/haproxy
    ports:
      - 80:80
    links:
      - web
    networks:
      - front-tier
      - back-tier
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock 

networks:
  front-tier:
    driver: bridge
  back-tier:
driver: bridge

```

可以看到一份标准配置文件应该包含 version、services、networks 三大部分，其中最关键的就是 services 和 networks 两个部分，下面先来看 services 的书写规则。

## 1\. image

```
services:
  web:
    image: hello-world

```

在 services 标签下的第二级标签是 web，这个名字是用户自己自定义，它就是服务名称。
image 则是指定服务的镜像名称或镜像 ID。如果镜像在本地不存在，Compose 将会尝试拉取这个镜像。
例如下面这些格式都是可以的：

```
image: redis
image: ubuntu:14.04
image: tutum/influxdb
image: example-registry.com:4000/postgresql
image: a4bc65fd

```

## 2\. build

服务除了可以基于指定的镜像，还可以基于一份 Dockerfile，在使用 up 启动之时执行构建任务，这个构建标签就是 build，它可以指定 Dockerfile 所在文件夹的路径。Compose 将会利用它自动构建这个镜像，然后使用这个镜像启动服务容器。

```
build: /path/to/build/dir

```

也可以是相对路径，只要上下文确定就可以读取到 Dockerfile。

```
build: ./dir

```

设定上下文根目录，然后以该目录为准指定 Dockerfile。

```
build:
  context: ../
  dockerfile: path/of/Dockerfile

```

注意 build 都是一个目录，如果你要指定 Dockerfile 文件需要在 build 标签的子级标签中使用 dockerfile 标签指定，如上面的例子。
如果你同时指定了 image 和 build 两个标签，那么 Compose 会构建镜像并且把镜像命名为 image 后面的那个名字。

```
build: ./dir
image: webapp:tag

```

既然可以在 docker-compose.yml 中定义构建任务，那么一定少不了 arg 这个标签，就像 Dockerfile 中的 ARG 指令，它可以在构建过程中指定环境变量，但是在构建成功后取消，在 docker-compose.yml 文件中也支持这样的写法：

```
build:
  context: .
  args:
    buildno: 1
    password: secret

```

下面这种写法也是支持的，一般来说下面的写法更适合阅读。

```
build:
  context: .
  args:
    - buildno=1
    - password=secret

```

与 ENV 不同的是，ARG 是允许空值的。例如：

```
args:
  - buildno
  - password

```

这样构建过程可以向它们赋值。

> 注意：YAML 的布尔值（true, false, yes, no, on, off）必须要使用引号引起来（单引号、双引号均可），否则会当成字符串解析。

## 3\. command

使用 command 可以覆盖容器启动后默认执行的命令。

```
command: bundle exec thin -p 3000

```

也可以写成类似 Dockerfile 中的格式：

```
command: [bundle, exec, thin, -p, 3000]

```

## 4.container_name

前面说过 Compose 的容器名称格式是：<项目名称>_<服务名称>_<序号>
虽然可以自定义项目名称、服务名称，但是如果你想完全控制容器的命名，可以使用这个标签指定：

```
container_name: app

```

这样容器的名字就指定为 app 了。

## 5.depends_on

在使用 Compose 时，最大的好处就是少打启动命令，但是一般项目容器启动的顺序是有要求的，如果直接从上到下启动容器，必然会因为容器依赖问题而启动失败。
例如在没启动数据库容器的时候启动了应用容器，这时候应用容器会因为找不到数据库而退出，为了避免这种情况我们需要加入一个标签，就是 depends_on，这个标签解决了容器的依赖、启动先后的问题。
例如下面容器会先启动 redis 和 db 两个服务，最后才启动 web 服务：

```
version: '2'
services:
  web:
    build: .
    depends_on:
      - db
      - redis
  redis:
    image: redis
  db:
    image: postgres

```

注意的是，默认情况下使用 docker-compose up web 这样的方式启动 web 服务时，也会启动 redis 和 db 两个服务，因为在配置文件中定义了依赖关系。

## 6.dns

和 --dns 参数一样用途，格式如下：

```
dns: 8.8.8.8

```

也可以是一个列表：

```
dns:
  - 8.8.8.8
  - 9.9.9.9

```

此外 dns_search 的配置也类似：

```
dns_search: example.com
dns_search:
  - dc1.example.com
  - dc2.example.com

```

## 7\. tmpfs

挂载临时目录到容器内部，与 run 的参数一样效果：

```
tmpfs: /run
tmpfs:
  - /run
  - /tmp

```

## 8\. entrypoint

在 Dockerfile 中有一个指令叫做 ENTRYPOINT 指令，用于指定接入点，第四章有对比过与 CMD 的区别。
在 docker-compose.yml 中可以定义接入点，覆盖 Dockerfile 中的定义：

```
entrypoint: /code/entrypoint.sh

```

格式和 Docker 类似，不过还可以写成这样：

```
entrypoint:
    - php
    - -d
    - zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20100525/xdebug.so
    - -d
    - memory_limit=-1
    - vendor/bin/phpunit

```

## 9.env_file

还记得前面提到的 .env 文件吧，这个文件可以设置 Compose 的变量。而在 docker-compose.yml 中可以定义一个专门存放变量的文件。
如果通过 docker-compose -f FILE 指定了配置文件，则 env_file 中路径会使用配置文件路径。

如果有变量名称与 environment 指令冲突，则以后者为准。格式如下：

```
env_file: .env

```

或者根据 docker-compose.yml 设置多个：

```
env_file:
  - ./common.env
  - ./apps/web.env
  - /opt/secrets.env

```

注意的是这里所说的环境变量是对宿主机的 Compose 而言的，如果在配置文件中有 build 操作，这些变量并不会进入构建过程中，如果要在构建中使用变量还是首选前面刚讲的 arg 标签。

## 10\. environment

与上面的 env_file 标签完全不同，反而和 arg 有几分类似，这个标签的作用是设置镜像变量，它可以保存变量到镜像里面，也就是说启动的容器也会包含这些变量设置，这是与 arg 最大的不同。
一般 arg 标签的变量仅用在构建过程中。而 environment 和 Dockerfile 中的 ENV 指令一样会把变量一直保存在镜像、容器中，类似 docker run -e 的效果。

```
environment:
  RACK_ENV: development
  SHOW: 'true'
  SESSION_SECRET:

environment:
  - RACK_ENV=development
  - SHOW=true
  - SESSION_SECRET

```

## 11\. expose

这个标签与Dockerfile中的EXPOSE指令一样，用于指定暴露的端口，但是只是作为一种参考，实际上docker-compose.yml的端口映射还得ports这样的标签。

```
expose:
 - "3000"
 - "8000"

```

## 12\. external_links

在使用Docker过程中，我们会有许多单独使用docker run启动的容器，为了使Compose能够连接这些不在docker-compose.yml中定义的容器，我们需要一个特殊的标签，就是external_links，它可以让Compose项目里面的容器连接到那些项目配置外部的容器（前提是外部容器中必须至少有一个容器是连接到与项目内的服务的同一个网络里面）。
格式如下：

```
external_links:
 - redis_1
 - project_db_1:mysql
 - project_db_1:postgresql

```

## 13\. extra_hosts

添加主机名的标签，就是往/etc/hosts文件中添加一些记录，与Docker client的--add-host类似：

```
extra_hosts:
 - "somehost:162.242.195.82"
 - "otherhost:50.31.209.229"

```

启动之后查看容器内部hosts：

```
162.242.195.82  somehost
50.31.209.229   otherhost

```

## 14\. labels

向容器添加元数据，和Dockerfile的LABEL指令一个意思，格式如下：

```
labels:
  com.example.description: "Accounting webapp"
  com.example.department: "Finance"
  com.example.label-with-empty-value: ""
labels:
  - "com.example.description=Accounting webapp"
  - "com.example.department=Finance"
  - "com.example.label-with-empty-value"

```

## 15\. links

还记得上面的depends_on吧，那个标签解决的是启动顺序问题，这个标签解决的是容器连接问题，与Docker client的--link一样效果，会连接到其它服务中的容器。
格式如下：

```
links:
 - db
 - db:database
 - redis

```

使用的别名将会自动在服务容器中的/etc/hosts里创建。例如：

```
172.12.2.186  db
172.12.2.186  database
172.12.2.187  redis

```

相应的环境变量也将被创建。

## 16\. logging

这个标签用于配置日志服务。格式如下：

```
logging:
  driver: syslog
  options:
    syslog-address: "tcp://192.168.0.42:123"

```

默认的driver是json-file。只有json-file和journald可以通过docker-compose logs显示日志，其他方式有其他日志查看方式，但目前Compose不支持。对于可选值可以使用options指定。
有关更多这方面的信息可以阅读官方文档：
[https://docs.docker.com/engine/admin/logging/overview/](https://link.jianshu.com?t=https://docs.docker.com/engine/admin/logging/overview/)

## 17\. pid

```
pid: "host"

```

将PID模式设置为主机PID模式，跟主机系统共享进程命名空间。容器使用这个标签将能够访问和操纵其他容器和宿主机的名称空间。

## 18\. ports

映射端口的标签。
使用HOST:CONTAINER格式或者只是指定容器的端口，宿主机会随机映射端口。

```
ports:
 - "3000"
 - "8000:8000"
 - "49100:22"
 - "127.0.0.1:8001:8001"

```

> 注意：当使用HOST:CONTAINER格式来映射端口时，如果你使用的容器端口小于60你可能会得到错误得结果，因为YAML将会解析xx:yy这种数字格式为60进制。所以建议采用字符串格式。

## 19\. security_opt

为每个容器覆盖默认的标签。简单说来就是管理全部服务的标签。比如设置全部服务的user标签值为USER。

```
security_opt:
  - label:user:USER
  - label:role:ROLE

```

## 20\. stop_signal

设置另一个信号来停止容器。在默认情况下使用的是SIGTERM停止容器。设置另一个信号可以使用stop_signal标签。

```
stop_signal: SIGUSR1

```

## 21\. volumes

挂载一个目录或者一个已存在的数据卷容器，可以直接使用 [HOST:CONTAINER] 这样的格式，或者使用 [HOST:CONTAINER:ro] 这样的格式，后者对于容器来说，数据卷是只读的，这样可以有效保护宿主机的文件系统。
Compose的数据卷指定路径可以是相对路径，使用 . 或者 .. 来指定相对目录。
数据卷的格式可以是下面多种形式：

```
volumes:
  // 只是指定一个路径，Docker 会自动在创建一个数据卷（这个路径是容器内部的）。
  - /var/lib/mysql

  // 使用绝对路径挂载数据卷
  - /opt/data:/var/lib/mysql

  // 以 Compose 配置文件为中心的相对路径作为数据卷挂载到容器。
  - ./cache:/tmp/cache

  // 使用用户的相对路径（~/ 表示的目录是 /home/<用户目录>/ 或者 /root/）。
  - ~/configs:/etc/configs/:ro

  // 已经存在的命名的数据卷。
  - datavolume:/var/lib/mysql

```

如果你不使用宿主机的路径，你可以指定一个volume_driver。

```
volume_driver: mydriver

```

## 22\. volumes_from

从其它容器或者服务挂载数据卷，可选的参数是 :ro或者 :rw，前者表示容器只读，后者表示容器对数据卷是可读可写的。默认情况下是可读可写的。

```
volumes_from:
  - service_name
  - service_name:ro
  - container:container_name
  - container:container_name:rw

```

## 23\. cap_add, cap_drop

添加或删除容器的内核功能。详细信息在前面容器章节有讲解，此处不再赘述。

```
cap_add:
  - ALL

cap_drop:
  - NET_ADMIN
  - SYS_ADMIN

```

## 24\. cgroup_parent

指定一个容器的父级cgroup。

```
cgroup_parent: m-executor-abcd

```

## 25\. devices

设备映射列表。与Docker client的--device参数类似。

```
devices:
  - "/dev/ttyUSB0:/dev/ttyUSB0"

```

## 26\. extends

这个标签可以扩展另一个服务，扩展内容可以是来自在当前文件，也可以是来自其他文件，相同服务的情况下，后来者会有选择地覆盖原有配置。

```
extends:
  file: common.yml
  service: webapp

```

用户可以在任何地方使用这个标签，只要标签内容包含file和service两个值就可以了。file的值可以是相对或者绝对路径，如果不指定file的值，那么Compose会读取当前YML文件的信息。
更多的操作细节在后面的12.3.4小节有介绍。

## 27\. network_mode

网络模式，与Docker client的--net参数类似，只是相对多了一个service:[service name] 的格式。
例如：

```
network_mode: "bridge"
network_mode: "host"
network_mode: "none"
network_mode: "service:[service name]"
network_mode: "container:[container name/id]"

```

可以指定使用服务或者容器的网络。

## 28\. networks

加入指定网络，格式如下：

```
services:
  some-service:
    networks:
     - some-network
     - other-network

```

关于这个标签还有一个特别的子标签aliases，这是一个用来设置服务别名的标签，例如：

```
services:
  some-service:
    networks:
      some-network:
        aliases:
         - alias1
         - alias3
      other-network:
        aliases:
         - alias2

```

相同的服务可以在不同的网络有不同的别名。

## 29\. 其它

还有这些标签：cpu_shares, cpu_quota, cpuset, domainname, hostname, ipc, mac_address, mem_limit, memswap_limit, privileged, read_only, restart, shm_size, stdin_open, tty, user, working_dir
上面这些都是一个单值的标签，类似于使用docker run的效果。

```
cpu_shares: 73
cpu_quota: 50000
cpuset: 0,1

user: postgresql
working_dir: /code

domainname: foo.com
hostname: foo
ipc: host
mac_address: 02:42:ac:11:65:43

mem_limit: 1000000000
memswap_limit: 2000000000
privileged: true

restart: always

read_only: true
shm_size: 64M
stdin_open: true
tty: true

```
## 实际运用
- 安装docker-compose
- 编写 docker-compose.yml 配置
- docker-compose up 运行


为快速部署测试、开发环境，决定用docker来管理常用的开发服务环境，并用docker-compose来进行组织。现已完成如下服务的docker化配置，并配置常规服务的一键启动脚本：

*   activeMQ
*   fastdfs简易版
*   mysql
*   redis单机版
*   redis单机版集群
*   solr
*   transmission：一个linux下BT下载工具
*   wordpress：著名开源博客框架
*   zk_cluster: zookeeper单机版集群
*   zookeeper

