---
title: "快速入门"
permalink: /docs/quick-start-guide/
excerpt: "如何利用JavaChassis迅速创建微服务."
last_modified_at: 2017-06-06T10:01:43-04:00
redirect_from:
  - /theme-setup/
---

这个快速入门指南会提供详细的指令，帮助您迅速创建微服务。

{% include toc %}

## 前提
以下软件需要被安装:


1. JDK 1.8
2. Docker 17.03.01-ce
3. Maven 3.5.0 


## 安装

通过Github下载Java Chassis源码并Build:

```bash
> git clone https://github.com/ServiceComb/java-chassis.git
> cd java-chassis
> mvn clean install
```

## 自动测试
构建并运行docker镜像，同时进行集成测试使用一下命令:


```bash
> cd java-chassis
> mvn clean install -Pdocker
```

如果您机器安装的是docker machine则使用以下命令:

```bash
> cd java-chassis
> mvn clean install -Pdocker -Pdocker-machine
```

## 运行Demo

拿demo-jaxrs工作做例子，运行demo-jaxrs-server的docker镜像

```bash
> cd java-chassis/demo/demo-jaxrs/jaxrs-client
> mvn clean install -Pdocker -Ddocker.keepRunning=true
```

运行后查看运行的镜像

```bash
> docker ps

```

会看到有以下的镜像正在运行当中

<figure>
  <img src="{{ '/assets/images/docker-runtime.png' | absolute_url }}" alt="docker运行时的容器">
  <figcaption>jaxrs-server:0.1.0-m2-SNAPSHOT和seanyinx/sc两个容器在运行.</figcaption>
</figure>

根据图片可以看到容器seanyinx/sc的端口是*32771*

如果您使用的是docker machine，你还需要通过以下的指令获取docker machina运行的IP

```bash
> docker-machine ip

```

同时进入文件夹*demo/demo-jaxrs/jaxrs-client/src/main/resources*下的microservice.yaml文件

```yaml
APPLICATION_ID: jaxrstest
service_description:
  name: jaxrsClient
  version: 0.0.1
cse:
  service:
    registry:
      address: http://IP:PORT
  handler:
    chain:
      Consumer:
        default: bizkeeper-consumer,loadbalance
  references:
    jaxrs:
      version-rule: 0.0.1
```

如果您安装的是原生docker，IP为 127.0.0.1，如果安装的是docker machine则IP为通过`docker-machine ip`指令获取的IP。同时端口则通过`docker ps`指令获取到容器seanyinx/sc映射的端口为32771

运行JaxrsClient的main函数则能看到client成功调用了微服务。