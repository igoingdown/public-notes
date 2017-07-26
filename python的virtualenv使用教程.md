## python的强隔离虚拟环境virtualenv使用教程

### 安装virtualenv

```shell
pip install virtualenv
```

### 在工程目录下创建一个虚拟环境
```bash
cd $your_project_dir
virtualenv $your_env_name

# 上述创建env的方法使用的python版本是系统默认版本.
# 创建虚拟环境其实创建了一个文件夹而已！
# 可以使用-p option来指定python版本，这个很有必要。
# 比如我需要创建一个tensorflow0.11的python3.5的环境
virtualenv -p /usr/bin/python3.5 tensorflow0.11

```

### 激活虚拟环境
```bash
source $your_env_name/bin/activate

```

### 安装需要的包
```bash
pip install $package_name

# python3环境下可以使用pip3
pip3 install $package_name

# 注意不要用sudo！！！
```


### 关闭虚拟环境
```bash
deactivate
```

### 删除虚拟环境
```bash
# 删除虚拟环境之前一定要先关闭虚拟环境!
# 删除虚拟环境直接将环境全部所在的目录干掉就完事了！
# 简单粗暴，想要复用装在$HOME下就行，不想复用，用完就删，装在工程目录下就行。
# 这种虚拟环境比anaconda的虚拟环境爽太多了！
rm -rf $your_env_name
```



