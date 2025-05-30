FROM jenkins/jenkins:lts

USER root

# Install Python and Docker CLI
RUN apt-get update && \
    apt-get install -y python3 python3-pip docker.io && \
    apt-get clean

# Add Jenkins user to Docker group for access
RUN usermod -aG docker jenkins

USER jenkins
