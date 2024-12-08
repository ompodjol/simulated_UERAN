# Use the latest Ubuntu image as the base
FROM ubuntu:22.04

# Set the non-interactive frontend to avoid prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Install dependencies, MongoDB, and Open5GS
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        python3-pip \
        python3-setuptools \
        python3-wheel \
        ninja-build \
        build-essential \
        flex \
        bison \
        git \
        cmake \
        make \
        libsctp-dev \
        libgnutls28-dev \
        libgcrypt-dev \
        libssl-dev \
        libidn11-dev \
        libmongoc-dev \
        libbson-dev \
        libyaml-dev \
        libnghttp2-dev \
        libmicrohttpd-dev \
        libcurl4-gnutls-dev \
        libtins-dev \
        libtalloc-dev \
        meson \
        ca-certificates \
        vim \
        sudo \
        net-tools \
        iptables \
        gcc \
        g++ \
        lksctp-tools \
        iproute2 \
        iputils-ping \
        openssh-client \
        curl \
        gnupg \
        software-properties-common \
        systemd systemd-sysv dbus && \
    curl -fsSL https://pgp.mongodb.com/server-6.0.asc | gpg --dearmor -o /usr/share/keyrings/mongodb-server-6.0.gpg && \
    echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-6.0.list && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        mongodb-org \
        mongodb-mongosh && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install the latest Node.js LTS version
RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install -y nodejs

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        software-properties-common && \
    add-apt-repository ppa:open5gs/latest


#RUN apt-get update && \
#    apt-get install -y --no-install-recommends \
#        open5gs

# Configure and prepare MongoDB
RUN mkdir -p /data/db && chown -R mongodb:mongodb /data/db


# Clone the Open5GS repository and set up the WebUI
#RUN git clone https://github.com/open5gs/open5gs.git /opt/open5gs && \ #old one
RUN git clone https://github.com/ompodjol/open5gs.git /opt/open5gs && \
    cd /opt/open5gs/webui && \
    npm install && \
    npm run build && \
    cd /opt/open5gs && \
    meson build --prefix=`pwd`/install && \
    ninja -C build && \
    cd build && \
    ninja install


# Expose default MongoDB port
EXPOSE 27017 9999 9191 9192 9193 9194

# Clone and build UERANSIM
RUN git clone https://github.com/ompodjol/UERANSIM.git /opt/UERANSIM && \
    cd /opt/UERANSIM && \
    git pull -r && \
    make

# Set the working directory
WORKDIR /opt/UERANSIM

# Enable MongoDB service
RUN systemctl enable mongod

# Use sed to find and replace a string in the configuration file
#RUN sed -i 's/mcc: 999/mcc: 240/g' /etc/open5gs/*.yaml
#RUN sed -i 's/mnc: 70/mnc: 06/g' /etc/open5gs/*.yaml
#RUN sed -i 's/PLMN-ID:99970/PLMN-ID:24006/g' /etc/open5gs/*.yaml


# Set the default command to initialize systemd
# Start MongoDB
# Start WebUI
CMD ["sh", "-c", "mongod --dbpath /data/db --bind_ip_all --port 27017 & cd /opt/open5gs/webui && npm run dev -- --host 0.0.0.0 & /opt/open5gs/install/bin/open5gs-amfd & /opt/open5gs/install/bin/open5gs-ausfd & /opt/open5gs/install/bin/open5gs-bsfd & /opt/open5gs/install/bin/open5gs-hssd & /opt/open5gs/install/bin/open5gs-mmed & /opt/open5gs/install/bin/open5gs-nrfd & /opt/open5gs/install/bin/open5gs-nssfd & /opt/open5gs/install/bin/open5gs-pcrfd & /opt/open5gs/install/bin/open5gs-scpd & /opt/open5gs/install/bin/open5gs-seppd & /opt/open5gs/install/bin/open5gs-sgwcd & /opt/open5gs/install/bin/open5gs-sgwud & /opt/open5gs/install/bin/open5gs-smfd  & /opt/open5gs/install/bin/open5gs-udmd & /opt/open5gs/install/bin/open5gs-udrd & /opt/open5gs/install/bin/open5gs-upfd & /opt/open5gs/install/bin/open5gs-pcfd & exec /sbin/init"]

