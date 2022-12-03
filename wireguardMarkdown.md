# Docker WireGuard Project

Create a DigitalOcean.com account
Create a Ubuntu 20.04 (LTS) Droplet
Open the console

## Download Docker

`sudo apt-get update
 sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release`

Add the GPG key
`sudo mkdir -p /etc/apt/keyrings
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg`

Set up the Repo
``echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null``

Download the latest version
`sudo apt-get update`
`sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin docker-compose`

Check the installation
`sudo docker run hello-world`

## Download WireGuard

Make the directories
`mkdir -p ~/wireguard/`
`mkdir -p ~/wireguard/config/`

Make the configuration file
`nano ~/wireguard/docker-compose.yml`

Add this to the docker-compose.yml file

"version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=US/Central
      - SERVERURL=
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1"

Edit SERVERURL to the private IP from the DO Droplet

Start Wireguard
`cd ~/wireguard/`
`docker-compose up -d`

**If it does not run correctly, edit the Version in the docker-compose.yml file to '3.3'**

### Connect Wireguard to Phone

Run `docker-compose logs -f wireguard`

Open the Wireguard app
Add a tunnel using QR code
Scan the given code
Turn on tunnel

### Test Wireguard

Download Wireguard computer application
Find and copy the .conf file to an accessible folder
Add a tunnel with the .conf file
Activate
Verify on [IPLeak.net](www.ipleak.net)


[Docker_Installation](https://docs.docker.com/engine/install/ubuntu/)
[Wireguard_Installation](https://thematrix.dev/setup-wireguard-vpn-server-with-docker/)