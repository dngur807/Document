### 설치

mkdir -p /vagrant/game/public_html
mkdir -p /vagrant/game/log
mkdir -p /vagrant/game/log/nginx
mkdir -p /vagrant/game/log/php-fpm
mkdir -p /vagrant/game/log/scribed
mkdir  ~/downloads

chmod -R 755 /vagrant

echo "Install system packages..."





cd ~/downloads

#sudo -i

yum -y update
yum -y install wget
yum -y install epel-release
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
wget http://rpms.remirepo.net/enterprise/remi-release-7.rpm
rpm -Uvh remi-release-7.rpm epel-release-latest-7.noarch.rpm

rpm -Uvh epel-release-latest-7.noarch.rpm

rpm -Uvh  http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

rpm -Uvh http://repo.webtatic.com/yum/el6/latest.rpm



echo "Set Vi Line Number .."

echo "set nu" >> /etc/virc



selinux 를 끈다.

vi /etc/sysconfig/selinux

SELINUX=disabled

재부팅 후 sestatus 확인





yum update 

curl -O http://nginx.org/packages/centos/7/x84_64/RPMS/nginx-1.14.0-1.el7_4.ngx.x86_64.rpm
yum localinstall nginx-1.14.0-1.el7_4.ngx.x86_64.rpm





echo "Install PHP72 ..."

yum -y --enablerepo=remi-php72 install php php-cli php-common php-devel php-fpm php-json php-mbstring php-mcrypt php-mysqlnd php-opcache php-pdo php-pear php-pecl-apcu php-pecl-apcu-bc php-pecl-geoip php-pecl-igbinary php-pecl-memcache php-pecl-redis php-pecl-xdebug php-process php-xml php-bcmath php-pecl-memcached



```
systemctl restart firewalld(FirewallD is not running 뜨면) 
 firewall-cmd --add-port=80/tcp --permanent
 firewall-cmd --reload
```







yum -y --enablerepo=remi install redis
yum -y install memcached



rpm -ivh https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
yum install mysql-community-server





yum -y --enablerepo=remi install golang



curl -L https://toolbelt.treasuredata.com/sh/install-redhat-td-agent2.5.sh | sh



yum -y install kernel-devel-2.6





wget http://download.virtualbox.org/virtualbox/4.3.8/VBoxGuestAdditions_4.3.8.iso
mkdir -p /media/VBoxGuestAdditions
mount -o loop,ro VBoxGuestAdditions_4.3.8.iso /media/VBoxGuestAdditions
sh /media/VBoxGuestAdditions/VBoxLinuxAdditions.run
rm  -f VBoxGuestAdditions_4.3.8.iso
umount /media/VBoxGuestAdditions
rm -rf /media/VBoxGuestAdditions



















