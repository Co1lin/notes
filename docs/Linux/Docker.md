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

