# Brainstorm #1 - Browsers


CENTOS 7

# PACOTES

curl -O http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
curl -O http://rpms.remirepo.net/enterprise/remi-release-7.rpm
curl -O https://repo.zabbix.com/zabbix/5.0/rhel/7/x86_64/zabbix-release-5.0-1.el7.noarch.rpm
curl -O https://packages.erlang-solutions.com/erlang-solutions-2.0-1.noarch.rpm
curl -O https://packages.erlang-solutions.com/rpm/erlang_solutions.asc
curl -O http://www.webmin.com/jcameron-key.asc
curl -sL https://rpm.nodesource.com/setup_14.x | sudo -E bash -
curl -sL https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
rpm --import https://packages.erlang-solutions.com/rpm/erlang_solutions.asc
rpm --import http://www.webmin.com/jcameron-key.asc
yum -y install *.rpm && yum -y install yum-utils && yum-config-manager --enable remi && yum-config-manager --enable epel && yum-config-manager --enable zabbix && yum install -y yarn && yum -y install wget vim-enhanced zip unzip htop iotop tcpdump telnet links nfs-utils erlang zabbix-agent

# NODE|YARN|YUM-UTILS

yum install -y yarn yum-utils

# ENABLING REPOS

yum-config-manager --enable remi
yum-config-manager --enable epel
yum-config-manager --enable zabbix
yum-config-manager --enable remi-php74

# UPGRADE CENTOS 7

yum -y install net-snmp

yum -y install wget vim-enhanced zip unzip htop iotop tcpdump telnet links nfs-utils erlang zabbix-agent

yum -y install htop iotop rsync links dkms make geoip-devel libmaxminddb-devel openssl-devel tokyocabinet-devel goaccess tcpdump erlang bzip2 memcached telnet links nfs-utils autoconf ftp curl vim-enhanced zip unzip htop iotop rsync traceroute &

yum -y upgrade

yum -y install wget gcc gcc-c++ zlib-devel libxml2-devel openssl-devel curl-devel freetype-devel gd-devel libjpeg-devel libpng-devel python-devel openldap-devel libmcrypt-devel bzip2-devel kernel-devel kernel-headers bzip2 memcached telnet links nfs-utils autoconf ftp curl vim-enhanced zip unzip htop iotop rsync ntsysv nss curl socat dkms make perl net-tools perl-Digest-MD5 lvm2 git nvme-cli syslog-ng cifs-utils openssl098e zabbix-agent nodejs yarn ncurses ncurses-devel geoip-devel libmaxminddb-devel openssl-devel tokyocabinet-devel goaccess tcpdump erlang

yum -y install samba samba-client samba-common

##DISABLE SELINUX

sed -i 's/^SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config

##MARIADB 10

vim /etc/yum.repos.d/MariaDB.repo

# MariaDB 10.5 CentOS repository list - created 2020-09-16 19:42 UTC
# http://downloads.mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.5/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1

yum -y install MariaDB-client
yum -y install MariaDB-server

systemctl enable mariadb
systemctl start mariadb

##WEBMIN

echo "[Webmin]
name=Webmin Distribution Neutral
#baseurl=https://download.webmin.com/download/yum
mirrorlist=https://download.webmin.com/download/yum/mirrorlist
enabled=1
" > /etc/yum.repos.d/webmin.repo

yum -y install webmin

sed -i 's/ssl=1/ssl=0/' /etc/webmin/miniserv.conf

sed -i 's/^ssl=.*/ssl=0/g' /etc/webmin/miniserv.conf

192.168.4.4:/infra/ /infra/ nfs rw,sync,hard,intr 0 0

/etc/init.d/webmin stop
systemctl enable webmin
systemctl start webmin

##REBUILD YUM DB
rm -f /var/lib/rpm/__*
rm -f /var/lib/rpm/.rpm*
rpm --rebuilddb -v -v
yum clean all
yum makecache fast

##FIREWALL CONFIGURATION

