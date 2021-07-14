# Docker

## Grant Permission

```shell
sudo groupadd docker
sudo gpasswd -a ${USER} docker
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

## Restart without killing containers

https://stackoverflow.com/questions/63434189/does-restarting-docker-service-kills-all-containers
