# Use Ubuntu 22.04 as base
FROM ubuntu:22.04

# Install base dependencies (sudo, python, mariadb, curl, and GIT)
RUN apt-get update && apt-get install -y \
    mariadb-server \
    python3 \
    python3-pip \
    sudo \
    curl \
    git \
    # Clean up the apt cache
    && rm -rf /var/lib/apt/lists/*

# Install Node.js (version 20, including npm) and Angular CLI
# We use NodeSource repository for a recent, stable Node.js version.
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - \
    && apt-get install -y nodejs \
    && npm install -g @angular/cli

# Create vscode user
RUN groupadd --gid 1000 vscode \
    && useradd -m -s /bin/bash --uid 1000 --gid 1000 vscode \
    && echo "vscode ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/vscode \
    && chmod 0440 /etc/sudoers.d/vscode

# Prepare MariaDB socket
RUN mkdir -p /run/mysqld && chown -R mysql:mysql /run/mysqld

# Create workspace and fix permissions
RUN mkdir -p /workspace && chown -R vscode:vscode /workspace

# Set working directory
WORKDIR /workspace

# Copy Python script and requirements (Assuming they are in the correct relative paths)
COPY create_db.py .devcontainer/create_db.py
COPY requirements.txt requirements.txt

# Install Python dependencies
RUN pip3 install -r requirements.txt

# Switch to vscode user
USER vscode
