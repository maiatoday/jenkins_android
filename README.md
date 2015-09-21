# Intro
This image is a jenkins official install with the clibs and android sdk installed ontop of it. It also has the appropriate plugins in plugins.txt.

# run jenkins server for the first time
Install docker. The version at the time this doc was written is 1.8.2
```
    mkdir /home/username/data/jenkins -p
    sudo chown 102 /home/username/data/jenkins
    docker run --name my_jenkins -d -p 8080:8080 -v /home/username/data/jenkins:/var/jenkins_home maiatoday/jenkins_android
```

# start/stop jenkins server
```
    docker start my_jenkins
    docker stop my_jenkins
```

# Connect to the jenkins server
In the browser goto http://localhost:8080
