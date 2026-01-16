<!-- ABOUT THE LAB -->

# Devops Jenkins basic Lab

<!-- GETTING STARTED -->

## Getting Started

This is an example of how you may give instructions on setting up your the jenkins lab locally.
To get a local copy up and running follow these simple example steps.

## Installation

1. Create a Dockerfile with the following content:

   ```sh
   FROM jenkins/jenkins:2.528.3-jdk21
   USER root
   RUN apt-get update && apt-get install -y lsb-release ca-certificates curl && \
       install -m 0755 -d /etc/apt/keyrings && \
       curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc && \
       chmod a+r /etc/apt/keyrings/docker.asc && \
       echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \
       https://download.docker.com/linux/debian $(. /etc/os-release && echo \"$VERSION_CODENAME\") stable" \
       | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
       apt-get update && apt-get install -y docker-ce-cli && \
       apt-get clean && rm -rf /var/lib/apt/lists/*
   USER jenkins
   RUN jenkins-plugin-cli --plugins "blueocean docker-workflow json-path-api"
   ```

2. Build the Jenkins BlueOcean Docker Image

   ```sh
   docker build -t myjenkins-blueocean:2.528.3-1 .
   ```

3. Create the network 'jenkins'

   ```sh
   docker network create jenkins
   ```

4. Run the Container

   a. MacOS / Linux

   ```sh
   docker run \
   --name jenkins-blueocean \
   --restart=on-failure \
   --detach \
   --network jenkins \
   --env DOCKER_HOST=tcp://docker:2376 \
   --env DOCKER_CERT_PATH=/certs/client \
   --env DOCKER_TLS_VERIFY=1 \
   --publish 8080:8080 \
   --publish 50000:50000 \
   --volume jenkins-data:/var/jenkins_home \
   --volume jenkins-docker-certs:/certs/client:ro \
   myjenkins-blueocean:2.528.3-1
   ```

   b. Windows

   ```sh
   docker run --name jenkins-docker --rm --detach ^
   --privileged --network jenkins --network-alias docker ^
   --env DOCKER_TLS_CERTDIR=/certs ^
   --volume jenkins-docker-certs:/certs/client ^
   --volume jenkins-data:/var/jenkins_home ^
   --publish 2376:2376 ^
   docker:dind
   ```

5. Connect to the Jenkins
   [https://localhost:8080/](https://localhost:8080/)

6. Get the Password
   ```sh
   docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
   ```

## Installation Reference:

```
https://www.jenkins.io/doc/book/installing/docker/
```
