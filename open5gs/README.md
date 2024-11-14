# Built the Dockerfile
docker build -t open5gs .
or
docker build -t open5gs-webui .


# Run the image name open5gs and label the container name as open5gs
sudo docker run -d --name open5gs open5gs tail -f /dev/null
#or
docker run --privileged -d --name open5gs-webui -p 9999:9999 -p 27017:27017 open5gs-webui

#to Access Web UI:
http://<container_ip>:3000

# Verifying the Setup
docker exec -it <container_id> mongo

#Web UI Logs:
docker exec -it <container_id> tail -f /opt/open5gs-webui/logs/default.log


# Create
docker network create net3

# Connecting to net1 and net2
sudo docker network connect net1 open5gs
sudo docker network connect net2 open5gs

# To login the container name open5gs
sudo docker exec -it open5gs /bin/bash

# Running open5gs-scpd
sudo open5gs-scpd

# Running open5gs-nrfd
sudo open5gs-nrfd

# restarting open5gs-amfd
sudo systemctl restart open5gs-amfd

# starting open5gs-amfd
sudo systemctl start open5gs-nrfd

# check status
sudo systemctl status open5gs-nrfd

# start
sudo open5gs-nrfd

# start
sudo open5gs-amfd

# start
sudo open5gs-pcfd

# Home Network Public Key
openssl genpkey -algorithm X25519 -out /etc/open5gs/hnet/curve25519-1.key

# Private and public keys can be view with the command
The public key is used when creating the SIM.
openssl pkey -in /etc/open5gs/hnet/curve25519-1.key -text


