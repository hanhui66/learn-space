####此实验以 CentOS 7.2 x64 的系统为环境，搭建 git 服务器
###1.安装
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
中间需要你确认一下(y/n)
###2.安装编译工具
yum install gcc perl-ExtUtils-MakeMaker
中间需要你确认一下(y/n)
###3.下载git
(1)选一个目录，用来放下载下来的安装包，这里将安装包放在 /usr/local/src 目录里
cd /usr/local/src
(2)到git官网找一个新版稳定的源码包下载到 /usr/local/src 文件夹里
wget https://www.kernel.org/pub/software/scm/git/git-2.10.0.tar.gz
###4.解压和编译
(1)解压下载的源码包
tar -zvxf git-2.10.0.tar.gz
(2)解压后进入 git-2.10.0 文件夹
cd git-2.10.0
(3)执行编译
make all prefix=/usr/local/git
(4)编译完成后, 安装到 /usr/local/git 目录下
make install prefix=/usr/local/git
###5.配置环境变量
(1)将 git 目录加入 PATH,将原来的 PATH 指向目录修改为现在的目录
echo 'export PATH=$PATH:/usr/local/git/bin' >> /etc/bashrc
(2)使环境变量生效
source /etc/bashrc
(3)此时我们能查看 git 版本号，说明我们已经安装成功了。
git --version
###6.创建git账号
(1)为我们刚刚搭建好的 git 创建一个账号
useradd -m gituser
(2)然后为这个账号设置密码
passwd gituser
使用这个命令后,需要设置密码(好像需要8位以上的密码,可以根据提示更改),输入两次,两次前后要一致
###7.初始化 git 仓库并配置用户权限
(1)我们创建 /data/repositories 目录用于存放 git 仓库
mkdir -p /data/repositories
(2)创建好后，初始化这个仓库
cd /data/repositories/ && git init --bare test.git
###8.配置用户权限
chown -R gituser:gituser /data/repositories
chmod 755 /data/repositories
查找 git-shell 所在目录(如果是按照上面的步骤,这个位置应该是 /usr/local/git/bin/git-shell, 否则请通过 whereis git-shell 命令查看位置),
编辑 /etc/passwd 文件，将最后一行关于 gituser 的登录 shell 配置改为 git-shell 的目录
最后一行改成gituser:x:500:500::/home/gituser:/usr/local/git/bin/git-shell
###9.使用
克隆项目test repo 到本地
cd ~ && git clone gituser@139.199.223.159:/data/repositories/test.git
需要前面设置的密码