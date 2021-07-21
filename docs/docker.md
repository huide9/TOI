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
