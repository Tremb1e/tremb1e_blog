# 如何正确编译GmSSL

### GmSSL项目地址[http://gmssl.org/](http://gmssl.org/)

### 编译要求：强烈建议使用Centos7（其他Linux版本或发行版可能会出错）


## 1.下载源代码，解压至当前工作目录
```
wget https://github.com/guanzhi/GmSSL/archive/master.zip
```

```
unzip GmSSL-master.zip
```

## 2.编译与安装

### 先预装需要的软件

```
yum install make perl gcc
```

### 开始进行编译和安装

```
cd GmSSL-master
```

```
./config
```

```
make
```

```
sudo make install
```

```
sudo cp libcrypto.so.1.1 libssl.so.1.1 /lib64
```

## 3.检查是否成功

### 终端中输入命令

```
gmssl
```

### 当出现以下命令行时即编译安装成功

```
GmSSL>
```
