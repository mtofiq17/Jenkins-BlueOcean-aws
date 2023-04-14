# Jenkins BlueOcean Setup in AWS


### Install Docker on the Ubuntu instance using the following command:
```
sudo apt-get update
sudo apt-get install docker.io
```

### Start the Docker service using the following command:
```
sudo systemctl start docker
```

### Create a Dockerfile to define the Jenkins container.
#### create a new file named Dockerfile in your working directory using the following command:
```
nano Dockerfile
```

### Add the following content to the Dockerfile:

```
FROM jenkins/jenkins:lts
USER root
RUN apt-get update && apt-get install -y apt-transport-https \
       ca-certificates curl gnupg2 \
       software-properties-common
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
RUN apt-key fingerprint 0EBFCD88
RUN add-apt-repository \
       "deb [arch=amd64] https://download.docker.com/linux/debian \
       $(lsb_release -cs) \
       stable"
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
RUN jenkins-plugin-cli --plugins "blueocean docker-workflow"
```

### Build the Docker image using the following command:

```
sudo docker build -t myjenkins .
```

### Run the Docker container using the following command:

```
sudo docker run -d --name jenkins -p 8080:8080 -v jenkins_home:/var/jenkins_home myjenkins
```
