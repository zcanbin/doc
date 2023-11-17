# linux下编译安装redis

## 下载

最新版链接：https://download.redis.io/redis-stable.tar.gz

## 编译

```shell
# 解压
tar -zxvf redis-5.0.8.tar.gz
cd redis-5.0.8
# 编译安装到家目录下的redis目录
make PREFIX=~/redis install
```