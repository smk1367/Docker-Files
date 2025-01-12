portainer
Portainer is an open-source lightweight management UI which allows you to easily manage your Docker host or Swarm cluster.

docker-compose.yml
version: "3.8"
services:
  portainer:
    image: portainer/portainer-ce
    ports:
      - "8000:8000"
      - "9000:9000"
    volumes:
      - ./data:/data
      - /var/run/docker.sock:/var/run/docker.sock
    restart: unless-stopped
