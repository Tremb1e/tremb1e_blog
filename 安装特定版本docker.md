# 安装特定版本docker

可参考官方文档 [https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/ "在 Ubuntu 上开始使用 Docker Engine")

本文基于 Ubuntu-20.04

### 卸载已安装的 docker , docker.io 或者是 docker-engine 

<pre>
$ sudo apt-get remove docker docker-engine docker.io containerd runc
</pre>

## 1.从 docekr 存储库中安装

### 设置存储库

#### 更新 apt 包索引并安装包以允许 apt 通过 HTTPS 使用存储库

<pre>
$ sudo apt-get update
</pre>


<pre>
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release 
</pre>

#### 添加 Docker 官方的 GPG 密钥

<pre>
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
</pre>

#### 使用以下命令设置稳定存储库

##### x86_64 / amd64
<pre>
$ echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
</pre>

##### armhf

<pre>
echo \
  "deb [arch=armhf signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
</pre>

##### arm64

<pre>
$ echo \
  "deb [arch=arm64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
</pre>

##### s390x

<pre>
$ echo \
  "deb [arch=s390x signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
</pre>

### 安装 Docker 引擎

#### 更新 apt 包索引，安装最新版本的 Docker Engine 和 containerd ，或者到下一步安装特定版本

<pre>
$ sudo apt-get update
</pre>

<pre>
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
</pre>

#### 要安装特定版本的 Docker Engine，请在 repo 中列出可用版本，然后选择并安装

##### 列出您的存储库中可用的版本

<pre>
$ apt-cache madison docker-ce
</pre>

<pre>
$ root@tremb1e:~# apt-cache madison docker-ce
 docker-ce | 5:20.10.8~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.7~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.6~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.5~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.4~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.3~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.2~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.1~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.0~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.15~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.14~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.13~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.12~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.11~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.10~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.9~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
</pre>

##### 安装显示的特定版本的 docker ，例如 5:20.10.8~3-0~ubuntu-focal 

<pre>
$ sudo apt-get install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io
</pre>

#### 通过运行 hello-world 映像检测 docker 是否安装成功

<pre>
$ sudo docker run hello-world
</pre>

<pre>
$ root@tremb1e:~# sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
b8dfde127a29: Pull complete 
Digest: sha256:0fe98d7debd9049c50b597ef1f85b7c1e8cc81f59c8d623fcb2250e8bec85b38
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
</pre>

正确运行，安装成功。

## 2.下载 deb 包并手动安装（可在离线环境下使用）

### 去 [https://download.docker.com/linux/ubuntu/dists/](https://download.docker.com/linux/ubuntu/dists/)选择你的Ubuntu版本，然后浏览pool/stable/，选择amd64， armhf，arm64，或s390x，并下载 .deb 文件要安装的 docker 版本

在 [https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/](https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/)目录下，包含以下文件，挑选相应版本下载

