```
https://gist.github.com/PinkyJie/5370470
#!/bin/bash

echo '=================================================='
echo '===============Install necessary tools==============='
echo '=================================================='

sudo yum update
sudo yum install git make flex bison libtool automake openssl-devel libevent libevent-devel python-devel gcc-c++ byacc java-1.7.0-openjdk ant

echo '=================================================='
echo '===============update autoconf==============='
echo '=================================================='

cd ~/downloads
sudo rpm -e --nodeps `rpm -qf /usr/bin/autoconf`
wget http://ftp.gnu.org/gnu/autoconf/autoconf-2.69.tar.gz
tar xzf autoconf-2.69.tar.gz 
cd autoconf-2.69/
./configure --prefix=/usr
make
sudo make install





echo '=================================================='
echo '===============install boost==============='
echo '=================================================='

cd ~
wget http://jaist.dl.sourceforge.net/project/boost/boost/1.45.0/boost_1_45_0.tar.gz
tar xzf boost_1_45_0.tar.gz 
cd boost_1_45_0/
./bootstrap.sh
sudo ./bjam install

echo '=================================================='
echo '===============install thrift==============='
echo '=================================================='

cd ~
git clone https://github.com/apache/thrift.git
cd thrift/
git fetch
git branch -a
git checkout 0.8.x
./bootstrap.sh
./configure --with-java
make
sudo make install
cd lib/py/
sudo python setup.py install

echo '=================================================='
echo '===============install fb303==============='
echo '=================================================='

cd ~/thrift/contrib/fb303/
./bootstrap.sh
./configure CPPFLAGS="-DHAVE_INTTYPES_H -DHAVE_NETINET_IN_H"
make
sudo make install
cd py/
sudo python setup.py install

echo '=================================================='
echo '===============install scribe==============='
echo '=================================================='

cd ~
git clone https://github.com/facebook/scribe.git
cd scribe/
export LD_LIBRARY_PATH=/usr/lib:/usr/local/lib
./bootstrap.sh
./configure CPPFLAGS="-DHAVE_INTTYPES_H -DHAVE_NETINET_IN_H -DBOOST_FILESYSTEM_VERSION=2" LIBS="-lboost_system -lboost_filesystem"
make
sudo make install
cd lib/py/
sudo python setup.py install
cd ~

echo '=================================================='
echo '===================== Done! ======================='
echo '=================================================='
```

```
#!/bin/bash
yum update
yum -y install git make flex bison libtool automake openssl-devel libevent libevent-devel python-devel gcc-c++ byacc java-1.7.0-openjdk ant openssl-devel autoconf

echo '=================================================='
echo '===================== install boost ======================='
echo '=================================================='
cd /tmp
yum install wget
wget http://jaist.dl.sourceforge.net/project/boost/boost/1.45.0/boost_1_45_0.tar.gz
tar xzf boost_1_45_0.tar.gz 
cd boost_1_45_0/
./bootstrap.sh
./bjam install


echo '=================================================='
echo '===============install thrift==============='
echo '=================================================='

cd /tmp
git clone https://github.com/apache/thrift.git
cd thrift/
git fetch
git branch -a 
git checkout 0.8.x
./bootstrap.sh
./configure --without-php --without-java
make 
mv compiler/cpp/thrifty.hh compiler/cpp/thrifty.h
make
make install


echo '=================================================='
echo '===============install fb303==============='
echo '=================================================='

cd /tmp/thrift/contrib/fb303/
./bootstrap.sh
./configure CPPFLAGS="-DHAVE_INTTYPES_H -DHAVE_NETINET_IN_H"
make
make install


echo '=================================================='
echo '===============install scribe==============='
echo '=================================================='
cd /tmp
git clone https://github.com/facebook/scribe.git
cd scribe
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib:/usr/local/lib
./bootstrap.sh
./configure CPPFLAGS="-DHAVE_INTTYPES_H -DHAVE_NETINET_IN_H -DBOOST_FILESYSTEM_VERSION=2" LIBS="-lboost_system -lboost_filesystem"
make
make install
cp scribed /usr/bin/
cd /tmp/scribe/src
scribed --help

echo '=================================================='
echo '===================== Done! ======================='
echo '=================================================='
```





scribe 실행

vi scribe.conf

```
##
## Sample Scribe configuration
##
 
# This file configures Scribe to listen for messages on port 1463
 
port=1463
max_msg_per_second=2000000
check_interval=3
 
# DEFAULT
<store>
category=default
type=buffer
 
target_write_size=20480
max_write_interval=1
buffer_send_rate=2
retry_interval=30
retry_interval_range=10
 
<primary>
type=file
fs_type=std
file_path=/game/log/scribe/default_primary
base_filename=thisisoverwritten
max_size=1000000
add_newlines=1
</primary>
 
<secondary>
type=file
fs_type=std
file_path=/game/log/scribe/default_secondary
base_filename=thisisoverwritten
max_size=3000000
</secondary>
</store>
```

scribed -c scribe.conf

![image-20200130101913834](../../\image\image-20200130101913834.png)

scribe client 테스트

- scribe client php 라이브러리 다운
  git clone https://github.com/infynyxx/scribe-php-client
- cd scribe-php-client
- vi scribe.php
  제공된 scribe.php 파일을 아래와 같이 수정합시다

```
<?php
 
$GLOBALS['THRIFT_ROOT'] = 'thrift';
 
// Load up all the thrift stuff
require $GLOBALS['THRIFT_ROOT'].'/Thrift.php';
require $GLOBALS['THRIFT_ROOT'].'/autoload.php';
 
 
// Load the package for scribe
require_once $GLOBALS['THRIFT_ROOT'].'/packages/scribe/scribe.php';
 
function create_scribe_client() {
 
  try {
 
    // Set up the socket connections
    $scribe_servers = array('localhost');
    $scribe_ports = array(1463);
 
    print "creating socket pool\n";
    $sock = new TSocketPool($scribe_servers, $scribe_ports);
    $sock->setDebug(0);
    $sock->setSendTimeout(1000);
    $sock->setRecvTimeout(2500);
    $sock->setNumRetries(1);
    $sock->setRandomize(false);
    $sock->setAlwaysTryLast(true);
    $trans = new TFramedTransport($sock);
    $prot = new TBinaryProtocol($trans);
 
    // Create the client
    print "creating scribe client\n";
    $scribe_client = new scribeClient($prot);
 
    // Open the transport (we rely on PHP to close it at script termination)
    print "opening transport\n";
    $trans->open();
  } catch (Exception $x) {
    print "Unable to create global scribe client, received exception: $x \n";
    return null;
  }
  return $scribe_client;
}
 
// 여기서부터 수정!!!!
$client = create_scribe_client();
$msg[] = new LogEntry(array(
    'category' => 'first_log',
    'message' => 'Hello, world!'
));
$client->Log($msg);
```

- php scribe.php
- 로그가 실제 파일에 저장이 되었는지 확인
  cat /vagrant/game/log/scribe/default_primary/first_log/first_log_current
  Hello, World! 문장이 출력됩니다.





### 재부팅시 에러

**scribed: error while loading shared libraries: libboost_system.so1.45.0: cannot open shared object file: No such file or directory**

- vi /etc/bashrc
- export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib:/usr/local/lib 를 붙여넣는다
- 저장 후, source /etc/bashrc
- echo $LD_LIBRARY_PATH 를 했을 때 다음과 같이 출력되면 잘 적용된 것이다