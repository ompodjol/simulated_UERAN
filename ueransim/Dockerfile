# Use the latest Ubuntu image as the base
FROM ubuntu:latest

# Set the non-interactive frontend to avoid prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Update package lists and install required packages
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        make \
        ca-certificates \
        git \
        vim \
        sudo \
        net-tools \
        iptables \
        gcc \
        g++ \
        libsctp-dev \
        lksctp-tools \
        iproute2 \
        cmake \
        iputils-ping \
        openssh-client \
        curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Clone the UERANSIM repository
RUN git clone https://github.com/aligungr/UERANSIM.git /opt/UERANSIM

# Set the working directory
WORKDIR /opt/UERANSIM

# Build UERANSIM
RUN make

# Set the default command to launch an interactive shell
CMD ["/bin/bash"]