<pre>
../
containerd.io_1.2.0-1_amd64.deb                                                       2020-07-28 16:28:05 19.0 MiB
containerd.io_1.2.0~beta.2-1_amd64.deb                                                2020-07-28 16:28:05 20.0 MiB
containerd.io_1.2.0~rc.0-1_amd64.deb                                                  2020-07-28 16:28:05 18.9 MiB
containerd.io_1.2.0~rc.2-1_amd64.deb                                                  2020-07-28 16:28:06 18.9 MiB
containerd.io_1.2.10-2_amd64.deb                                                      2020-07-28 16:28:06 19.1 MiB
containerd.io_1.2.10-3_amd64.deb                                                      2020-07-28 16:28:07 19.1 MiB
containerd.io_1.2.12-1_amd64.deb                                                      2020-07-28 16:28:07 19.2 MiB
containerd.io_1.2.13-1_amd64.deb                                                      2020-07-28 16:28:08 19.2 MiB
containerd.io_1.2.13-2_amd64.deb                                                      2020-07-28 16:28:08 20.4 MiB
containerd.io_1.2.2-1_amd64.deb                                                       2020-07-28 16:28:09 19.0 MiB
containerd.io_1.2.2-3_amd64.deb                                                       2020-07-28 16:28:09 19.0 MiB
containerd.io_1.2.4-1_amd64.deb                                                       2020-07-28 16:28:09 19.0 MiB
containerd.io_1.2.5-1_amd64.deb                                                       2020-07-28 16:28:10 19.0 MiB
containerd.io_1.2.6-3_amd64.deb                                                       2020-07-28 16:28:10 21.6 MiB
containerd.io_1.3.7-1_amd64.deb                                                       2020-10-08 20:39:06 23.2 MiB
containerd.io_1.3.9-1_amd64.deb                                                       2020-11-30 22:57:50 23.2 MiB
containerd.io_1.4.3-1_amd64.deb                                                       2020-12-09 01:54:59 26.8 MiB
containerd.io_1.4.3-2_amd64.deb                                                       2021-04-08 00:35:40 27.0 MiB
containerd.io_1.4.4-1_amd64.deb                                                       2021-04-08 00:35:41 26.9 MiB
containerd.io_1.4.6-1_amd64.deb                                                       2021-06-14 15:59:51 27.0 MiB
containerd.io_1.4.8-1_amd64.deb                                                       2021-07-30 20:13:40 23.6 MiB
containerd.io_1.4.9-1_amd64.deb                                                       2021-07-30 20:13:40 23.6 MiB
docker-ce-cli_18.09.0~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:11 12.5 MiB
docker-ce-cli_18.09.1~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:11 12.5 MiB
docker-ce-cli_18.09.2~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:12 12.5 MiB
docker-ce-cli_18.09.3~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:12 12.5 MiB
docker-ce-cli_18.09.4~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:13 12.5 MiB
docker-ce-cli_18.09.5~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:14 12.6 MiB
docker-ce-cli_18.09.6~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:14 12.5 MiB
docker-ce-cli_18.09.7~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:15 12.6 MiB
docker-ce-cli_18.09.8~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:15 12.6 MiB
docker-ce-cli_18.09.9~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:16 13.9 MiB
docker-ce-cli_19.03.0~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:16 40.5 MiB
docker-ce-cli_19.03.10~3-0~ubuntu-bionic_amd64.deb                                    2020-07-28 16:28:17 39.3 MiB
docker-ce-cli_19.03.11~3-0~ubuntu-bionic_amd64.deb                                    2020-07-28 16:28:18 39.3 MiB
docker-ce-cli_19.03.12~3-0~ubuntu-bionic_amd64.deb                                    2020-07-28 16:28:19 39.3 MiB
docker-ce-cli_19.03.13~3-0~ubuntu-bionic_amd64.deb                                    2020-10-08 20:39:06 42.1 MiB
docker-ce-cli_19.03.14~3-0~ubuntu-bionic_amd64.deb                                    2020-12-08 16:25:07 42.1 MiB
docker-ce-cli_19.03.15~3-0~ubuntu-bionic_amd64.deb                                    2021-02-25 07:28:16 42.1 MiB
docker-ce-cli_19.03.1~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:20 40.5 MiB
docker-ce-cli_19.03.2~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:20 40.5 MiB
docker-ce-cli_19.03.3~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:22 40.5 MiB
docker-ce-cli_19.03.4~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:23 40.5 MiB
docker-ce-cli_19.03.5~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:24 40.5 MiB
docker-ce-cli_19.03.6~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:24 40.6 MiB
docker-ce-cli_19.03.7~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:25 40.6 MiB
docker-ce-cli_19.03.8~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:26 40.6 MiB
docker-ce-cli_19.03.9~3-0~ubuntu-bionic_amd64.deb                                     2020-07-28 16:28:27 39.3 MiB
docker-ce-cli_20.10.0~3-0~ubuntu-bionic_amd64.deb                                     2020-12-09 01:55:00 37.2 MiB
docker-ce-cli_20.10.1~3-0~ubuntu-bionic_amd64.deb                                     2021-02-25 07:28:17 39.5 MiB
docker-ce-cli_20.10.2~3-0~ubuntu-bionic_amd64.deb                                     2021-02-25 07:28:22 39.5 MiB
docker-ce-cli_20.10.3~3-0~ubuntu-bionic_amd64.deb                                     2021-02-25 07:28:22 39.5 MiB
docker-ce-cli_20.10.4~3-0~ubuntu-bionic_amd64.deb                                     2021-02-26 15:21:17 39.5 MiB
docker-ce-cli_20.10.5~3-0~ubuntu-bionic_amd64.deb                                     2021-04-08 00:32:15 39.5 MiB
docker-ce-cli_20.10.6~3-0~ubuntu-bionic_amd64.deb                                     2021-04-12 11:34:16 39.5 MiB
docker-ce-cli_20.10.7~3-0~ubuntu-bionic_amd64.deb                                     2021-06-14 15:54:32 39.5 MiB
docker-ce-cli_20.10.8~3-0~ubuntu-bionic_amd64.deb                                     2021-08-03 23:49:12 37.0 MiB
docker-ce-rootless-extras_20.10.0~3-0~ubuntu-bionic_amd64.deb                         2020-12-09 01:55:02 8.5 MiB
docker-ce-rootless-extras_20.10.1~3-0~ubuntu-bionic_amd64.deb                         2021-02-25 07:28:23 8.5 MiB
docker-ce-rootless-extras_20.10.2~3-0~ubuntu-bionic_amd64.deb                         2021-02-25 07:28:24 8.5 MiB
docker-ce-rootless-extras_20.10.3~3-0~ubuntu-bionic_amd64.deb                         2021-02-25 07:28:24 8.5 MiB
docker-ce-rootless-extras_20.10.4~3-0~ubuntu-bionic_amd64.deb                         2021-02-26 15:21:20 8.5 MiB
docker-ce-rootless-extras_20.10.5~3-0~ubuntu-bionic_amd64.deb                         2021-04-08 00:32:16 8.5 MiB
docker-ce-rootless-extras_20.10.6~3-0~ubuntu-bionic_amd64.deb                         2021-04-12 11:34:17 8.6 MiB
docker-ce-rootless-extras_20.10.7~3-0~ubuntu-bionic_amd64.deb                         2021-06-14 15:54:33 8.6 MiB
docker-ce-rootless-extras_20.10.8~3-0~ubuntu-bionic_amd64.deb                         2021-08-03 23:49:13 7.5 MiB
docker-ce_18.03.1~ce~3-0~ubuntu_amd64.deb                                             2020-07-28 16:28:27 32.3 MiB
docker-ce_18.06.0~ce~3-0~ubuntu_amd64.deb                                             2020-07-28 16:28:28 38.3 MiB
docker-ce_18.06.1~ce~3-0~ubuntu_amd64.deb                                             2020-07-28 16:28:29 38.4 MiB
docker-ce_18.06.2~ce~3-0~ubuntu_amd64.deb                                             2020-07-28 16:28:29 38.3 MiB
docker-ce_18.06.3~ce~3-0~ubuntu_amd64.deb                                             2020-07-28 16:28:30 38.4 MiB
docker-ce_18.09.0~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:31 16.6 MiB
docker-ce_18.09.1~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:31 16.6 MiB
docker-ce_18.09.2~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:32 16.6 MiB
docker-ce_18.09.3~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:32 16.6 MiB
docker-ce_18.09.4~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:33 16.6 MiB
docker-ce_18.09.5~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:34 16.6 MiB
docker-ce_18.09.6~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:34 16.6 MiB
docker-ce_18.09.7~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:35 16.6 MiB
docker-ce_18.09.8~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:35 16.6 MiB
docker-ce_18.09.9~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:36 19.1 MiB
docker-ce_19.03.0~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:36 21.6 MiB
docker-ce_19.03.10~3-0~ubuntu-bionic_amd64.deb                                        2020-07-28 16:28:37 21.5 MiB
docker-ce_19.03.11~3-0~ubuntu-bionic_amd64.deb                                        2020-07-28 16:28:37 21.5 MiB
docker-ce_19.03.12~3-0~ubuntu-bionic_amd64.deb                                        2020-07-28 16:28:38 21.5 MiB
docker-ce_19.03.13~3-0~ubuntu-bionic_amd64.deb                                        2020-10-08 20:39:07 21.5 MiB
docker-ce_19.03.14~3-0~ubuntu-bionic_amd64.deb                                        2020-12-08 16:25:08 21.7 MiB
docker-ce_19.03.15~3-0~ubuntu-bionic_amd64.deb                                        2021-02-25 07:28:24 21.7 MiB
docker-ce_19.03.1~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:39 21.6 MiB
docker-ce_19.03.2~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:40 21.7 MiB
docker-ce_19.03.3~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:40 21.8 MiB
docker-ce_19.03.4~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:40 21.8 MiB
docker-ce_19.03.5~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:41 21.8 MiB
docker-ce_19.03.6~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:42 21.8 MiB
docker-ce_19.03.7~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:42 21.8 MiB
docker-ce_19.03.8~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:43 21.8 MiB
docker-ce_19.03.9~3-0~ubuntu-bionic_amd64.deb                                         2020-07-28 16:28:43 21.4 MiB
docker-ce_20.10.0~3-0~ubuntu-bionic_amd64.deb                                         2020-12-09 01:55:03 23.6 MiB
docker-ce_20.10.1~3-0~ubuntu-bionic_amd64.deb                                         2021-02-25 07:28:25 23.6 MiB
docker-ce_20.10.2~3-0~ubuntu-bionic_amd64.deb                                         2021-02-25 07:28:26 23.6 MiB
docker-ce_20.10.3~3-0~ubuntu-bionic_amd64.deb                                         2021-02-25 07:28:26 23.6 MiB
docker-ce_20.10.4~3-0~ubuntu-bionic_amd64.deb                                         2021-02-26 15:21:20 23.7 MiB
docker-ce_20.10.5~3-0~ubuntu-bionic_amd64.deb                                         2021-04-08 00:32:17 23.7 MiB
docker-ce_20.10.6~3-0~ubuntu-bionic_amd64.deb                                         2021-04-12 11:34:18 23.7 MiB
docker-ce_20.10.7~3-0~ubuntu-bionic_amd64.deb                                         2021-06-14 15:54:33 23.7 MiB
docker-ce_20.10.8~3-0~ubuntu-bionic_amd64.deb                                         2021-08-03 23:49:14 20.2 MiB
docker-scan-plugin_0.7.0~ubuntu-bionic_amd64.deb                                      2021-04-12 11:34:19 3.7 MiB
docker-scan-plugin_0.8.0~ubuntu-bionic_amd64.deb                                      2021-06-14 15:54:34 3.7 MiB
</pre>

