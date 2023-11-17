### 下载文件到同一目录

```shell
mkdir ~/software
cd ~/software
wget https://www.openssl.org/source/openssl-3.1.1.tar.gz
wget http://zlib.net/zlib-1.2.13.tar.gz
wget https://github.com/PCRE2Project/pcre2/releases/download/pcre2-10.42/pcre2-10.42.tar.gz
wget http://nginx.org/download/nginx-1.24.0.tar.gz
```

### 解压文件

```shell
tar -zxvf openssl-3.1.1.tar.gz
tar -zxvf zlib-1.2.13.tar.gz
tar -zxvf pcre2-10.42.tar.gz
tar -zxvf nginx-1.24.0.tar.gz
```

### 编译

```shell
# 进入nginx目录
cd nginx-1.24.0
# 生成配置
./configure \
--with-cc-opt='-O2' \
--prefix=.. \
--with-compat \
--with-debug \
--with-pcre-jit \
--with-http_ssl_module \
--with-http_stub_status_module \
--with-http_realip_module \
--with-http_auth_request_module \
--with-http_v2_module \
--with-http_dav_module \
--with-http_slice_module \
--with-threads \
--with-http_addition_module \
--with-http_gunzip_module \
--with-http_gzip_static_module \
--with-http_sub_module
--with-stream \
--with-stream_ssl_module \
--with-http_secure_link_module
--with-http_random_index_module
--with-http_flv_module
--with-http_mp4_module
--with-file-aio

# 编译
make -j4

# 安装
make install
```



### 整理nginx包

```shell
mkdir ~/nginx
cd ~/nginx
mv ~/software/conf ./
mv ~/software/html ./
mv ~/software/logs ./
mv ~/software/sbin ./

cd sbin
# 将二进制nginx重命名为nginx_bin, 
mv nginx nginx_bin
# 新建shell脚本nginx, 用与实现编译时prefix==..相对路径，导致在其他的（非nginx/sbin）路径运行nginx, 报错配置文件路径错误的问题
vim nginx
chmod +x nginx
```

### nginx脚本内容

```shell
#! /bin/sh
# 脚本所在目录(nginx_bin所在目录)
BIN_PATH=$(dirname $(readlink -f $0))
cd $BIN_PATH
# $* 表示脚本接收到的所有参数， 全部传递给nginx_bin
./nginx_bin $*
```

### 生成nginx.tar.gz压缩包

```shell
cd ~
tar -zcvf nginx.tar.gz nginx
```

