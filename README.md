[linuxserverurl]: https://linuxserver.io
[forumurl]: https://forum.linuxserver.io
[ircurl]: https://www.linuxserver.io/irc/
[podcasturl]: https://www.linuxserver.io/podcast/
[appurl]: https://nginx.org/
[hub]: https://hub.docker.com/r/lsioarmhf/nginx-armhf/

[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)][linuxserverurl]

The [LinuxServer.io][linuxserverurl] team brings you another container release featuring easy user mapping and community support. Find us for support at:
* [forum.linuxserver.io][forumurl]
* [IRC][ircurl] on freenode at `#linuxserver.io`
* [Podcast][podcasturl] covers everything to do with getting the most from your Linux Server plus a focus on all things Docker and containerisation!

# lsioarmhf/nginx-armhf
[![](https://images.microbadger.com/badges/version/lsioarmhf/nginx-armhf.svg)](https://microbadger.com/images/lsioarmhf/nginx-armhf "Get your own version badge on microbadger.com")[![](https://images.microbadger.com/badges/image/lsioarmhf/nginx-armhf.svg)](https://microbadger.com/images/lsioarmhf/nginx-armhf "Get your own image badge on microbadger.com")[![Docker Pulls](https://img.shields.io/docker/pulls/lsioarmhf/nginx-armhf.svg)][hub][![Docker Stars](https://img.shields.io/docker/stars/lsioarmhf/nginx-armhf.svg)][hub][![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Builders/armhf/armhf-nginx-armhf)](https://ci.linuxserver.io/job/Docker-Builders/job/armhf/job/armhf-nginx-armhf/)

This Container is a simple nginx webserver configured with default and ssl, and all relevant config files moved out the user via /config for ultimate control. it contains some of the basic php-packages. and is built on our internal nginx baseimage.

[![nginx](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/nginx-banner.png)][appurl]

## Usage

```
docker create \
--name=nginx \
-v <path to data>:/config \
-e PGID=<gid> -e PUID=<uid>  \
-p 80:80 -p 443:443 \
-e TZ=<timezone> \
lsioarmhf/nginx-armhf
```

## Parameters

`The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side. 
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
http://192.168.x.x:8080 would show you what's running INSIDE the container on port 80.`


* `-p 80` - The web-services.
* `-p 443` - The SSL-Based Webservice
* `-v /config` - Contains your www content and all relevant configuration files.
* `-e PGID` for GroupID - see below for explanation
* `-e PUID` for UserID - see below for explanation
* `-e TZ` - timezone ie. `America/New_York`

It is based on alpine linux with s6 overlay, for shell access whilst the container is running do `docker exec -it nginx /bin/bash`.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application 
`IMPORTANT... THIS IS THE ARMHF VERSION`

Add your web files to /config/www for hosting. 

*Protip: This container is best combined with a sql server, e.g. [mariadb](https://hub.docker.com/r/linuxserver/mariadb/)* 


## Info

* To monitor the logs of the container in realtime `docker logs -f nginx`.


* container version number 

`docker inspect -f '{{ index .Config.Labels "build_version" }}' nginx`

* image version number

`docker inspect -f '{{ index .Config.Labels "build_version" }}' lsioarmhf/nginx-armhf`

## Versions

+ **30.09.17:** Add lua5.1-cjson and lua5.1-resty-http packages for google auth support, copy additional root files into image
+ **24.09.17:** Add memcached service
+ **31.08.17:** Add php7-phar
+ **14.07.17:** Enable modules dynamically in nginx.conf
+ **22.06.17:** Add various nginx modules and enable all modules in the default nginx.conf
+ **05.06.17:** Add php7-bz2
+ **29.05.17:** Rebase to alpine 3.6.
+ **03.05.17:** Update to php 7.1x.
+ **18.04.17:** Add php7-sockets
+ **01.03.17:** Intial Release.
