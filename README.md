# Jenkins-BlueOcean-aws
## Installation

### Install Docker on the Ubuntu instance using the following command:
```
sudo apt-get update
sudo apt-get install docker.io
```

### Start the Docker service using the following command:
```
sudo systemctl start docker
```

### Create a Dockerfile to define the Jenkins container. You can create a new file named Dockerfile in your working directory using the following command:
```
nano Dockerfile
```

### Add the following content to the Dockerfile:

```
FROM jenkins/jenkins:2.387.2
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
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
