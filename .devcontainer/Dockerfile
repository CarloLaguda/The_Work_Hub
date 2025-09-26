# Use Ubuntu 22.04 as base
FROM ubuntu:22.04

# Install base dependencies (sudo, python, mariadb, curl, and GIT)
# Usiamo DEBIAN_FRONTEND=noninteractive per prevenire richieste interattive durante l'installazione
ENV DEBIAN_FRONTEND=noninteractive

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
# Usiamo NodeSource repository per una versione Node.js recente e stabile.
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - \
    && apt-get install -y nodejs \
    && npm install -g @angular/cli

# RIMOZIONE: Eliminiamo la creazione manuale dell'utente. Codespaces gestirà l'utente con UID 1000 
# e i permessi corretti automaticamente, assicurando che la tua identità GitHub sia mappata.
RUN groupadd --gid 1000 vscode \
     && useradd -m -s /bin/bash --uid 1000 --gid 1000 vscode \
     && echo "vscode ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/vscode \
     && chmod 0440 /etc/sudoers.d/vscode


# Prepare MariaDB socket (Questo deve essere eseguito da root, quindi lasciamo "mysql" come proprietario)
RUN mkdir -p /run/mysqld && chown -R mysql:mysql /run/mysqld

# Create workspace (Il clone avverrà qui automaticamente dal Codespace)
RUN mkdir -p /workspace

# Set working directory
WORKDIR /workspace

# Copy Python script and requirements
# Nota: La copia avviene come root, ma Codespaces correggerà i permessi a "vscode" al momento della clonazione.
COPY create_db.py .devcontainer/create_db.py
COPY requirements.txt requirements.txt

# Install Python dependencies
RUN pip3 install -r requirements.txt

# RIMOZIONE: Lasciamo che Codespaces gestisca il cambio utente.
# USER vscode
