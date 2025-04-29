FROM --platform=linux/arm64 jenkins/agent:latest-jdk21

USER root

# Install dependencies
RUN apt-get update && apt-get install -y \
    curl unzip python3 python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Install AWS CLI v2 (ARM64)
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip \
    && ./aws/install \
    && rm -rf aws awscliv2.zip

ENV PATH="/usr/local/aws-cli/v2/current/bin:$PATH"

USER jenkins