# build and push to dockerhun public repo
docker build -t jenkins-python-docker .
docker tag jenkins-python-docker your-dockerhub-username/jenkins-python-docker:latest
docker login
docker push your-dockerhub-username/jenkins-python-docker:latest

# jenkins pipeline to build and push docker image

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

# run container on Windows

docker run -d -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home -v //var/run/docker.sock:/var/run/docker.sock --name jenkins-python jenkins-python-docker

# run container on linux

