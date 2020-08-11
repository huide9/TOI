# 1. manually create a alpine based s3fs client container

## 1.1. retrieve and run alpine
``` docker run -dit alpine:3.6 sh ```

## 1.2 get container id and attach
```
docker contianer ls
docker attach <alpine contianer id>

#apk update && apk add git
```

## 1.3 install the necessary packages for building s3fs client
``` apk --update add --virtual build-dependencies build-base alpine-sdk fuse fuse-dev automake autoconf git libressl-dev curl-dev libxml2-dev ca-certificates```

## 1.4 check out source code and build from scratch
```
git clone https://github.com/s3fs-fuse/s3fs-fuse.git 
cd s3fs-fuse 
git checkout tags/v1.82
./autogen.sh
./configure --prefix=/usr
make
make install 
```

## * one long command!
### alpine
``` 
apk --update add --virtual build-dependencies build-base alpine-sdk fuse fuse-dev automake autoconf git libressl-dev curl-dev libxml2-dev ca-certificates && git clone https://github.com/s3fs-fuse/s3fs-fuse.git && cd s3fs-fuse && git checkout tags/v1.82 && ./autogen.sh && ./configure --prefix=/usr && make && make install 
```
### debian
apt update && apt install -y build-essenssial fuse libfuse-dev automake autoconf git libxml2-dev ca-certificates libcurl4-openssl-dev pkg-config

## 1.5 generate the iamge and check in to hub.docker.com
```
$ docker commit -m "Added GIT" <container ID>  huide/<src_image>
$ docker login
$ docker tag huide/<src_image> huide/<tar_image>[:<revision>]
$ docker push huide/<tar_kamge>[:<revision>]
```

# 2. server side
## * OneFS Cluster
### Enable s3:

  ``` isi services s3 enable ```

### Create user key   

 ``` isi s3 keys create root --show-key ```

### copy the user/key info for linux client setup - see <Linux client → Settings→  generate .passwd-s3fs>

### Configure base-domain

```isi s3 settings zone modify --base-domain=<your sc zone name>  /* i.e., vert-son.west.isilon.com */ ```

### Create bucket

``` isi s3 buckets create <bucket-name> /ifs/data/<your-bucket-path> ```
 
* if not use https

``` isi s3 settings global modify --https-only false ```

# 3. mount s3 
## 3.1 generate .passwd-s3fs
```
<user-info>:<credential>
```  
## 3.2 chmod 600:
```
chmod 600 .passwd-s3fs
```
## 3.3 mount the taget bucket to local mount point
```
s3fs <bucket> <local/mount/point> -o passwd_file=<path/to/.passwd-s3fs> -o url="http://<your.cluster>:9020" 
```
### debug mode
```
s3fs <bucket> <local/mount/point> -o dbglevel=info -f -o curldbg -o passwd_file=/home/huide/.passwd-s3fs -o url="http://<your.cluster.fqdn>:9020"
```  
## Permission Notes
Run container with follwoing permission to enable fuse mount in container
``` 
docker container run -it --cap-add SYS_ADMIN --device /dev/fuse --security-opt apparmor:unconfined <dir>/<image_name> sh 
```
