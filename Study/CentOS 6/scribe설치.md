cent os 6.1
yum update
yum install git make flex bison libtool automake openssl-devel libevent libevent-devel python-devel gcc-c++ byacc java-1.7.0-openjdk ant openssl-devel autoconf
yum install git make flex bison libtool automake openssl-devel libevent libevent-devel python-devel gcc-c++ byacc java-1.7.0-openjdk ant openssl-devel autoconf



boost 라이브러리 1.44버전을 설치합니다. 
cd /tmp
yum install wget
wget http://jaist.dl.sourceforge.net/project/boost/boost/1.44.0/boost_1_44_0.tar.gz
tar xzf boost_1_44_0.tar.gz
 cd boost_1_44_0/
chmod +x bootstrap.sh
./bootstrap.sh
chmod +x bjam
./bjam install

thrift
wget http://archive.apache.org/dist/thrift/0.7.0/thrift-0.7.0.tar.gz
tar xvzfp thrift-0.7.0.tar.gz
cd thrift-0.7.0
./bootstrap.sh
./configure
make && make install