### 安装 Docker Engine，将下面的路径更改为下载 Docker 包的路径

<pre>
$ sudo dpkg -i /path/to/package.deb
</pre>

### 通过运行 hello-world 映像验证 Docker Engine 是否已正确安装

<pre>
$ sudo docker run hello-world
</pre>

## 3.自动化脚本安装

### 提示

Docker 在 [https://get.docker.com/](https://get.docker.com/) 上提供了一个方便的脚本，可以快速且非交互地将 Docker 安装到开发环境中。不建议将便捷脚本用于生产环境，但可以用作示例来创建适合您需求的配置脚本。另请参阅使用存储库安装 步骤以了解使用软件包存储库进行安装的安装步骤。该脚本的源代码是开源的，可以在 GitHub 上的 [docker-install 存储库](https://github.com/docker/docker-install)中找到。

在本地运行之前，请务必检查从 Internet 下载的脚本。在安装之前，让自己熟悉便利脚本的潜在风险和限制：

+ 脚本需要root或sudo特权才能运行。

+ 该脚本尝试检测您的 Linux 发行版和版本并为您配置包管理系统，并且不允许您自定义大多数安装参数。

+ 该脚本无需确认即可安装依赖项和建议。这可能会安装大量软件包，具体取决于主机的当前配置。

+ 默认情况下，该脚本会安装 Docker、containerd 和 runc 的最新稳定版本。使用此脚本配置机器时，可能会导致 Docker 的主要版本意外升级。在部署到生产系统之前，始终在测试环境中测试（主要）升级。

+ 该脚本并非旨在升级现有的 Docker 安装。使用脚本更新现有安装时，依赖项可能不会更新到预期版本，从而导致使用过时的版本。

#### 可以运行带有DRY_RUN=1选项的脚本以了解脚本在安装过程中将执行的步骤

<pre>
$ curl -fsSL https://get.docker.com -o get-docker.sh
</pre>

<pre>
$ DRY_RUN=1 sh ./get-docker.sh
</pre>


<pre>
$ root@tremb1e:~# sudo ./get-docker.sh
# Executing docker install script, commit: 0e685c6ac0bddd7b2ba7bcaaeb519746ad249a29
<...>
</pre>


## 4.源码安装

### 本文使用 **docker-18.09.0** 版本作为示例

### 下载 docker 对应版本的离线安装包

[Github链接：https://github.com/docker/docker-ce/releases](https://github.com/docker/docker-ce/releases/ "docker源码下载地址")

### 新建文件 docker.service ，填入以下内容

<pre>
$ [Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
#BindsTo=containerd.service.  — 无需containerd。
After=network-online.target firewalld.service
Wants=network-online.target

[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd -H unix://
ExecReload=/bin/kill -s HUP $MAINPID
TimeoutSec=0
RestartSec=2
Restart=always

# Note that StartLimit* options were moved from "Service" to "Unit" in systemd 229.
# Both the old, and new location are accepted by systemd 229 and up, so using the old location
# to make them work for either version of systemd.
StartLimitBurst=3

# Note that StartLimitInterval was renamed to StartLimitIntervalSec in systemd 230.
# Both the old, and new name are accepted by systemd 230 and up, so using the old name to make
# this option work for either version of systemd.
StartLimitInterval=60s

# Having non-zero Limit*s causes performance problems due to accounting overhead
# in the kernel. We recommend using cgroups to do container-local accounting.
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity

# Comment TasksMax if your systemd version does not supports it.
# Only systemd 226 and above support this option.
TasksMax=infinity

# set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes

# kill only the docker process, not all processes in the cgroup
KillMode=process

[Install]
WantedBy=multi-user.target
</pre>

### 新建shell脚本，命名为 docker.sh 

<pre>
#：
echo " 1. unzip the docker bin."
tar zxvf docker-18.09.0.tgz
echo " 2. move docker* into /usr/bin "
cp docker/* /usr/bin/
cp docker.service /usr/lib/systemd/system/docker.service
echo " 3. docker install successfully !, check docker info to see the result."
</pre>

### 给 docker.sh 运行权限

<pre>
$ chmod a+x docker.sh
</pre>

### 运行 docker.sh 
<pre>
$ ./docker.sh
</pre>

<pre>
$ root@tremb1e:~# ./docker.sh 
 1. unzip the docker bin.
docker/
docker/ctr
docker/containerd-shim
docker/containerd
docker/docker-proxy
docker/docker
docker/dockerd
docker/runc
docker/docker-init
 2. move docker* into /usr/bin 
 3. docker install successfully !, check docker info to see the result.
</pre>

### 查看 docker 版本信息

<pre>
$ docker version
</pre>

<pre>
$ root@tremb1e:~# docker version
Client: Docker Engine - Community
 Version:           18.09.0
 API version:       1.39
 Go version:        go1.10.4
 Git commit:        4d60db4
 Built:             Wed Nov  7 00:46:51 2018
 OS/Arch:           linux/amd64
 Experimental:      false
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

</pre>

### 设置 docker 开机自启动

<pre>
$ systemctl enable docker.service
</pre>

<pre>
$ root@tremb1e:~# systemctl enable docker.service
Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /lib/systemd/system/docker.service.
</pre>

## 5.安装 docker-compose 

### 运行此命令以下载 Docker Compose 的当前稳定版本(要安装不同版本的 Compose，请替换1.29.2 为你要使用的 Compose 版本)

<pre>
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
</pre>

### 对二进制文件应用可执行权限

<pre>
$ sudo chmod +x /usr/local/bin/docker-compose
</pre>

### 如果 docker-compose 安装后命令失败，请检查您的路径。您还可以 /usr/bin 在路径中创建指向或任何其他目录的符号链接

<pre>
$ sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
</pre>

### 查看 docker-compose 版本信息

<pre>
$ root@tremb1e:~# docker-compose version
</pre>

<pre>
$ root@tremb1e:~# docker-compose version
docker-compose version 1.29.2, build 5becea4c
docker-py version: 5.0.0
CPython version: 3.7.10
OpenSSL version: OpenSSL 1.1.0l  10 Sep 2019
</pre>