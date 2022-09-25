# Rasberry Pi- Stack
Utilising docker containers to run my local IT infra setup on Raspberry Pi.

## Inspiration:
The project is inspired by the [IOTStack](https://github.com/SensorsIot/IOTstack) project by Andres Spiess of SensorsIOT.

## Why I created this
The original project has become to vast and granular, since it incorporates a lot of projects and architectures. I faced several issues to setup a few containers. There are a number of networks, volumes and images that are created and pulled that made it quite cumbersome for me.

## Pre-Requisites
The original project linked above, does the best safe to install docker on our fragile Raspberry Pi. Thus, I would recommend to use it for the installation of  
  * docker
  * docker-compose
  
## Our Docker-Compose:
I used docker-compose to setup and run only one container: [Portainer](https://hub.docker.com/r/portainer/portainer). The rest is done via this.  
Portainer provides a lot of flexibility to play with containers, images and volumes without the need to remeber a lot of terminal commands.

## Deployment:
I deployed only a few required containers.

### On-Prem Data Cloud: Nextcloud  
I used the nextcloudpi docker image by ownyourbits. [Link](https://hub.docker.com/r/ownyourbits/nextcloudpi/)  
  * image: ownyourbits/nextcloudpi
  * ports: 
      - 4443:4443
      - 443:443
      - 81:80
  * volumes: Map `/data` of container to a new volume (created via Volumes in SidePanel in Portainer) or bind it to a path on your host.
  * restart: As you wish to choose.
  
And that was pretty much enough for me to get nextcloud up and running without worrying a lot about databases and networks. Follow the steps at localhost:4443 to get started.

### Block Ads and Trackers: Pi-hole
The Pi-hole® is a DNS sinkhole that protects your devices from unwanted content, without installing any client-side software.
  * image: pihole/pihole:latest
  * ports:
    - "8089:80/tcp" #(the port where the Web-InterFace loads up)
    - "53:53/tcp"
    - "53:53/udp"
    - "67:67/udp"
  * environment:
    - WEBPASSWORD=<your_password_here>
    - INTERFACE=eth0
    - DNSMASQ_USER=root #(this enables pihole to be used by root for acting as the primary DNS).
  * volumes:
    - ./volumes/pihole/etc-pihole/:/etc/pihole/
    - ./volumes/pihole/etc-dnsmasq.d/:/etc/dnsmasq.d/
  * dns:
    - 127.0.0.1
    - 1.1.1.1
  * cap_add:
    - NET_ADMIN
  * restart: As you wish to choose.
      

