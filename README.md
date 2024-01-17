# Cybersecurity-Homelab
Creating a Cybersecurity Home lab on Docker Engine using a Raspberry pi

General Steps

#Connecting my host computer to the raspberry pi using ssh

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/628ba0c1-eb52-44fb-a5d6-5a5f1fb53ce6)

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/1a565ef0-f0e8-44a0-b698-fec0cf44a52d)

#Downloading Docker on Raspberry Pi

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/45014604-2502-43d7-9d64-fa3e8d30e2ca)

#Running the Docker script 

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/eadfb108-c17d-44d8-a06d-e1b39cb7fd5a)

#Downloading Docker Images 

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/fde9a681-99a3-4601-99b3-21e81d669d39)
![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/a5bfbdfb-8554-4321-a976-b5132f0e26f8)
![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/ebb2e18f-c041-4190-9b48-3f925f45c4b7)

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/0c4b3d3c-023e-426f-83a8-2d5fada51bdd)

#Create Docker Repository on the Docker Hub and tagging the Docker images into the Repository 

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/13c5fa41-4aea-4711-bf09-f3be7b31cca0)

https://hub.docker.com/repository/docker/wtauf1998/offensivehackinglab2024/general

#Deploy Ubuntu Container - Kali Container - Nginx Container 

The Ubuntu and Kali containers use the same set of commands with the -it option, enabling an interactive session with the container. This makes it suitable for scenarios where user input is required

Container deployment code for an Nginx container will differ from that of Ubuntu and Kali containers. The -p 80 option maps port 80 of the host to port 80 of the container, and an interactive shell (/bin/sh) is run to override the default command and entry point defined in the Docker images

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/bb203b43-2b74-4ee7-98cc-fdae70e343c5)

#Downloading Packages 

Kali Container 

Commands used
1.apt install iproute2 > IP routing:
2.apt install iputils-ping> Ping Utility:
3.apt install git > git:

Ubuntu Container

Commands used 
1.apt install iproute2 > IP routing:
2.apt install iputils-ping> Ping Utility:

nginx Container 

Commands used
1.apk add bash-completion
2.apk add openssl

#Establish basic connection using Docker(0) network

By using the ip addr command on each container, you can inspect their IP addresses. Running a ping from Kali to both the Ubuntu and Nginx containers allows you to verify the basic network connection between the Kali machine and the other two containers.

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/f61468e7-964f-423c-9f3a-a4a2cd337717)




