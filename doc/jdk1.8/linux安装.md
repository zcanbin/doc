# linux下安装JDK1.8详细步骤

## 下载

由于商用版权问题，推荐下载 jdk-8u202 版本

链接：[Java Archive Downloads - Java SE 8 (oracle.com)](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html)

## 安装

使用应用的账号登录

上传文件到家目录

```shell
#解压文件
tar zxvf jdk-8u202-linux-x64.tar.gz
```

```
#修改[.bash_profile]# 新增以下内容
vi .bash_profile
```

```shell
#jdk
if [ -z ${JAVA_HOME} ]; then
	export JAVA_HOME=${HOME}/jdk1.8.0_202
	export JRE_HOME=${JAVA_HOME}/jre
	export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
	export PATH=${JAVA_HOME}/bin:${PATH}
fi
```

```shell
#执行生效环境变量
source .bash_profile
#验证
java -version
```

