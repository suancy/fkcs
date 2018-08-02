# A Docker Flavored Way for Anti-Censorship
[中文在这里](./README_ZH.md)
## Simple Diagram for Seasoned Hackers
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
This is a pretty *dumb* solution, you can understand it at a first glance. Configurations are all packaged in your image, and the container can update itself when the you modify and push updated images.

## Want to be a service provider?
Check [how to use this repo](./How_to_use_this_repo.md), for a brief instruction on how to setup and maintain.

## **Instruction for Users**
## Intro
For years we've been enduring omnipresent censorship and almost murderous latencies. The wall that distancing us from the world and replacing meaningful human creativities with useless craps of <a href="https://medium.com/@liruyi/bye-bye-wechat-688148f53dfd" title="if one can’t link to the web, one will have to duplicate the web - Lawrence Li on Bye-bye WeChat">pilferage</a> is at still evolution. What can we do?

## Preliminaries
### OS requirements
- You need a **Unix-like** operation system or a **Windows 10** that is either Pro or Enterprise version 14393 or a **Windows server 2016 RTM**. The solution possibly won't work on any other systems such as iOS or Android.
- You have enough disk space for *Docker* (introduced below).

### Terminologies & Skills
- You understand what is a **directory**.
- And you know what is called a **proxy** including some knowing of basic steps to setup proxy for your applications or the whole system.
- You are aware that human and a computer can communicate through a **command line**, and you are able to open up a **terminal** or a **PowerShell** window.

Otherwise you won't success or you "don't need" this solution for now. (Although I am sure that you are in terrible needs of this.)

## Features
This solution comprises of two containerized applications, one for establishing network connection and the other one is for **self-updating**. Containerized applications are shipped as docker images, and run inside containers created by docker.

Once setup, the user will not have to update new cores or check configurations. The provider should do this when pushing new images. And pushed new images will be shipped to the user automatically.

## Defects
Though docker images are commonly small, docker engine itself is quite **cumbersome** for the simple objective of crossing the wall. The installer of docker already requires more than 500MB disk space on Windows.

To learn some docker skills and get familiar with this versatile development tool is highly recommended. Hope you can manage its powerfulness!

## Instructions
**If you want to use the my servers**, please contact me (*suancaiyu_at_protonmail.com*) to get the necessary `docker-compose.yml`.

### Install and configure Docker on Linux
On Linux systems the setup is simple, I will take Ubuntu as an example. (For a full instruction, please refer the [official docs of docker](https://docs.docker.com/install/))

1. Install the community edition of docker along with docker-compose:
```shell
$ sudo apt update && sudo apt upgrade -y && sudo install -y docker docker-compose
```

2. Modify your permission to communicate with docker daemon, this step is optional:
```shell
$ sudo usermod -aG docker $USER
```
After you modified your permission, **logout and login** the session for changes taken into effect.

3. If you decide not to modify your user group due to security concerns, you need to use a `sudo` before all docker commands such as `docker` or `docker-compose`. Or if you don't mind **the danger of messing up your system**, you can switch to the root account:
```shell
$ sudo -s
```

4. Now add a mirror server to speedup your image pulling (Assuming you're in China):
```shell
$ sudo printf '{\n\t"registry-mirrors": ["https://registry.docker-cn.com"]\n}\n' > /etc/docker/daemon.json
```

5. Restart docker daemon to load the mirror settings:
```shell
$ sudo systemctl restart docker
```

### Install and configure Docker on Mac
1. You may download docker for mac [here](https://store.docker.com/editions/community/docker-ce-desktop-mac). After the installation process, you can configure registry mirrors through a friendly GUI.

2. Click the **little whale** on your system tray and choose Preferences. Then click the Daemon, and add `https://docker.mirrors.ustc.edu.cn/` to the Registry mirrors.

3. Apply & Restart docker! And it's done.

You can get started with docker from the [official docs](https://docs.docker.com/docker-for-mac/).

### Install and configure Docker on Windows
1. You may download the installer [here](https://store.docker.com/editions/community/docker-ce-desktop-windows). Similar to Docker for Mac, Docker for Windows comes with a friendly GUI, just follow the instruction of the Installer.

2. After installation, you can see a little whale on your system tray. Right-click it, and click Settings > Daemon, add `https://docker.mirrors.ustc.edu.cn/` to the Registry mirrors.

3. Apply and we're done.

You can get started with docker from the [official docs](https://docs.docker.com/docker-for-windows/).

### Deploy the solution *FKCS*

*FKCS* is an acronym for *Fuck Censor*.


1. Create a directory, name it whatever you like:

  *Linux or Mac*
  ```shell
  $ mkdir fkcs
  ```

  *Windows PowerShell*
  ```shell
  > mkdir fkcs
  ```

2. Move the `docker-compose.yml` file to that directory &mdash; `fkcs` in my case &mdash; and change the working directory to it with the following command:

  *Linux or Mac*
  ```shell
  $ cd fkcs
  ```

  *Windows PowerShell*
  ```shell
  > cd fkcs
  ```
3. Check twice that you've moved `docker-compose.yml` to the directory, and deploy the solution:

  *Linux or Mac*
  ```shell
  $ docker-compose up
  ```

  *Windows PowerShell*
  ```shell
  > docker-compose up
  ```

After it automatically setups itself, you're finally able to access the Internet. The default `socks5` proxy is on port `1095` and `http` is on `9999`.

To stop or restart it, run `docker-compose stop` or `docker-compose restart` **under the directory you've created just now** (`fkcs` in my case).
## Future perspectives
- [ ] Deploy a standalone client side app, with the auto-update ability (And may prompt for an auto-update). Maybe a new repo for that.

## For my precious supporters
I work as a barista in China. If you want to support this project, *buy yourself a cup of coffee from me*!

If I happened to be unreachable to you, there are still other ways to support this project including:
- keep learning
- keep thinking
- be warm-hearted
- and do something MEANINGFUL now.

May the wall won't stand long against a civilized population!

## Credits
- [Project V](https://github.com/v2ray/v2ray-core): A platform for building proxies to bypass network restrictions. https://www.v2ray.com/
- [Watchtower](https://github.com/v2tec/watchtower): Automatically update running Docker containers
- [接近风的地方 - 使用 Nginx 对 Project V 做负载均衡](https://ellinia.me/Load-balancing-project-v-by-nginx/)
