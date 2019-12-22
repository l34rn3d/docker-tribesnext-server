# Docker TribesNext

## Information
Tribes 2 dedicated server patched to run in Lan mode and running within Docker under wine.

*This is does not include TribesNext and uses a LAN patch to run in without it*

The image will pull required files and install them at build time (providing the sources are live). 

Docker image is completely self contained when built; it is currently based off Alpine/Ubuntu. This brings in the server at around 3.2GB once built.

The server runs as the gameserv user


## Ports
Exposed ports are `666`, `28000`, the standard TribesNext ports, these can be mapped to whatever you need when you run the container, example below.


## Volumes
No volumes are used


## Usage
**Build the image**

`sudo docker-compose up --build`

**Run a container**

NB: the `--rm` arg will destroy the container when stopped; internal ports (666) can be mapped to available host ports (27999) per container
The container starts automatically when built.

**Stop container**

`docker ps`
`docker stop <container-id>`


## Server Customization
You can customize the server at build time by dropping the appropriate files at the appropriate locations in `_custom/`, these will be copied into the image into the install location within the container at build time.
You can customize your server from the available options in the docker compose. These are set at build time.

You can override the following defaults at build time
```
ARG SRVUSER=gameserv
ARG SRVUID=1000
ARG SRVDIR=/tmp/tribes2/
```

You can also override the start-server script by added one to _custom this will overwrite the default at build time.



## Notes
You can modify the installer script to update the source locations of the required files.

`tribesnext-server-installer` may also be used in standalone mode to install TribesNext RC2a on the host system under wine but your mileage may vary.

Testing has been minimal but it is running the NET247 server so you can try it out at any point.


## SSH into server
```
docker ps
docker exec -it --user root <container-id> bash
```


## Install Docker
```
curl -fsSL get.docker.com -o get-docker.sh && sudo sh get-docker.sh
sudo usermod -aG docker USERNAME
```


## Install Docker-Compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose 
sudo chmod +x /usr/local/bin/docker-compose 
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose 
docker-compose --version
```
