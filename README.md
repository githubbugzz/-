# -
SSR 梯子
今天试图用python requests模块加载shadowsocks代理，发现根本不支持，所以python想用vpn还必须得搭建Socks5。

之前因为懒，看网上得很多教程相当繁琐就懒得搞了，没想到今天还是免不了这个劫，所以这篇文章是个备忘也算是个总结。

毕竟sock5 毕竟我们毕竟只是用来作为工具，用来开发或者渗透，更或者看一些不可描述得东西，所以，搭建必须得快，在这方面耽误太长时间就毫无意义了。

socks5下载
下载地址：http://sourceforge.net/projects/ss5/files/

安装

tar zxvf ss5-3.6.4-3.tar.gz
cd ss5-3.6.4
./configure //默认是1080端口，如果想改端口的话，./configure --with-defaultport=10800
make
make install
默认安装目录在：/etc/opt/ss5

若出现报错：

configure: error: *** Some of the headers weren't found ***
则 需要安装：

yum -y install pam-devel

如果出现报错：
SS5OpenLdap.c:29:18: fatal error: ldap.h: No such file or directory
安装：
yum -y install openldap-devel
修改配置
之前本指望不改那些繁琐的配置，直接就用，事实证明ss5日志会报错：

"" ISERROR - - - (-:- -- -:-) (Socks method unknown or bad request)
所以还是需要改配置：

/etc/opt/ss5/ss5.conf内容全部删除，仅添加内容：

auth 0.0.0.0/0 - u
permit u 0.0.0.0/0 - 0.0.0.0/0 - - - - -
/etc/opt/ss5/ss5.passwd中添加用户名与密码：

一行一个账号与密码：



另外，默认装好后ss5的服务程序是没有执行权限的，需要添加权限：

chmod -R 777 /etc/init.d/ss5

注意：如果你服务器开了防火墙不要忘了关掉，或者iptables里做下策略

配置完后

