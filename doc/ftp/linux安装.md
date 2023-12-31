# 离线vsftpd安装配置

- centos7

## 下载rpm安装包

- 在外网机器上

```sh
yum install --downloadonly --downloaddir=/root/rpms vsftpd
```

## 安装

- 上传到目标机器上执行 yum install ...

## 配置

- 配置虚拟用户

```shell
vim /etc/vsftpd/login_user.txt
```

写入：

```
username1
password123
```

- 生成虚拟用户数据文件

```sh
db_load -T -t hash -f /etc/vsftpd/login_user.txt /etc/vsftpd/login_user.db
```

- 

```conf
# Example config file /etc/vsftpd/vsftpd.conf
#
# The default compiled in settings are fairly paranoid. This sample file
# loosens things up a bit, to make the ftp daemon more usable.
# Please see vsftpd.conf.5 for all compiled in defaults.
#
# READ THIS: This example file is NOT an exhaustive list of vsftpd options.
# Please read the vsftpd.conf.5 manual page to get a full idea of vsftpd's
# capabilities.
#
# Allow anonymous FTP? (Beware - allowed by default if you comment this out).
anonymous_enable=NO
#
# Uncomment this to allow local users to log in.
# When SELinux is enforcing check for SE bool ftp_home_dir
local_enable=YES
#
# Uncomment this to enable any form of FTP write command.
write_enable=YES
#
# Default umask for local users is 077. You may wish to change this to 022,
# if your users expect that (022 is used by most other ftpd's)
local_umask=022
#
# Uncomment this to allow the anonymous FTP user to upload files. This only
# has an effect if the above global write enable is activated. Also, you will
# obviously need to create a directory writable by the FTP user.
# When SELinux is enforcing check for SE bool allow_ftpd_anon_write, allow_ftpd_full_access
#anon_upload_enable=YES
#
# Uncomment this if you want the anonymous FTP user to be able to create
# new directories.
#anon_mkdir_write_enable=YES
#
# Activate directory messages - messages given to remote users when they
# go into a certain directory.
dirmessage_enable=YES
#
# Activate logging of uploads/downloads.
xferlog_enable=YES
#
# Make sure PORT transfer connections originate from port 20 (ftp-data).
# 修改主动模式的端口号60225
connect_from_port_20=YES
ftp_data_port=60225
#
# If you want, you can arrange for uploaded anonymous files to be owned by
# a different user. Note! Using "root" for uploaded files is not
# recommended!
#chown_uploads=YES
#chown_username=whoever
#
# You may override where the log file goes if you like. The default is shown
# below.
#xferlog_file=/var/log/xferlog
#
# If you want, you can have your log file in standard ftpd xferlog format.
# Note that the default log file location is /var/log/xferlog in this case.
xferlog_std_format=YES
#
# You may change the default value for timing out an idle session.
#idle_session_timeout=600
#
# You may change the default value for timing out a data connection.
#data_connection_timeout=120
#
# It is recommended that you define on your system a unique user which the
# ftp server can use as a totally isolated and unprivileged user.
#nopriv_user=ftpsecure
#
# Enable this and the server will recognise asynchronous ABOR requests. Not
# recommended for security (the code is non-trivial). Not enabling it,
# however, may confuse older FTP clients.
#async_abor_enable=YES
#
# By default the server will pretend to allow ASCII mode but in fact ignore
# the request. Turn on the below options to have the server actually do ASCII
# mangling on files when in ASCII mode. The vsftpd.conf(5) man page explains
# the behaviour when these options are disabled.
# Beware that on some FTP servers, ASCII support allows a denial of service
# attack (DoS) via the command "SIZE /big/file" in ASCII mode. vsftpd
# predicted this attack and has always been safe, reporting the size of the
# raw file.
# ASCII mangling is a horrible feature of the protocol.
#ascii_upload_enable=YES
#ascii_download_enable=YES
#
# You may fully customise the login banner string:
#ftpd_banner=Welcome to blah FTP service.
#
# You may specify a file of disallowed anonymous e-mail addresses. Apparently
# useful for combatting certain DoS attacks.
#deny_email_enable=YES
# (default follows)
#banned_email_file=/etc/vsftpd/banned_emails
#
# You may specify an explicit list of local users to chroot() to their home
# directory. If chroot_local_user is YES, then this list becomes a list of
# users to NOT chroot().
# (Warning! chroot'ing can be very dangerous. If using chroot, make sure that
# the user does not have write access to the top level directory within the
# chroot)
chroot_local_user=YES
#chroot_list_enable=YES
# (default follows)
#chroot_list_file=/etc/vsftpd/chroot_list
#
# You may activate the "-R" option to the builtin ls. This is disabled by
# default to avoid remote users being able to cause excessive I/O on large
# sites. However, some broken FTP clients such as "ncftp" and "mirror" assume
# the presence of the "-R" option, so there is a strong case for enabling it.
#ls_recurse_enable=YES
#
# When "listen" directive is enabled, vsftpd runs in standalone mode and
# listens on IPv4 sockets. This directive cannot be used in conjunction
# with the listen_ipv6 directive.
listen=NO
#
# This directive enables listening on IPv6 sockets. By default, listening
# on the IPv6 "any" address (::) will accept connections from both IPv6
# and IPv4 clients. It is not necessary to listen on *both* IPv4 and IPv6
# sockets. If you want that (perhaps because you want to listen on specific
# addresses) then you must run two copies of vsftpd with two configuration
# files.
# Make sure, that one of the listen options is commented !!
listen_ipv6=YES
reverse_lookup_enable=NO

pam_service_name=vsftpd_virtual
userlist_enable=YES
userlist_file=/etc/vsftpd/user_list
tcp_wrappers=YES

# 激活虚拟用户
guest_enable=YES
guest_username=ftp
user_config_dir=/etc/vsftpd/virtual_users_conf.d

virtual_use_local_privs=YES
user_sub_token=$USER
# 设置ftp虚拟用户根目录
local_root=/opt/ftp/$USER
hide_ids=YES
allow_writeable_chroot=YES
file_open_mode=777

listen_port=2112
pasv_enable=YES
#pasv_address=44.144.5.100
#pasv_addr_resolve=YES
# 被动模式的端口
pasv_min_port=10201
pasv_max_port=10299
pasv_promiscuous=NO
#ftpd_banner=welcome to 44.144.5.100 FTP service

```

- 创建配置认证文件

```sh
vim /etc/pam.d/vsftpd_virtual
```

改成：

```conf
auth    sufficient      /lib64/security/pam_userdb.so    db=/etc/vsftpd/login_user
account sufficient      /lib64/security/pam_userdb.so    db=/etc/vsftpd/login_user
session    required     pam_loginuid.so
```

- 建立数据目录

```sh
mkdir -p /data/www/virtual/
mkdir -p /data/www/virtual/username1 # 建立用户1的目录
chown ftp:ftp /data/www/virtual/username1 # 编辑目录权限
```

每次添加用户就需要重复`配置虚拟用户` -> `生成虚拟用户数据文件` -> `建立数据目录`

## 启动

```sh
systemctl start vsftpd
```