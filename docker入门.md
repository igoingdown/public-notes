## docker入门

1. docker和传统虚拟化方式不同，docker容器在操作系统层面上实现虚拟化，直接复用本地主机的操作系统，而传统方式是在硬件层面实现。

---

2. docker的三个基本概念：

    * image

        镜像，是只读的！表示为dockerfile，dockerfile类似数据库的日志，可以显示各个image之间的树状关系。“一个不包含恶意行为的dockerfile + 一个可靠的base_image = 一个好用的image”

    * container

        容器，container在创建时创建一层可写层作为最上层，这就是container运行时可以修改image的原因！image和container的关系类似程序和进程之间的关系。就是说image可以用来创建container，各个container之间是相互隔离的，而container可以用来运行应用。可以将container看做是一个简易的Linux环境和运行在其中的单个的应用。

    * repository

        docker仓库，集中存放image文件的场所。

3. 目录映射

    ```bash
    docker run -v $local_path:$dist_path -it $image_name
    # 举例如下
    docker run -v ~/Desktop/zmx/dockers:/tf/mnist tensorflow/tensorflow
    docker run -v ~/PycharmProjects/serving-old/:/tf/serving --name tf-server -it $USER/tensorflow-serving-devel
    ```

4. 如果无法使用`docker ps -a`类似的命令无法连接docker守护进程

    有两种方法可以解决
    1. 每次都使用`sudo`
    2. 使用`sudo usermod -a -G docker $USER`解决之后，
    退出所有的用户登录，重新登录之后就可以了！
