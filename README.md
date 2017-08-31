# phpIPAM Docker Container
The project is porting phpIPAM to run as Docker container. It is initially available on Raspberry Pi 2/3 platform first. Refer to source of phpIPAM from https://phpipam.net.

You can get docker image from Docker hub at https://hub.docker.com/r/mbixtech/arm32v7-phpipam/.

# Usage
## Alternative 1: Manaul Run

```
docker run -d --name phpipamdb \
  -v /home/docker/phpipam/data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=passw0rd \
  -e TZ=Asia/Bangkok \
  jsurf/rpi-mariadb

```
  
```
docker run -d --name phpipamap \
  --link phpipamdb:phpipamdb \
  -e PHPIPAM_DB_HOST=phpipamdb \
  -e PHPIPAM_DB_USER=root \
  -e PHPIPAM_DB_PASS=passw0rd \
  -e PHPIPAM_DB_NAME=phpipam \
  -e TZ=Asia/Bangkok \
  -p 80:80 \
  mbixtech/arm32v7-phpipam
```

Credits
* phpIPAM IPAM IP address management software from https://phpipam.net/.
