# Windows下安装JDK1.8详细步骤

## 下载

由于商用版权问题，推荐下载 jdk-8u202 版本

链接：[Java Archive Downloads - Java SE 8 (oracle.com)](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html)

## 安装

双击jdk1.8安装包进行安装：
![在这里插入图片描述](img\8df565332633421285a88512eac60855.png)
 **点击下一步（这里安装在D盘，根据自己需要进行修改即可）：**
 ![在这里插入图片描述](img\ff2ddcfd7d6c4294b22ebfcc4720b8e9.png)
 **点击下一步，进入安装状态，等待安装完成：**
 ![在这里插入图片描述](img\332efa592fa34c54bc36a3991b563cd8.png)

 **上面安装完成后继续安装jre-1.8：**
 ![在这里插入图片描述](img\2da0f0cebee94d349401709424bd3ed4.png)
 **点击下一步进入安装状态：**
 ![在这里插入图片描述](img\9458c8032d91483087d38484304da601.png)
 **安装结束：**
 ![在这里插入图片描述](img\881d9957de724f93a6e81c819b48bab5.png)

## 配置

配置环境变量步骤如下：

定位属性：此电脑-->属性-->高级系统设置-->环境变量-->系统变量-->新建



步骤一：

新建环境变量JAVA_HOME
变量名：JAVA_HOME
变量值（就是刚刚JDK的安装路径）：D:\Program Files\Java\jdk-1.8

![在这里插入图片描述](img\84ba0f6ce1a84725aeb22d3a9d1c1db7.png)

操作完成点击确定后可以看到新建成功：

步骤二：
在系统变量中新建系统变量：CLASSPATH
变量值为：.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar


即：

![在这里插入图片描述](img\5f9c284ea6a74d468d2ae9ae52c2dae5.png)

步骤三：配置PATH路径：

依然实在系统变量区域中找到PATH变量值，然后双击进行编辑：

然后点击新建按钮：

输入：%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin

![在这里插入图片描述](img\c43d0b7d02df42fa9538a6ba0b25f8cf.png)

千万不要忘记点击确认把所有的界面全部退出去。

最后来验证是否安装成功：
快捷键win+R，然后输入cmd。
输入：java -version

![在这里插入图片描述](img\e33d3905fb384f2681d5eb2a8442cc5b.png)

输入：javac

![在这里插入图片描述](img\e842d4dc575b494c973739ead2ac19b8.png)
