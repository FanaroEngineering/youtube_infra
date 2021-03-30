# Brainstorm #1 - Browsers


CENTOS 7

##PACOTES

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

##NODE|YARN|YUM-UTILS

yum install -y yarn yum-utils

##ENABLING REPOS

yum-config-manager --enable remi
yum-config-manager --enable epel
yum-config-manager --enable zabbix
yum-config-manager --enable remi-php74

##UPGRADE CENTOS 7

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

##PGSQL

PGPASSWORD=Pz4rkWw+q@C8FXafQvMG /usr/local/pgsql/bin/reindexdb -U zUser_AWS -h rds01.zanthusonline.com.br -a -e -v

PGPASSWORD=Pz4rkWw+q@C8FXafQvMG /usr/local/pgsql/bin/vacuumdb -U zUser_AWS -h rds01.zanthusonline.com.br -a -z -F -f -v

PGPASSWORD=Pz4rkWw+q@C8FXafQvMG /usr/local/pgsql/bin/pg_dump -v -U zUser_AWS -h rds01.zanthusonline.com.br -d SAAS_PrimeiroPreco -F c -b -f /backup/z-backup-manual/SAAS_PrimeiroPreco-no-sales-data.dump.backup --exclude-table-data 'zan_m*' --exclude-table-data 'zan_pedido*' --exclude-table-data 'tab_evento_lote_nfe' --exclude-table-data 'tab_geracao_arquivos' --exclude-table-data 'zan_1tra' --exclude-table-data 'tab_eventos_nfe' --exclude-table-data 'zan_endereco_pedido' --exclude-table-data 'tab_logfile' --exclude-table-data 'tab_carga_loja' --exclude-table-data 'tab_premio_concedido' --exclude-table-data 'tab_inutilizacao_nfe' --exclude-table-data 'tab_controle_nfe' --exclude-table-data 'tab_notificacao' --exclude-table-data 'tab_controle_lote_nfe' --exclude-table-data 'tab_integracao_generica' --exclude-table-data 'tab_nota*' --exclude-table-data 'tab_fluxo_financeiro' --exclude-table-data 'tab_envio_arquivos_fiscais' --exclude-table-data 'tab_mov_tes_saldo_cx_fin' --exclude-table-data 'tab_carga_*' --exclude-table-data 'tab_fechamento' --exclude-table-data 'tab_movimento_tesouraria' --exclude-table-data 'zan_arquivos_pdv' --exclude-table-data 'tab_reembolso_cliente' --exclude-table-data 'tab_usuarios_logados' --exclude-table-data 'tab_registro_indevido_*' --exclude-table-data 'tab_reembolso_cliente' --exclude-table-data 'tab_troca*'

PGPASSWORD=H4zfUFX0 /usr/local/pgsql/bin/pg_restore -v -U zUser_AWS -h rds02.zanthusonline.com.br -d PRIME_GPA /backup/z-backup-manual/SAAS_PrimeiroPreco-no-sales-data.dump.backup

PGPASSWORD=H4zfUFX0 /usr/local/pgsql/bin/pg_dump -v -U zUser_AWS -h rds02.zanthusonline.com.br -d SAAS_Supribem -F c -b -f /backup/z-backup-manual/SAAS_Supribem-no-sales-data.dump.backup --exclude-table-data 'zan_m*' --exclude-table-data 'zan_pedido*' --exclude-table-data 'tab_evento_lote_nfe' --exclude-table-data 'tab_geracao_arquivos' --exclude-table-data 'zan_1tra' --exclude-table-data 'tab_eventos_nfe' --exclude-table-data 'zan_endereco_pedido' --exclude-table-data 'tab_logfile' --exclude-table-data 'tab_carga_loja' --exclude-table-data 'tab_premio_concedido' --exclude-table-data 'tab_inutilizacao_nfe' --exclude-table-data 'tab_controle_nfe' --exclude-table-data 'tab_notificacao' --exclude-table-data 'tab_controle_lote_nfe' --exclude-table-data 'tab_integracao_generica' --exclude-table-data 'tab_nota*' --exclude-table-data 'tab_fluxo_financeiro' --exclude-table-data 'tab_envio_arquivos_fiscais' --exclude-table-data 'tab_mov_tes_saldo_cx_fin' --exclude-table-data 'tab_carga_*' --exclude-table-data 'tab_fechamento' --exclude-table-data 'tab_movimento_tesouraria' --exclude-table-data 'zan_arquivos_pdv' --exclude-table-data 'tab_reembolso_cliente' --exclude-table-data 'tab_usuarios_logados' --exclude-table-data 'tab_registro_indevido_*' --exclude-table-data 'tab_reembolso_cliente' --exclude-table-data 'tab_troca*'

##SED UTILS

