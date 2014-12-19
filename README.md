## Docker Development Environment

#### Getting started

1. Clone the repository.
```
git clone https://github.com/kiltlifter/docker.git
```

2. Build the Docker image.
```
sudo docker build -t centos6-devbox docker/centos/Dockerfile
```

3. Start a Docker container
```
sudo docker run -d -P --name devbox centos6-devbox
```
4. ssh into your container (password is "docker")
```
ssh -p $(sudo docker inspect devbox | sed -rn 's/"HostPort":\s"(.*)"/\1/p') docker@localhost
```

### Stopping containers
* Stopping the devbox container
```
sudo docker stop devbox
```
* If you didn't name your container devbox or don't know the container name,
stop the first one.
```
sudo docker stop $(sudo docker ps | awk '{if (NR>1&&NR<3) {print $1}}')
```
* To stop all containers
```
sudo docker stop $(sudo docker ps | awk '{if (NR>1) {print $1}}')
```

### Destroying containers
* Destroying the devbox container
```
sudo docker rm devbox
```

### Removing this image
* Destroying the devbox image
```
sudo docker rmi centos6-devbox
```
* To remove all images
```
sudo docker rmi $(sudo docker images | awk '{if (NR>1) {print $3}}')
```
