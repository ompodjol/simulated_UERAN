# Built the Dockerfile
docker build -t open5gs .

# Run the image name open5gs and label the container name as open5gs
sudo docker run -d --name open5gs open5gs tail -f /dev/null

# To login the container name open5gs
sudo docker exec -it open5gs /bin/bash
