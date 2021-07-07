# docker容器和镜像的停止和删除
## 1.列出所有docker镜像
```
docker images
```

![docker images](https://macrz-wordpress.oss-cn-beijing.aliyuncs.com/2021-07-06%3Adocker%E5%AE%B9%E5%99%A8%E5%92%8C%E9%95%9C%E5%83%8F%E7%9A%84%E5%81%9C%E6%AD%A2%E5%92%8C%E5%88%A0%E9%99%A4/docker%20images.png)

- repository：存储库
- tag：用于版本控制
- image id：镜像的ID
- created：创建时间
- size：镜像大小

存储库和镜像ID分析

（1）repository-存储库：此时为dockerhub中的nginx官方仓库，若为私有仓库，格式一般为demo.harbor.com/demo/nginx:tag

```
docker login --username=$username $url
```

登陆仓库，并输入密码

```
docker pull $image_url
```
从仓库中拉取镜像

```
docker images
```

查看镜像列表

![私有镜像格式](https://macrz-wordpress.oss-cn-beijing.aliyuncs.com/2021-07-06%3Adocker%E5%AE%B9%E5%99%A8%E5%92%8C%E9%95%9C%E5%83%8F%E7%9A%84%E5%81%9C%E6%AD%A2%E5%92%8C%E5%88%A0%E9%99%A4/%E7%A7%81%E6%9C%89%E9%95%9C%E5%83%8F%E6%A0%BC%E5%BC%8F.png)

（2）image id-镜像的ID：镜像ID唯一的表示一个镜像，ID值是根据该镜像的数据配置文件使用sha256算法计算获得。文件存放在 /var/lib/docker/image/overlay2/imagedb/content/sha256 目录中。
![image id存放位置](https://macrz-wordpress.oss-cn-beijing.aliyuncs.com/2021-07-06%3Adocker%E5%AE%B9%E5%99%A8%E5%92%8C%E9%95%9C%E5%83%8F%E7%9A%84%E5%81%9C%E6%AD%A2%E5%92%8C%E5%88%A0%E9%99%A4/image%20id%E5%AD%98%E6%94%BE%E4%BD%8D%E7%BD%AE.png)

与上图两个images的image id对比一致。

打开第一个nginx的文件查看

```
cat 4f380adfc10f4cd34f775ae57a17d2835385efd5251d6dfe0f246b0018fb0399 | python -m json.tool
```

![nginx sha256文件](https://macrz-wordpress.oss-cn-beijing.aliyuncs.com/2021-07-06%3Adocker%E5%AE%B9%E5%99%A8%E5%92%8C%E9%95%9C%E5%83%8F%E7%9A%84%E5%81%9C%E6%AD%A2%E5%92%8C%E5%88%A0%E9%99%A4/nginx%20sha256%E6%96%87%E4%BB%B6.png)

可以看出确实是配置文件

我们再将此文件的sha256值计算出来

```
sha256sum 4f380adfc10f4cd34f775ae57a17d2835385efd5251d6dfe0f246b0018fb0399
```

![image配置文件的sha256值计算](https://macrz-wordpress.oss-cn-beijing.aliyuncs.com/2021-07-06%3Adocker%E5%AE%B9%E5%99%A8%E5%92%8C%E9%95%9C%E5%83%8F%E7%9A%84%E5%81%9C%E6%AD%A2%E5%92%8C%E5%88%A0%E9%99%A4/image%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E7%9A%84sha256%E5%80%BC%E8%AE%A1%E7%AE%97.png)

可以看出这个文件的sha256值等于文件名，也等于image id

## 2.查看正在运行的、或所有的docker容器
1. 查看正在运行的docker容器


```
docker ps
```

2. 查看所有的docker容器

这个命令也会显示未启动的容器信息

```
docker ps -a
```

![显示docker容器](https://macrz-wordpress.oss-cn-beijing.aliyuncs.com/2021-07-06%3Adocker%E5%AE%B9%E5%99%A8%E5%92%8C%E9%95%9C%E5%83%8F%E7%9A%84%E5%81%9C%E6%AD%A2%E5%92%8C%E5%88%A0%E9%99%A4/%E6%98%BE%E7%A4%BAdocker%E5%AE%B9%E5%99%A8.png)

## 3.停止所有容器

```
docker stop $(docker ps -aq)
```

docker stop操作的对象是 container id 而不是 image name 

![docker stop all](https://macrz-wordpress.oss-cn-beijing.aliyuncs.com/2021-07-06%3Adocker%E5%AE%B9%E5%99%A8%E5%92%8C%E9%95%9C%E5%83%8F%E7%9A%84%E5%81%9C%E6%AD%A2%E5%92%8C%E5%88%A0%E9%99%A4/docker%20stop%20all.png)

## 4.删除所有容器

```
docker rm $(docker ps -aq)
```

和停止容器同样， docker rm 操作的对象也是 container id 

![docker delete all container](https://macrz-wordpress.oss-cn-beijing.aliyuncs.com/2021-07-06%3Adocker%E5%AE%B9%E5%99%A8%E5%92%8C%E9%95%9C%E5%83%8F%E7%9A%84%E5%81%9C%E6%AD%A2%E5%92%8C%E5%88%A0%E9%99%A4/docker%20delete%20all%20container.png)

## 5.删除所有镜像

### 通过 image name 删除单个镜像

```
docker image rm $image_name
```

![delete images through image name](https://macrz-wordpress.oss-cn-beijing.aliyuncs.com/2021-07-06%3Adocker%E5%AE%B9%E5%99%A8%E5%92%8C%E9%95%9C%E5%83%8F%E7%9A%84%E5%81%9C%E6%AD%A2%E5%92%8C%E5%88%A0%E9%99%A4/delete%20images%20through%20image%20name.png)

### 通过 image id 删除单个镜像

```
docker rmi $image_id
```

![delete images through image id](https://macrz-wordpress.oss-cn-beijing.aliyuncs.com/2021-07-06%3Adocker%E5%AE%B9%E5%99%A8%E5%92%8C%E9%95%9C%E5%83%8F%E7%9A%84%E5%81%9C%E6%AD%A2%E5%92%8C%E5%88%A0%E9%99%A4/delete%20images%20thtough%20image%20id.png)

### 删除所有镜像

```
docker rmi $(docker images -q)
```

![delete all images](https://macrz-wordpress.oss-cn-beijing.aliyuncs.com/2021-07-06%3Adocker%E5%AE%B9%E5%99%A8%E5%92%8C%E9%95%9C%E5%83%8F%E7%9A%84%E5%81%9C%E6%AD%A2%E5%92%8C%E5%88%A0%E9%99%A4/delete%20all%20images.png)

## 6.删除所有停止的容器

```
docker container prune -f
```

![delete container which doesn't use](https://macrz-wordpress.oss-cn-beijing.aliyuncs.com/2021-07-06%3Adocker%E5%AE%B9%E5%99%A8%E5%92%8C%E9%95%9C%E5%83%8F%E7%9A%84%E5%81%9C%E6%AD%A2%E5%92%8C%E5%88%A0%E9%99%A4/delete%20container%20which%20doesn%27t%20use.png)


## 7.删除所有不使用的镜像

```
docker image prune --force --all 
//或者 
docker image prune -f -a
```

![delete images which doesn't use](https://macrz-wordpress.oss-cn-beijing.aliyuncs.com/2021-07-06%3Adocker%E5%AE%B9%E5%99%A8%E5%92%8C%E9%95%9C%E5%83%8F%E7%9A%84%E5%81%9C%E6%AD%A2%E5%92%8C%E5%88%A0%E9%99%A4/delete%20images%20which%20doesn%27t%20use.png)
