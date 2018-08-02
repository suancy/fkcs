# 如何借 Docker 之力反审查/翻墙
[The English README is here](./README.md)
## *一览*
```
===============================  What's in the docker-compose.yml  ================================

       ___________________________________
      |        nginx:stable-alpine        |
      |                                   |
      |       _____________________       |                             _____________________
      |      |                     |      |      watch for updates     |                     |
      |      |        V2Ray        |      |   <----------------------  |      watchtower     |
      |      |_____________________|      |                            |_____________________|
      |                                   |                                v2tec/watchtower
      |                                   |
      |___________________________________|
                your/image:and_tag


=================================  How V2Ray and NGINX cooperate  =================================


                                                    :----------> dummy.server.com:443
                                     load-balancer  :
            V2Ray --------> NGINX -----------------------------> sweet.server.com:443
                                                    :
                                                    :----------> honey.server.com:443

```
如您所见，此方案并不高明。您只需将配置文件打包编译入 docker 映像，当您更新映像，客户端容器将会自动升级。

## *欲提供突破审查服务？*
关于如何设置服务器或进行维护，请参考 [How to use this repo](./How_to_use_this_repo.md)。

## **客户端指引**
## 简介
「若不作寒蝉，何以享太平？」

何不金蝉脱殼，享沙鸥翔集、锦鳞游泳之自由？

## 预备
### 操作系统
- **类 Unix** ，或专业/企业 14393 版的 **Windows 10**，或 **Windows Server 2016 RTM**。非常遗憾，此客户端无法部署在 iOS 或 Android 上。
- 足够的硬盘空间以安装 *Docker*（介绍见下）。

### 必知术语
- 您了解什么是 **目录**。
- 您知道什么是 **代理**，并且会给您的应用或整个系统设置代理。
- 您明白人类与电脑可以通过 **命令行** 对话，并且您能够开启 **终端** 或 **PowerShell** 窗口。

否则您可能会失败。

## 特点
此客户端由两个容器化应用组成，一个用于建立网络连接，另一个用于 **自动升级**。容器化应用以 docker 映像的形式输送，而运行应用的容器由 docker 创建。

一旦成功部署客户端，用户便不再需要反复修改配置或者升级内核，一切只需由服务提供者完成。服务提供者更新映像文件后，所有的客户端都将自动更新。

## 缺点
尽管 docker 映像通常不大，docker 引擎却会 **占用过多空间**（Windows 10 仅安装器会占用 500 MB 以上的空间）。

但我非常希望您能掌握丰富的 docker 技巧。愿其成为于您得心应手的工具！

## 部署客户端
若您愿使用我的服务器，请邮件联系（*suancaiyu_at_protonmail.com*），以便获取关键文件 `docker-compose.yml`。

### 在 Linux 上安装并配置 Docker
在 Linux 上安装并不难，以 Ubuntu 为例。（您亦可参阅[ Docker 官方文档]((https://docs.docker.com/install/)以获得全面指引）

1. 安装 Docker 社区版以及 docker-compose：
```shell
$ sudo apt update && sudo apt upgrade -y && sudo install -y docker docker-compose
```

2. （此步可跳过）修改您的权限，以便直接与 docker daemon 沟通：
```shell
$ sudo usermod -aG docker $USER
```
然后 **登出（注销）再登录** 您的账户，使您的修改生效。

3. （基于您是否跳过上一步）若您出于安全考虑决定不修改您的权限，您将需要在所有 docker 命令（比如 `docker` 或 `docker-compose`）前添加 `sudo`。若您不介意可能摧毁系统的风险，也可直接切换为 `root` 帐户。
```shell
$ sudo -s
```

4. 添加镜像服务器以加速您的映像拉取（假设您在中国）：
```shell
$ sudo printf '{\n\t"registry-mirrors": ["https://registry.docker-cn.com"]\n}\n' > /etc/docker/daemon.json
```

5. 重启 docker daemon 使得镜像服务器修改生效：
```shell
$ sudo systemctl restart docker
```

### 在 Mac 上安装并配置 Docker
1. 在 [这里](https://store.docker.com/editions/community/docker-ce-desktop-mac) 下载。当安装完成后，您将可以通过一个友好的图形界面管理 docker。

2. 点击出现在系统托盘上的 **小鲸鱼**，点击 Preferences > Daemon，添加 `https://docker.mirrors.ustc.edu.cn/` 到 Registry mirrors。

3. 点击 Apply & Restart 即可完成。

您可以从 [官方文档](https://docs.docker.com/docker-for-mac/) 开始了解 docker。

### 在 Windows 上安装并配置 Docker
1. 从 [这里](https://store.docker.com/editions/community/docker-ce-desktop-windows) 下载安装器。与 Docker for Mac 类似，Docker for Windows 也拥有一个十分友好的图形界面。按照安装器的指引即可完成安装。

2. 安装完成后，您将能在系统托盘上看到一只 **小鲸鱼**。右击它，选择 Settings > Daemon，添加 `https://docker.mirrors.ustc.edu.cn/` 到 Registry mirrors。

3. 点击 Apply 以应用镜像服务器。

您可以从这个 [官方文档](https://docs.docker.com/docker-for-windows/) 开始了解 docker。

## 执行本方案 FKCS
*FKCS* 是 *Fuck Censor* 的缩写。


1. 创建一个目录，您可自定义目录名：

  *Linux or Mac*
  ```shell
  $ mkdir fkcs
  ```

  *Windows PowerShell*
  ```shell
  > mkdir fkcs
  ```

2. 将 `docker-compose.yml` 移动到刚才创建的目录，并用下面的命令切换工作目录：

  *Linux or Mac*
  ```shell
  $ cd fkcs
  ```

  *Windows PowerShell*
  ```shell
  > cd fkcs
  ```
3. 请再次确认 `docker-compose.yml` 在当前目录下，然后执行：

  *Linux or Mac*
  ```shell
  $ docker-compose up
  ```

  *Windows PowerShell*
  ```shell
  > docker-compose up
  ```

当两个容器都启动之后，您将连接上互联网。默认的 `socks5` 代理开在 `1095` 端口，默认 `http` 代理在 `9999` 端口。

若要停止或重启它，在 **上述新建的目录下** （本例中是 `fkcs`）运行 `docker-compose stop` 或 `docker-compose restart`即可。

## 展望
- [ ] 开发一个便于分发的客户端，或许会新开一个 repo。

## 感谢您的支持
我经营着一间咖啡小店，若您认识我并且愿意支持我，您可以 *给自己买一杯我做的咖啡* ！

若不幸我没有与您的一面之缘，您也可以通过下面的方式支持我：
- 努力学习
- 努力思考
- 保持热心
- 做 **有意义** 的事。

愿这堵墙能在你我面前倒塌！

## Credits
- [Project V](https://github.com/v2ray/v2ray-core): A platform for building proxies to bypass network restrictions. https://www.v2ray.com/
- [Watchtower](https://github.com/v2tec/watchtower): Automatically update running Docker containers
- [接近风的地方 - 使用 Nginx 对 Project V 做负载均衡](https://ellinia.me/Load-balancing-project-v-by-nginx/)