# That's for Oracle
firewall-cmd --permanent --add-port=1521/tcp --zone=public
firewall-cmd --permanent --add-port=5500/tcp --zone=public
firewall-cmd --permanent --add-port=5520/tcp --zone=public
firewall-cmd --permanent --add-port=3938/tcp --zone=public

# That's for Webmin
firewall-cmd --permanent --add-port=10000/tcp --zone=public

# That's for PostgreSQL
firewall-cmd --permanent --add-port=5432/tcp --zone=public

# That's for Samba
firewall-cmd --permanent --add-port=137/tcp --zone=public;
firewall-cmd --permanent --add-port=138/tcp --zone=public;
firewall-cmd --permanent --add-port=139/tcp --zone=public;
firewall-cmd --permanent --add-port=445/tcp --zone=public;
firewall-cmd --permanent --add-port=901/tcp --zone=public;
firewall-cmd --permanent --add-port=514/tcp --zone=public;
firewall-cmd --permanent --add-port=514/udp --zone=public;
firewall-cmd --permanent --add-service=samba --zone=public;

# That's for Apache
firewall-cmd --permanent --add-service=http --zone=public;
firewall-cmd --permanent --add-service=https --zone=public;

# That's for MySQL
firewall-cmd --permanent --zone=trusted --add-source=192.168.0.0/21
firewall-cmd --permanent --zone=trusted --add-port=3306/tcp
firewall-cmd --permanent --zone=public --add-port=3306/tcp

# That's for Operfire
firewall-cmd --zone=public --add-port=9090/udp --permanent
firewall-cmd --zone=public --add-port=9090/tcp --permanent 
firewall-cmd --zone=public --add-port=9091/udp --permanent
firewall-cmd --zone=public --add-port=9091/tcp --permanent
firewall-cmd --zone=public --add-port=5222/udp --permanent
firewall-cmd --zone=public --add-port=5222/tcp --permanent
firewall-cmd --zone=public --add-port=5223/udp --permanent
firewall-cmd --zone=public --add-port=5223/tcp --permanent

# That's for NFS
firewall-cmd --permanent --add-port=111/tcp
firewall-cmd --permanent --add-port=54302/tcp
firewall-cmd --permanent --add-port=20048/tcp
firewall-cmd --permanent --add-port=2049/tcp
firewall-cmd --permanent --add-port=46666/tcp
firewall-cmd --permanent --add-port=42955/tcp
firewall-cmd --permanent --add-port=875/tcp

firewall-cmd --add-service={http,https} --permanent
firewall-cmd --add-port={10051/tcp,10050/tcp} --permanent

firewall-cmd --reload

usermod –aG wheel UserName

On Linux systems either add
/usr/local/pgsql/bin/pg_ctl start -l logfile -D /usr/local/pgsql/data
to /etc/rc.d/rc.local or /etc/rc.local
or look at the file
contrib/start-scripts/linux in the PostgreSQL source distribution.

##NAS

yum -y install nfs-utils

systemctl enable rpcbind nfs-server nfs-lock nfs-idmap
systemctl start rpcbind nfs-server nfs-lock nfs-idmap

mkdir /infra
mkdir /backup
chmod 777 /infra/
chmod 777 /backup/

vim /etc/exports

/infra/		192.168.0.0/21(rw,sync,no_root_squash,no_all_squash)
/backup/	192.168.0.0/21(rw,sync,no_root_squash,no_all_squash)

exportfs -r

exportfs

systemctl restart nfs-server

192.168.5.9:/infra  /infra  nfs rw,sync,hard,intr 0 0
192.168.5.9:/backup /backup nfs rw,sync,hard,intr 0 0

net use Y: /delete
mount -o mtype=hard \\192.168.5.9\infra Y: /persistent:no
mount -o mtype=hard \\192.168.5.9\backup X: /persistent:no

rw – Writable permission to shared folder
sync – Synchronize shared directory
no_root_squash – Enable root privilege
no_all_squash - Enable user’s authority
