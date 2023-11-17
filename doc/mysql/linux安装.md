# linux下安装mysql8

## 下载

链接：[MySQL :: Download MySQL Community Server](https://dev.mysql.com/downloads/mysql/)

选择与系统对应的版本下载

![image-20231116171904748](img\image-20231116171904748.png)

## 安装

```shell
# 解压
tar -xvf mysql-8.2.0-linux-glibc2.17-x86_64.tar.xz
cd mysql-8.2.0-linux-glibc2.17-x86_64
# 创建目录
mkdir data log run
# 修改my.cnf
vi my.conf
```

