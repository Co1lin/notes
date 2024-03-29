# Docker

## Grant Permission

```shell
sudo groupadd docker
#sudo gpasswd -a ${USER} docker
sudo usermod -aG docker $USER
sudo service docker restart
```

## Clean

```shell
# untaggged and unused
docker image prune
# unused
docker image prune -a
# filter
docker image prune -a --filter "until=24h"
# delete stopped containers
docker container prune
# rm exited containers
docker rm $(docker ps -a -f status=exited -q)
# clean images
docker rmi $(docker images --filter "dangling=true" -q --no-trunc)
```

## Build

```shell
docker build --network host /dir/contains/dockerfile

docker tag img name:tag

docker commit container name:tag
```

## Run

```shell
docker run --name test -h hostname --network host -itd -v /abs/path:/abs/path imgname:tag

docker exec -it test bash
```

## Saving

### Export/Import

```shell
docker export <container_id> > container.tar
docker import /path/to/container.tar
```

Export is taking a snapshot of the container. **All informations about history and layers will be lost!**


### Save/Load

```shell
docker save container_id > container.tar
docker save myimage:latest | gzip > myimage_latest.tar.gz
docker load < hangge_server.tar
```

Informations about layers and history will be saved and loaded!

## Restart without killing containers

https://stackoverflow.com/questions/63434189/does-restarting-docker-service-kills-all-containers
