# Built the Dockerfile
docker build -t ueransim .

# Run the image name ueransim and label the container name as ueransim
sudo docker run -d --name ueransim ueransim tail -f /dev/null

# To login the container name ueransim
sudo docker exec -it ueransim /bin/bash
