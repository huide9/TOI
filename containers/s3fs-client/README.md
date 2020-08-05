# manually create a alpine based s3fs client container

## retrieve and run alpine
docker run -dit alpine:3.6 sh

## get container id and attach
docker contianer ls
docker attach <alpine contianer id>

#apk update && apk add git

## install the necessary packages for building s3fs client
apk --update add --virtual build-dependencies build-base alpine-sdk fuse fuse-dev automake autoconf git libressl-dev curl-dev libxml2-dev ca-certificates

## check out source code and build from scratch
git clone https://github.com/s3fs-fuse/s3fs-fuse.git 
cd s3fs-fuse 
git checkout tags/v1.82
./autogen.sh
./configure --prefix=/usr
make
make install 

# one long command!
#apk --update add --virtual build-dependencies build-base alpine-sdk fuse fuse-dev automake autoconf git libressl-dev curl-dev libxml2-dev ca-certificates && git clone https://github.com/s3fs-fuse/s3fs-fuse.git && cd s3fs-fuse && git checkout tags/v1.82 && ./autogen.sh && ./configure --prefix=/usr && make && make install 

# generate the iamge and check in to hub.docker.com
$ docker commit -m "Added GIT" <container ID>  huide/<src_image>
$ docker login
$ docker tag huide/<src_image> huide/<tar_image>[:<revision>]
$ docker push huide/<tar_kamge>[:<revision>]


# server side
## OneFS Cluster
### Enable s3:

  isi services s3 enable

### Create user key   

 isi s3 keys create root --show-key

### copy the user/key info for linux client setup - see <Linux client → Settings→  generate .passwd-s3fs>

### Configure base-domain

isi s3 settings zone modify --base-domain=<your sc zone name>  /* i.e., vert-son.west.isilon.com */
Create bucket

isi s3 buckets create <bucket-name> /ifs/data/<your-bucket-path>
 *if not use htts

isi s3 settings global modify --https-only false

