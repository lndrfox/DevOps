# DevOps

## Cheat Sheet

### misc

`docker pull image` - pull   the specified image
`docker images` - list all images on the machine
`docker ps -a` - list all containers that you ran

### run

`-it` will create an interactive container
`-d` will create a container with the process detached from our terminal
`-P` will publish all the exposed container ports to random ports on the Docker host
`-e` is how you pass environment variables to the container.
`--name` allows you to specify a container name
`-v volume:/path/container` run with local volume volume to container path /path/container
`--net=network` will run it on network network


### stop

`docker stop id` - stop container with id ( from docker ps )
`docker rm id` - remove container
`docker rm -f` - shortcut
`docker kill name` - kill container with name name

