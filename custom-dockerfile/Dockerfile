# Use an Alpine-based image as the base
FROM alpine:latest

# Maintainer information
LABEL maintainer="Abbassi Zied <abbassizied@outlook.fr>"

# Set environment variables for versions
ENV HELM_VERSION="v3.10.2"
ENV KUBECTL_VERSION="v1.25.4"
ENV DOCKER_COMPOSE_VERSION="v2.6.1"

# Install dependencies
RUN apk add --no-cache \
    bash \
    curl \
    git \
    sudo \
    openssh-client \
    ca-certificates \
    gnupg \
    libintl \
    tzdata \
    libc6-compat \
    openrc \
    py-pip \
    python3 \
    py3-pip

# Install Docker
RUN apk add --no-cache docker && \
    rc-update add docker boot

# Install Docker Compose
RUN curl -L "https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose

# Install kubectl
RUN curl -LO "https://dl.k8s.io/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl" && \
    chmod +x ./kubectl && \
    mv ./kubectl /usr/local/bin/kubectl

# Install Helm
RUN curl -fsSL https://get.helm.sh/helm-${HELM_VERSION}-linux-amd64.tar.gz | tar -xz && \
    mv linux-amd64/helm /usr/local/bin/helm && \
    rm -rf linux-amd64

# Create Jenkins user and group
RUN adduser -D jenkins && \
    addgroup jenkins docker && \
    echo "jenkins ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Switch to Jenkins user
USER jenkins
WORKDIR /home/jenkins

# Expose Docker daemon socket (optional, if needed for Docker commands)
# VOLUME /var/run/docker.sock

# Set default entrypoint and command
ENTRYPOINT ["/bin/bash"]