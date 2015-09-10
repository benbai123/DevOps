
### Demo Video

[Rails Selenium Working Case with Docker](https://www.youtube.com/watch?v=m1jiMEN-8BQ)

The demo video contains every steps below.

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

**NOTE:** To test remote Windows machine you will need to prepare Selenium Standalone Server as described in [Run With Remote WebDriver](https://github.com/benbai123/RubyOnRails/tree/master/Practice/RubyOnRails/Test/Selenium/WorkinCase/SeleniumTest#run-with-remote-webdriver)

```shell
# run docker image
# -d : Run container in the background
# -p : make the exposed port accessible on the host
# --name : specify a name so can start/stop it easily
# rclt : the image name
docker run -d -p 3000:3000 -p 8080:8080 --name RSWC rclt
```

Please refer to [Docker run reference](https://docs.docker.com/reference/run/) for more information.

#### Start/Stop an existing container as below.

**NOTE:** Use start/stop to run original container, if you use docker run again then it will run a new container that does not include any change made by previous action. 

```shell
# start a stopped container
docker start RSWC
# stop a running container
docker stop RSWC
```

### Trouble Shooting and Possible Issue

If the container is not started as expected, you can attach the terminal to see console output from container as below:

```shell
# start container and attach terminal
docker start RSWC && docker attach RSWC
```

One possible issue is [A server is already running](http://stackoverflow.com/questions/15072846/server-is-already-running-in-rails) caused by rails server is not stopped properly when stopping docker container, to solve this issue, you can kill server.pid as below

```shell
# start container and delete the file server.pid
docker start RSWC && docker exec RSWC rm -rf /RubyOnRails/Practice/RubyOnRails/Test/Selenium/WorkinCase/SeleniumTest/tmp/pids/server.pid
```

To prevent this issue, I guess you can modify CMD in Dockerfile as below

```shell
## code before add commands
# ...
RUN echo "echo Init_Docker_For_Selenium_Test" >> cmds
RUN echo "/etc/init.d/jenkins start" >> cmds
# always remove server.pid before start rails server
RUN echo "rm -rf /RubyOnRails/Practice/RubyOnRails/Test/Selenium/WorkinCase/SeleniumTest/tmp/pids/server.pid" >> cmds
RUN echo "bin/bundle exec rails s" >> cmds
## code after
# ...
```
