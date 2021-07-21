1. 安装 
   * 在 Ubuntu 14.04 上安装 Docker 
前提要求：

内核版本必须是3.10或者以上
依次执行下面的步骤：

sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates
sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D

编辑 /etc/apt/sources.list.d/docker.list 文件，添加 deb https://apt.dockerproject.org/repo ubuntu-trusty main
sudo apt-get update
sudo apt-get purge lxc-docker
apt-cache policy docker-engine
apt-get upgrade
sudo apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual
sudo apt-get install docker-engine
至此，安装过程完成。

运行 sudo service docker start 启动 Docker 守护进程。
运行 docker version 查看 Docker 版本

```
root@devstack:/home/sammy# docker --version
Docker version 1.12.1, build 23cf638
```
启动第一个容器：

启动第一个Docker 容器 docker run hello-world
root@devstack:/home/sammy# docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.
它的运行成功也表明前面的安装步骤都运行正确了。 

以上内容参考自 Docker 官网：https://docs.docker.com/engine/installation/linux/ubuntulinux/

* Docker 到目前（2016/09/16）为止的版本历史
版本号	发布日期	发布经理
Docker 1.12.1	08/18/2016	 
Docker 1.12.0	07/28/2016	 
Docker 1.11.0	04/12/2016	@mlaventure
Docker 1.10.0	02/04/2016	@thaJeztah
Docker 1.9.0	10/29/2015	@tiborvass
Docker 1.8.0	08/11/2015	@calavera

2.  Docker 的基本操作

    2.1  Docker 容器的状态机
![DockerStateMachine](https://github.com/huide9/TOI/blob/master/docs/img/docker-state-machine.jpg)

* 一个容器在某个时刻可能处于以下几种状态之一：
  - created：已经被创建 （使用 docker ps -a 命令可以列出）但是还没有被启动 （使用 docker ps 命令还无法列出）
  - running：运行中
  - paused：容器的进程被暂停了
  - restarting：容器的进程正在重启过程中
  - exited：上图中的 stopped 状态，表示容器之前运行过但是现在处于停止状态（要区别于 created 状态，它是指一个新创出的尚未运行过的容器）。可以通过 start 命令使其重新进入 running 状态 
  - destroyed：容器被删除了，再也不存在了
 
你可以在 docker inspect 命令的输出中查看其详细状态：
```
"State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 4597,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2016-09-16T08:09:34.53403504Z",
            "FinishedAt": "2016-09-16T08:06:44.365106765Z"
        }
```        

    2.2 Docker 命令概述
    
    我们可以把Docker 的命令大概地分类如下：
```
* 镜像操作：
    build     Build an image from a Dockerfile
    commit    Create a new image from a container's changes
    images    List images
    load      Load an image from a tar archive or STDIN
    pull      Pull an image or a repository from a registry
    push      Push an image or a repository to a registry
    rmi       Remove one or more images
    search    Search the Docker Hub for images
    tag       Tag an image into a repository
    save      Save one or more images to a tar archive (streamed to STDOUT by default)
    history   显示某镜像的历史
    inspect   获取镜像的详细信息

* 容器及其中应用的生命周期操作：
    create    Create a new container （创建一个容器）        
    kill      Kill one or more running containers
    inspect   Return low-level information on a container, image or task
    pause     Pause all processes within one or more containers
    ps        List containers
    rm        Remove one or more containers （删除一个或者多个容器）
    rename    Rename a container
    restart   Restart a container
    run       Run a command in a new container （创建并启动一个容器）
    start     Start one or more stopped containers （启动一个处于停止状态的容器）
    stats     Display a live stream of container(s) resource usage statistics （显示容器实时的资源消耗信息）
    stop      Stop one or more running containers （停止一个处于运行状态的容器）
    top       Display the running processes of a container
    unpause   Unpause all processes within one or more containers
    update    Update configuration of one or more containers
    wait      Block until a container stops, then print its exit code
    attach    Attach to a running container
    exec      Run a command in a running container
    port      List port mappings or a specific mapping for the container
    logs      获取容器的日志    
    
* 容器文件系统操作：
    cp        Copy files/folders between a container and the local filesystem
    diff      Inspect changes on a container's filesystem
    export    Export a container's filesystem as a tar archive
    import    Import the contents from a tarball to create a filesystem image
    
* Docker registry 操作：
    login     Log in to a Docker registry.
    logout    Log out from a Docker registry.
    
* Volume 操作
    volume    Manage Docker volumes
    
* 网络操作
    network   Manage Docker networks
    
* Swarm 相关操作
    swarm     Manage Docker Swarm
    service   Manage Docker services
    node      Manage Docker Swarm nodes       
    
* 系统操作：    
    version   Show the Docker version information
    events    Get real time events from the server  (持续返回docker 事件)
    info      Display system-wide information （显示Docker 主机系统范围内的信息）
```
