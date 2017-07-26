## 默认用户名和密码

root@localhost: abc123

## mysql服务开启不了的原因在于没有使用sudo！
开启mysql服务只需要下面的步骤即可
```bash
sudo /usr/local/mysql/support-files/mysql.server start
```

## 关闭mysql服务
```bash
sudo /usr/local/mysql/support-files/mysql.server stop
```

可以将上述的命令以alias的形式写入到.bashrc文件中！

现在已经写入到.bashrc中了，可以直接通过
`mysqlstart`和`mysqlstop`来开启和关闭mysql服务器。