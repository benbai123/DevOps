
### Install Docker

```shell
# Switch to root first
sudo su -
# I always add key manually however
wget -qO- https://get.docker.com/gpg | sudo apt-key add -
# install docker
wget -qO- https://get.docker.com/ | sh

# start docker, it will be started automatically if
# you switch to root by 'sudo su -' at the beginning
service docker start
```

### Download the Dockerfile

**Just download the Dockerfile to your machine somewhere**

### Build image

Edit Dockerfile, update hostsip and default_remote_ip as needed, then

```shell
# the rclt is image name specified by yourself
# change it to any other name if needed
docker build -t rclt .
```

This will take about 20 minutes.

You probably will see lots of red text from terminal but it just things happend in Docker and should be fine.

### Run docker image

Run a just built image as below.

```shell
# run docker image
# -d : Run container in the background
# -p : make the exposed port accessible on the host
# --name : specify a name so can start/stop it easily
# rclt : the image name
docker run -d -p 3000:3000 -p 8080:8080 --name RSWC rclt
```

Start/Stop an exists container as below.

**NOTE:** Use start/stop to run original container, if you use docker run again then it will run a new container and any change made by previous action will be disappeared. 

```shell
docker start RSWC
docker stop RSWC
```
