Please refer to the 04-docker/Jenkinsfile

# build and push to dockerhub public repo
docker build -t jenkins-python-docker .
docker tag jenkins-python-docker watertechsakei/jenkins-python-docker:latest
docker login
docker push watertechsakei/jenkins-python-docker:latest

# jenkins pipeline to build and push docker image to dockerhub repo

pipeline {
    agent any
    stages {
        stage('Build and Push Docker Image') {
            steps {
                sh '''
                    docker build -t your-dockerhub-username/jenkins-python-docker:latest .
                    echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                    docker push your-dockerhub-username/jenkins-python-docker:latest
                '''
            }
        }
    }
}

# run container on Windows, with jenkins access to docker host daemon

docker run -d -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home -v //var/run/docker.sock:/var/run/docker.sock --name jenkins-python watertechsakei/jenkins-python-docker:v2

# another way to add jenkins as user so it can access the docker host daemon

docker run -d \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --group-add 103 \
  --name jenkins-python \
  watertechsakei/jenkins-python-docker:v2

# cange the GID based on 

getent group docker



# another way is modify docker.sock file to allow permission 666

# since my jenkins container needs to make sure the docker-in-docker is accessible through the socket 
docker exec -it --user root jenkins-python bash
# quick and dirty way
chmod 666 /var/run/docker.sock



# github remote to trigger webhook and then jenkins build
git add .
git commit -m "COMMIT MESSAGES"
git push origin main

# to have githubwebook link, ngrok needs to be running
RUN: ngrok http 8080
You should see something like below:

Forwarding  https://abc123.ngrok.io -> http://localhost:8080

copy URL https://abc123.ngrok.io and paste to githubwehook

# run container on linux

# run minikube tunnel to simulate loadbalance to get external IP

# using K6 to test how well it handles web tracffic


