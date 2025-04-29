FROM jenkins/agent:latest-alpine3.21-jdk21

USER root

# Install AWS CLI v2 for ARM64
RUN apk add --no-cache curl unzip python3 py3-pip \
    && curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip \
    && ./aws/install \
    && rm -rf aws awscliv2.zip

USER jenkins