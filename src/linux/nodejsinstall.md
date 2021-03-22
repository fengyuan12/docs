## 安装必要的编译软件包
```javascript
yum install gcc gcc-c++
```

## 从源码下载Nodejs
```javascript
wget https://npm.taobao.org/mirrors/node/v13.1.0/node-v13.1.0.tar.gz
```

## 升级gcc版本
```javascript
wget http://ftp.gnu.org/gnu/gcc/gcc-7.1.0/gcc-7.1.0.tar.gz
tar -zxvf gcc-7.1.0.tar.gz
cd gcc-7.1.0/
vi ./contrib/download_prerequisites
./contrib/download_prerequisites
```
>如果报错：<br/>
yum -y install bzip2


## gcc源码编译
```javascript
yum install m4 -y
yum install gmp-devel.x86_64 -y
yum install mpfr-devel.x86_64 -y
yum install gcc-c++.x86_64 -y
cd ..
mkdir gcc-build-7.1.0
cd gcc-build-7.1.0
/usr/local/gcc-7.1.0/configure --enable-checking=release --enable-languages=c,c++ --disable-multilib
make -j4
make install
```
> ps: /usr/local/gcc-7.1.0/  路径为本地的gcc-7.1.0

## 解压nodejs安装包
```javascript
tar -zxvf node-v13.1.0.tar.gz
```

## 进入解压的node文件夹 开始编译
```javascript
cd node-v13.1.0/
./configure
make
```

##### 如果报错：
/usr/lib64/libstdc++.so.6: version "CXXABI_1.3.9" not found <br/>
```javascript
>ls -l /usr/lib64/libstdc++.so.6
lrwxrwxrwx 1 root root 19 Aug 24 12:28 /usr/lib64/libstdc++.so.6 -> 
libstdc++.so.6.0.13

> strings /usr/lib/libstdc++.so.6.0.13 | grep CXXABI
CXXABI_1.3
CXXABI_1.3.1
CXXABI_1.3.2
CXXABI_1.3.3
CXXABI_1.3.4

> cd **/gcc-7.2.0/build/ # build是编译时创建的目录
> find . -name "libstdc++.so.*"
./prev-x86_64-pc-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6.0.24
./prev-x86_64-pc-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6
./stage1-x86_64-pc-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6.0.24
./stage1-x86_64-pc-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6
./x86_64-pc-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6.0.24
./x86_64-pc-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6

> /bin/cp -f x86_64-pc-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6* /usr/lib
> rm -f /usr/lib64/libstdc++.so.6 # 移出旧链接
> ln -s /usr/lib/libstdc++.so.6.0.24 /usr/lib64/libstdc++.so.6 # 创建新链接

> strings /usr/lib/libstdc++.so.6 | grep CXXABI
CXXABI_1.3
CXXABI_1.3.1
CXXABI_1.3.2
CXXABI_1.3.3
CXXABI_1.3.4
CXXABI_1.3.5
CXXABI_1.3.6
CXXABI_1.3.7
CXXABI_1.3.8
CXXABI_1.3.9
CXXABI_1.3.10
CXXABI_1.3.11
CXXABI_TM_1
CXXABI_FLOAT128
```

##### 如果报错：

```javascript
yum whatprovides libstdc++.so.6

yum install libstdc++.so.6  #安装上边查出来的.so
```
