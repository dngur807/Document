echo "CS Version 1.0"


if [ $(id -u) -ne 0 ]; then exec sudo bash "$0" "$@"; fi

echo "Setup Start !!!"


echo "Create Directory!"

mkdir -p /vagrant/server/public_html
mkdir -p /vagrant/server/log
mkdir -p /vagrant/server/log/nginx
mkdir -p /vagrant/server/log/php-fpm
mkdir -p /vagrant/server/log/scribed


echo "Install system packages..."

cd /

#sudo -i

yum -y update
yum -y install wget
yum -y install epel-release
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
wget http://rpms.remirepo.net/enterprise/remi-release-7.rpm
rpm -Uvh remi-release-7.rpm epel-release-latest-7.noarch.rpm

rpm -Uvh  http://dl.fedoraproject.org/pub/epel/7/x86_64/epel-release.noarch-7-12.rpm
rpm -Uvh  http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

#nginx 설치
curl -O http://nginx.org/packages/centos/7/noarch/RPMS/nginx-1.14.0-1.el7_4.ngx.x86_64.rpm
yum localinstall nginx-1.14.0-1.el7_4.ngx.x86_64.rpm

# php 72
yum -y --enablerepo=remi-php72 install php php-cli php-common php-devel php-fpm php-json php-mbstring php-mcrypt php-mysqlnd php-opcache php-pdo php-pear php-pecl-apcu php-pecl-apcu-bc php-pecl-geoip php-pecl-igbinary php-pecl-memcache php-pecl-redis php-pecl-xdebug php-process php-xml php-bcmath php-pecl-memcached

# redis
yum -y --enablerepo=remi install redis

# memcache
yum -y install memcached

#golang
yum -y --enablerepo=remi install golang

# mysql
rpm -ivh https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
yum install mysql-community-server

# td-agent
vi /etc/yum.repos.d/td.repo
[treasuredata]
name=TreasureData
baseurl=http://packages.treasure-data.com/redhat/$basearch
gpgcheck=0


yum -y --enablerepo=remi install td-agent



wget http://download.virtualbox.org/virtualbox/4.3.8/VBoxGuestAdditions_4.3.8.iso
mkdir -p /media/VBoxGuestAdditions
mount -o loop,ro VBoxGuestAdditions_4.3.8.iso /media/VBoxGuestAdditions
sh /media/VBoxGuestAdditions/VBoxLinuxAdditions.run
rm  -f VBoxGuestAdditions_4.3.8.iso
umount /media/VBoxGuestAdditions
rm -rf /media/VBoxGuestAdditions


#---------------------------------------------------------





echo "[extra]
name=extra
baseurl=http://repo.com2us.kr/extra
gpgcheck=0" > /etc/yum.repos.d/com2us.repo

rpm -Uvh http://repo.webtatic.com/yum/el6/latest.rpm



echo "Set Vi Line Number .."

echo "set nu" >> /etc/virc




echo "Install Nginx ..."

yum -y install nginx


echo "Set Nginx .."

sed -i "9s/.*/root	/vagrant/server/public_html/g" /etc/nginx/conf.d/default.conf
sed -i "10s/.*/index	index.php index.html index.htm/g" /etc/nginx/conf.d/default.conf





echo "Install PHP71 ..."

yum -y --enablerepo=remi-php71 install php php-cli php-common php-devel php-fpm php-json php-mbstring php-mcrypt php-mysqlnd php-opcache php-pdo php-pear php-pecl-apcu php-pecl-apcu-bc php-pecl-geoip php-pecl-igbinary php-pecl-memcache php-pecl-redis php-pecl-xdebug php-process php-xml php-bcmath php-pecl-memcached

echo "Install Redis ... "

yum -y --enablerepo=remi install redis

echo "Install Memcacehd ..."

yum -y install memcached

echo "Install Scribed ... "

yum -y install scribe

echo "Install Mysql ... "

yum -y install mysql

echo "Install Golang ... "

yum -y --enablerepo=remi install golang

echo "Install td-agent(Fluentd) ... "

yum -y --enablerepo=remi install td-agent

echo "install kernel-devel"

yum -y install kernel-devel-2.6.32-696.28.1.el6.x86_64

echo "Disable iptables!"

service iptables stop
chkconfig iptables off

echo "Install Guest Addition .. Need to develop......"



wget http://download.virtualbox.org/virtualbox/4.3.8/VBoxGuestAdditions_4.3.8.iso
mkdir -p /media/VBoxGuestAdditions
mount -o loop,ro VBoxGuestAdditions_4.3.8.iso /media/VBoxGuestAdditions
sh /media/VBoxGuestAdditions/VBoxLinuxAdditions.run
rm  -f VBoxGuestAdditions_4.3.8.iso
umount /media/VBoxGuestAdditions
rm -rf /media/VBoxGuestAdditions