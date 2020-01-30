cent os 6.1

> https://m.blog.naver.com/PostView.nhn?blogId=iyagi15&logNo=10174224637&proxyReferer=https%3A%2F%2Fwww.google.com%2F

sudo su -

yum install -y python-devel ruby-devel libicu-devel zlib-devel openssl-devel flex

yum install git make flex bison libtool automake openssl-devel libevent libevent-devel python-devel gcc-c++ byacc java-1.7.0-openjdk ant openssl-devel autoconf



\# scribe :: boost

cd /usr/local/src

wget http://sourceforge.net/projects/boost/files/boost/1.44.0/boost_1_44_0.tar.gz

tar zxvfp boost_1_44_0.tar.gz

cd boost_1_44_0

chmod +x bootstrap.sh

./bootstrap.sh

./bjam install





\# scribe :: thrift

cd /usr/local/src

wget http://archive.apache.org/dist/thrift/0.7.0/thrift-0.7.0.tar.gz

tar xvzfp thrift-0.7.0.tar.gz

cd thrift-0.7.0

chmod +x configure

./configure --witchout-java

make && make install



curl -L -O http://ftp.gnu.org/gnu/autoconf/autoconf-2.69.tar.gz
tar zxf autoconf-2.69.tar.gz
cd autoconf-2.69
yum install -y openssl-devel
./configure
make && make install





#fb303을 설치합니다

cd contrib/fb303

./bootstrap.sh

./configure CPPFLAGS="-DHAVE_INTTYPES_H -DHAVE_NETINET_IN_H"

make

make install





\# scribe :: installl

cd /usr/local/src



