# Make an android build server in a docker image
You don't need your own one, I created one on docker hub. It has jenkins, git, jdk 7, and android install. You can see it at https://hub.docker.com/r/maiatoday/jenkins_android/
The Dockerfile is hosted on a github repo.
https://github.com/maiatoday/jenkins_android

# Run jenkins server for the first time
Install docker. The version at the time this doc was written is 1.8.2
```
    docker pull maiatoday/jenkins_android # pull the image from docker hub
    mkdir /home/username/data/jenkins -p # replace username - this will be the shared directory
    sudo chown 102 /home/username/data/jenkins # you need to chown otherwise the jenkins user in the container can't access it
    docker run --name my_jenkins -d -p 8080:8080 -v /home/username/data/jenkins:/var/jenkins_home maiatoday/jenkins_android
```

# start/stop jenkins server
```
    docker start my_jenkins
    docker stop my_jenkins
```

# Connect to the jenkins server
In the browser goto http://localhost:8080

# Useful docker commands
```
    docker exec -it my_jenkins /bin/bash #connect to running container and get a shell prompt
```

# jenkins ssh setup
get a shell prompt on the running jenkins server
```
    docker exec -it my_jenkins /bin/bash #connect to running container and get a shell prompt
```
switch to the jenkins user
```
   su jenkins -s /bin/bash
```
Do following commands to generate a ssh key.
```
   cd # go to home directory
   ssh-keygen -t rsa -b 4096 -C "email@host.com" # insert email and choose a different name for the file e.g. repo1-rsa
```
Browse over to github and click on "admin" for each of the repo's, click on deploy key, and add .pub file version as the deploy key. Remember the keys are in the .ssh folder of the shared folder.

In the jenkins user shell make/edit ``~/.ssh/config` Add the following: replace repo1 and repo1-rsa with the correct values. You can have multiple instances
```
    Host repo1.github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/repo1-rsa
```

When you refer to a github url use the new url as in the following example
  original url:
  git@github.com:maiatoday/RistrettoEspressoTest.git
  new url:
  git@repo1.github.com:maiatoday/RistrettoEspressoTest.git

# project setup in jenkins
* make shh or username password credentials in jenkins
* make a new freestyle item
* add the git repo url
* set the update to  polling SCM with values `H/15 * * * *` which will check every 15 minutes to see if it should build

# Docker installation
https://docs.docker.com/installation

# Useful links
http://dertompson.com/2014/09/01/jenkins-in-a-docker-container/
http://blog.zuehlke.com/en/configure-your-android-project-on-jenkins/
http://www.vogella.com/tutorials/Jenkins/article.html
https://github.com/jenkinsci/docker

https://developer.github.com/guides/managing-deploy-keys/
https://gist.github.com/victorborda/2871029
