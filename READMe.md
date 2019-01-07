## 青松旁路解密TLS项目
基于suricata旁路解密的tls-patch，支持suricata3.0-4.0版本。

依赖openssl-1.0.2o(https://www.openssl.org/source/old/1.0.2/openssl-1.0.2o.tar.gz)

使用方法：

```
cd $WORK-PATH

wget https://www.openssl.org/source/old/1.0.2/openssl-1.0.2o.tar.gz

tar xf openssl-1.0.2o.tar.gz

cd openssl-1.0.2o ./config --prefix=$PWD/openssl-work && make install

cd suricata

./configure --prefix=$PWD/suricata-work/ --enable-tls-decode=yes  --with-libopenssl-includes=$WORK-PATH/openssl-1.0.2o/openssl-work/include --with-libopenssl-libraries==$WORK-PATH/openssl-1.0.2o/openssl-work/lib

make && make install
```

tips:

	目前版本支持基于rsa密钥协商方式的解密操作。需要将获取到的私钥放到suricata的工作目录中，当前版本支持一个私钥，如果需要
	多私钥的方式，在代码中添加密钥对部分代码（具体格式key,server,port）用于获取对应密钥进行解密，对解完密的明文按照各自需求
	进行下一步处理
验证方式：

```
OpenSSL> s_server -key server.key -cipher AES128-GCM-SHA256
OpenSSL> s_client

suricata -i lo -k server.key
```