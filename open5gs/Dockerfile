# Use the latest Ubuntu image as the base
FROM ubuntu:22.04

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
        curl \
        gnupg \
        systemd systemd-sysv dbus && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Mask unwanted systemd services to prevent them from running in the container
RUN systemctl mask \
    dev-hugepages.mount \
    sys-fs-fuse-connections.mount \
    sys-kernel-config.mount \
    sys-kernel-debug.mount \
    systemd-logind.service \
    getty.target \
    console-getty.service


RUN curl -fsSL https://pgp.mongodb.com/server-6.0.asc | sudo gpg --dearmor -o /usr/share/keyrings/mongodb-server-6.0.gpg

RUN echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        mongodb-org && \
        apt-get clean

# Create the MongoDB data directory
RUN mkdir -p /data/db

# Expose the default MongoDB port
EXPOSE 27017

# Start MongoDB
#CMD ["mongod", "--bind_ip_all"]


RUN apt-get update && \
    apt-get install -y software-properties-common && \
    add-apt-repository ppa:open5gs/latest && \
    apt-get update && \
    apt-get install -y open5gs && \
    apt-get clean


# Clone the UERANSIM repository
RUN git clone https://github.com/ompodjol/UERANSIM.git /opt/open5gs

# Set the working directory
WORKDIR /opt/open5gs

# Build UERANSIM
RUN make

# Set the default command to launch an interactive shell
#CMD ["/bin/bash"]

# Set the default command to initialize systemd
CMD ["/sbin/init"]
