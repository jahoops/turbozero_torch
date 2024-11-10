# Use an official Python runtime as a parent image
FROM python:3.10-slim

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    python3-dev \
    net-tools \
    && rm -rf /var/lib/apt/lists/*

# Upgrade pip
RUN pip install --upgrade pip

# Install Python dependencies
COPY ./requirements.txt /tmp/
RUN pip install --no-cache-dir -r /tmp/requirements.txt

# Set environment variables
ARG FOLDER_NAME=pyAgent_C4
ENV FOLDER_NAME=${FOLDER_NAME}

# Set the working directory
WORKDIR /${FOLDER_NAME}

# Copy project files
COPY . /${FOLDER_NAME}

# Expose the debug port
EXPOSE 5680

# Define build arguments with default values; overridden by devcontainer.json
ARG DOCKER_GID=986
ARG USER_ID=1000
ARG GROUP_ID=1000

# Create the docker group with the same GID as the host's docker group
RUN groupadd -g $DOCKER_GID docker

# Create the user 'j' with the same UID and GID, and add to docker group
RUN groupadd -g $GROUP_ID j && \
    useradd -m -u $USER_ID -g j j && \
    usermod -aG docker j

# Switch to user 'j'
USER j
