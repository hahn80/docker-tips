# Use Ubuntu 20.04 LTS as base image
FROM ubuntu:20.04

# Avoid prompts from apt
ARG DEBIAN_FRONTEND=noninteractive

# Install necessary packages
RUN apt-get update\
    && apt-get install -y \
    build-essential \
    git \
    python3 \
    python3-pip \
    cmake \
    && apt-get clean\
    && rm -rf /var/lib/apt/lists/*

#    libboost-all-dev \
#    curl \

# Create a symbolic link for python command
RUN ln -s /usr/bin/python3.8 /usr/bin/python

# Make port 80 available to the world outside this container
# EXPOSE 80


# Add PATH
# ENV PATH="/path/bin:${PATH}"

# Use /data as a volume for runtime data
VOLUME /data
WORKDIR /data

# Default command
CMD ["/bin/bash"]

