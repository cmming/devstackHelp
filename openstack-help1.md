切换 root

vi /etc/ssh/sshd_config
PermitRootLogin yes
重启ssh服务

修改网络
address 192.168.0.183
netmask 255.255.255.0
gateway 192.168.0.2
dns-nameservers 192.168.0.2

auto enp0s8 
iface enp0s8 inet manual

auto enp0s9
iface enp0s9 inet manual

修改阿里

cp /etc/apt/sources.list /etc/apt/sources.list.bak

vi /etc/apt/sources.list

deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse

apt-get update

下载源码
git clone https://git.openstack.org/openstack-dev/devstack -b stable/ocata


创建stack用户apt-get install python-pip

安装pip
apt-get install python-pip
pip -V
修改pip源
 在root和stack用户底下分别 配置pip源


 在主目录下创建.pip文件夹

mkdir ~/.pip

然后在该目录下创建pip.conf文件编写如下内容：

[global]
trusted-host =  pypi.douban.com
index-url = http://pypi.douban.com/simple


su - stack
mkdir ~/.pip

然后在该目录下创建pip.conf文件编写如下内容：

[global]
trusted-host =  pypi.douban.com
index-url = http://pypi.douban.com/simple


迁移文件
mv devstack /opt/stack
修改文件的权限
chown -R stack:stack /opt/stack/devstack


配置文件
local.conf

[[local|localrc]]

MULTI_HOST=true
HOST_IP=192.168.0.183 # management & api network
LOGFILE=/opt/stack/logs/stack.sh.log

# Credentials
ADMIN_PASSWORD=admin
MYSQL_PASSWORD=secret
RABBIT_PASSWORD=secret
SERVICE_PASSWORD=secret
SERVICE_TOKEN=abcdefghijklmnopqrstuvwxyz

# enable neutron-ml2-vlan
disable_service n-net
enable_service q-svc,q-agt,q-dhcp,q-l3,q-meta,neutron,q-lbaas,q-fwaas,q-vpn
Q_AGENT=linuxbridge
ENABLE_TENANT_VLANS=True
TENANT_VLAN_RANGE=3001:4000
PHYSICAL_NETWORK=default

LOG_COLOR=False
LOGDIR=$DEST/logs
SCREEN_LOGDIR=$LOGDIR/screen

# use TryStack git mirror
GIT_BASE=http://git.trystack.cn
NOVNC_REPO=http://git.trystack.cn/kanaka/noVNC.git
SPICE_REPO=http://git.trystack.cn/git/spice/spice-html5.git

#控制分支版本
HORIZON_BRANCH=stable/ocata
KEYSTONE_BRANCH=stable/ocata
NOVA_BRANCH=stable/ocata
NEUTRON_BRANCH=stable/ocata
GLANCE_BRANCH=stable/ocata
CINDER_BRANCH=stable/ocata