sed -i 's/^POSTGRES-PRD/localhost/g' /utils/bash/backupDatabase.sh
sed -i 's/ZSAASEBS/ebs01/setup/ZSAAS/stats/'

##DISK

file -s /dev/sdb1 | gawk '{print $8}' >> /etc/fstab

##PVCREATE

pvcreate /dev/nvme1n1

pvs

vgcreate manager /dev/nvme1n1

lvcreate -n infra -L 10240M vg2_geek
lvcreate -n backup -L 102400M vg2_geek
lvcreate -n web -L 102400M vg2_geek
lvcreate -n web -l 100%FREE manager 

pvs

lvs

mkfs -t ext4 /dev/vg2_geek/web 
mkfs -t ext4 /dev/vg2_geek/data 
mkfs -t ext4 /dev/vg2_geek/infra
mkfs -t ext4 /dev/vg2_geek/backup
 
blkid /dev/vg2_geek/web
blkid /dev/vg2_geek/infra
blkid /dev/vg2_geek/data
blkid /dev/vg2_geek/backup

mkdir /data /web /infra /backup

vim /etc/fstab

cp -arpv /etc/fstab /etc/fstab.backup

file -s /dev/sdb1 | gawk '{print $8}'

mount -a

chcon -t samba_share_t /web
chcon -t samba_share_t /transfer

mkdir -p /backup /backup/restore

cd /
ln -sf /infra/configs
ln -sf /infra/pacotes
ln -sf /infra/primeshare
ln -sf /infra/tmpz
ln -sf /infra/utils

Setup RAR for Linux x64

wget https://www.rarlab.com/rar/rarlinux-x64-5.5.0.tar.gz
tar xzvf rarlinux-x64-5.5.0.tar.gz
mv -fv rar/ /usr/local/
cd /usr/bin/
ln -sf /usr/local/rar/rar
ln -sf /usr/local/rar/unrar

mkdir -p /var/www/html/itop/conf
mkdir -p /var/www/html/itop/data
mkdir -p /var/www/html/itop/env-production
mkdir -p /var/www/html/itop/log
chmod -fR 777 /var/www/html/itop/conf/
chmod -fR 777 /var/www/html/itop/data
chmod -fR 777 /var/www/html/itop/env-production/
chmod -fR 777 /var/www/html/itop/log

##POSTGRES-PRD

PGPASSWORD=H4zfUFX0 /usr/local/pgsql/bin/pg_dump -v -U zUser_AWS -h rds02.zanthusonline.com.br -d PRIME_GPA -F c -b -f /backup/z-backup-manual/PRIME_GPA-no-sales-data.dump.backup --exclude-table-data 'zan_m*' --exclude-table-data 'zan_pedido*' --exclude-table-data 'tab_evento_lote_nfe' --exclude-table-data 'tab_geracao_arquivos' --exclude-table-data 'zan_1tra' --exclude-table-data 'tab_eventos_nfe' --exclude-table-data 'zan_endereco_pedido' --exclude-table-data 'tab_logfile' --exclude-table-data 'tab_carga_loja' --exclude-table-data 'tab_premio_concedido' --exclude-table-data 'tab_nota*' --exclude-table-data 'tab_inutilizacao_nfe'

PGPASSWORD=H4zfUFX0 /usr/local/pgsql/bin/pg_restore -v -U zUser_AWS -h rds02.zanthusonline.com.br -d PRIME_GPA /backup/z-backup-manual/PRIME_GPA-no-sales-data.dump.backup

select schemaname as table_schema,
    relname as table_name,
    pg_size_pretty(pg_total_relation_size(relid)) as total_size,
    pg_size_pretty(pg_relation_size(relid)) as data_size,
    pg_size_pretty(pg_total_relation_size(relid) - pg_relation_size(relid))
      as external_size
from pg_catalog.pg_statio_user_tables
order by pg_total_relation_size(relid) desc,
         pg_relation_size(relid) desc
limit 50;

#DATE=`date '+%Y%m%d'`
#HORA=`date '+%H%M'`
#DATE_ANT=`date -d "-7 day" '+%Y%m%d'`

##OLD

yum -y install gcc gcc-c++ zlib-devel libxml2-devel openssl-devel curl-devel freetype-devel gd-devel libjpeg-devel libpng-devel python-devel openldap-devel libmcrypt-devel bzip2-devel kernel-devel kernel-headers bzip2 memcached telnet links nfs-utils autoconf ftp wget vim-enhanced zip unzip htop iotop rsync ntsysv nss curl socat dkms make perl net-tools perl-Digest-MD5 lvm2 git nvme-cli syslog-ng cifs-utils openssl098e perl-Net-SSLeay perl-Crypt-SSLeay samba samba-client samba-common
