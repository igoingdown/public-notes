## 安装

[完整的mango安装教程](http://www.jianshu.com/p/1bb663918cfd)

---

##  安装提示

A CA file has been bootstrapped using certificates from the SystemRoots
keychain. To add additional certificates (e.g. the certificates added in
the System keychain), place .pem files in `/usr/local/etc/openssl/certs`.

and run
```bash
/usr/local/opt/openssl/bin/c_rehash
```
This formula is keg-only, which means it was not symlinked into `/usr/local`.

Apple has deprecated use of OpenSSL in favor of its own TLS and crypto libraries

If you need to have this software first in your PATH run:
```bash
echo 'export
PATH="/usr/local/opt/openssl/bin:$PATH"' 
>> ~/.bash_profile
```
For compilers to find this software you may need to set:
```
LDFLAGS: -L/usr/local/opt/openssl/lib
CPPFLAGS: -I/usr/local/opt/openssl/include
```
For pkg-config to find this software you may need to set:
```
PKG_CONFIG_PATH: /usr/local/opt/openssl/lib/pkgconfig
```
---

## 配置与启动

To have launchd start mongodb now and restart at login:
```bash
  brew services start mongodb
```
Or, if you don't want/need a background service you can just run:
```bash
  mongod --config /usr/local/etc/mongod.conf
```

实际的启动方法是这样的：
```shell
/usr/local/Cellar/mongodb/3.4.4/bin/mongod
# 开启守护进程，即server端

/usr/local/Cellar/mongodb/3.4.4/bin/mongo
# 开启mongo client，自动连接本机的server端。
# 在PATH环境变量中添加了上述的bin目录之后，直接用mongod和mongo就行了。
```
---

## mongo shell路径

`/usr/local/Cellar/mongodb/3.4.4/bin`



