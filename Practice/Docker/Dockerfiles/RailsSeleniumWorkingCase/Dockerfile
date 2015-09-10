# FROM image ubuntu:14.04
FROM ubuntu:14.04

# my info
MAINTAINER benbai123

# image version
LABEL version="0.1.0"

##
#       NOTE: Need to update the two variables
#           BEFORE build image.
#           
#           hostsip: IP of the machine that run
#               the docker container.
#           
#           default_remote_ip: IP of default remote
#               machine that run Selenium Sdandalone Server
##
ENV hostsip "1.1.1.1"
ENV default_remote_ip "2.2.2.2"

RUN apt-get -y update

##
#      Install required items
##
# for add-apt-repository
RUN apt-get install -y python-software-properties
RUN apt-get install -y software-properties-common
# for make
RUN apt-get install -y build-essential

##
#      Install JDK and Jenkins
##
# Install Oracle JDK/JRE
# (You can choose any other JDK/JRE you want)
RUN add-apt-repository -y ppa:webupd8team/java
# RUN rm -rf /var/lib/apt/lists/*
RUN apt-get -y update
# auto agree license
RUN echo oracle-java7-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
# then install java
RUN apt-get install -y oracle-java7-installer

# Install Jenkins, commands from official site
# Add apt repo I guess
RUN wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -
RUN sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
# update and install
RUN apt-get -y update
RUN apt-get install -y jenkins

##
#       Install Rails
##
# Install ruby 2.1
RUN add-apt-repository -y ppa:brightbox/ruby-ng
RUN apt-get -y update
RUN apt-get install -y ruby2.1

# Install ruby Dev-Kit
RUN apt-get install -y ruby2.1-dev

# Install sqlite3 (but no DB Needed for this project)
RUN apt-get install -y sqlite3

# Install sqlite3 Dev-Kit
RUN apt-get install -y libsqlite3-dev

# Install nodejs (javascript-runtime)
RUN apt-get install -y nodejs

# Install Rails
RUN gem install rails --no-ri --no-rdoc -v "=4.1"

##
#       Install Git and Clone Git Repo
#       You can clone any extra repo here if needed
##
# install git
RUN apt-get install -y git
# clone repository
RUN git clone https://github.com/benbai123/RubyOnRails.git
RUN git clone https://github.com/benbai123/DevOps.git

##
#       Init environment
##
# go to SeleniumTest project
RUN cd /RubyOnRails/Practice/RubyOnRails/Test/Selenium/WorkinCase/SeleniumTest
WORKDIR /RubyOnRails/Practice/RubyOnRails/Test/Selenium/WorkinCase/SeleniumTest

# bundle install
RUN bin/bundle install

# update hosts file
RUN sed -i "s/192\.168\.31\.225/${hostsip}/g" test/test_helper.rb

##
#       Copy cloned job to jenkins folder
##
# copy jenkins job
RUN mkdir -p /var/lib/jenkins/jobs
RUN cp -r /DevOps/Resources/jenkins_job/RailsSeleniumWorkingCase/DockerSeleniumTest /var/lib/jenkins/jobs/DockerSeleniumTest
# update IP of default remote machine
RUN sed -i "s/192\.168\.31\.169/${default_remote_ip}/g" /var/lib/jenkins/jobs/DockerSeleniumTest/config.xml
RUN chmod 777 -R /var/lib/jenkins
RUN chown jenkins:jenkins -R /var/lib/jenkins

# create commands run at container startup
RUN touch cmds
RUN echo "#!/bin/bash" >> cmds
##
#         These commands will be executed when container startup
#         You can add some more commands such like
#         update some git repository (e.g., a repo contains your test case)
#         copy test cases to test/integration folder, etc
##
RUN echo "echo Init_Docker_For_Selenium_Test" >> cmds
RUN echo "/etc/init.d/jenkins start" >> cmds
RUN echo "bin/bundle exec rails s" >> cmds

RUN chmod +x cmds
RUN chmod 777 -R .

CMD /RubyOnRails/Practice/RubyOnRails/Test/Selenium/WorkinCase/SeleniumTest/cmds
