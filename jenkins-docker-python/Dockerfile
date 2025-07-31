FROM jenkins/jenkins:lts

USER root

# Install Python, venv module, pip, and Docker CLI
RUN apt-get update && \
    apt-get install -y \
        python3 \
        python3-pip \
        python3.11-venv \
        docker.io && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN groupadd -g 103 docker && usermod -aG docker jenkins

# Add Jenkins user to the Docker group


USER jenkins
# run jenkins as root quick and dirty way
# or can run jenkins as jenkins user