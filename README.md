# Cybersecurity-Homelab
Creating a Cybersecurity Home lab on Docker Engine using a Raspberry pi

General Steps

#Connecting host computer to the raspberry pi using ssh

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

#Downloading pre configured Webpage & NGINX c2 configuration file 

DEEBOODAH PNG: https://docs.google.com/uc?export=download&id=1EmxGnRJXYjkDesrO4qGMetVJir1B0lMX 

C2 Config File: https://docs.google.com/uc?export=download&id=1vUI5VAhjCsNk7a7FRCkR26AvHSKvRqAu 

HTML File: https://docs.google.com/uc?export=download&id=1bV88JJ_vSadnbdZoakZjUG1sf87-40ZO 

Befor using wget command I created a folder called project_files in the / directory.

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/916fafc4-383e-4e8f-8d6c-e649db3df09e)

Will be using the wget command with -o to output the file onto onto the raspberry pi 

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/a06cd186-155a-4fda-88d5-4903fbf795a1)
![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/c46e3618-527f-4a7e-a72d-fc6ce794cbb5)
![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/32af8a76-8fd5-496f-ac0a-0b68c2d835d1)

From here, copy the three files to the Nginx container (reverse proxy) using the docker cp command. Ensure you use docker ps to find the CONTAINER ID.

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/13ece8d2-3a86-43c9-9802-d0370353df2c)

Go back to the reverse_proxy container where you copied the three files using docker exec -it reverse_proxy /bin/bash. Replace the index.html file with the custom index.html file using cp command and copy the dee_boo_dah.png file into the /nginx/html directory.

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/1a6e538f-37ef-464e-879d-fd8ea21d6328)

The c2_config.c2 file will be moved to /etc/nginx/conf.d/ directory using the mv command.

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/5ffb71b2-5965-4b70-97a3-14789457a32e)

#Create a selfsigned certificate using openssl so we can serve https instead of http through the reverse_proxy

Go to /etc/ssl using cd.  Create a directory called private if you do not have one already using mkdir private.  
Once that is created will use the command 'openssl req -x509 -nodes -days 365 -newkey rsa:4096 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt -subj "/C=US/ST=NYO=DEE, BOODAH./OU=IT/CN=deeboodah.com" '

Breaking down openssl here 
openssl req: This command is part of OpenSSL and is used for X.509 certificate requests and related functions.

-x509: This option specifies that the output should be a self-signed certificate (X.509) rather than a certificate signing request (CSR).

-nodes: This option indicates that the private key should not be encrypted with a passphrase.

-days 365: This sets the validity period of the certificate to 365 days.

-newkey rsa:4096: This generates a new RSA private key with a length of 4096 bits.

-keyout /etc/ssl/private/nginx-selfsigned.key: This specifies the path and filename where the generated private key should be saved.

-out /etc/ssl/certs/nginx-selfsigned.crt: This specifies the path and filename where the generated self-signed certificate should be saved.

-subj "/C=US/ST=NY/O=DEE, BOODAH./OU=IT/CN=deeboodah.com": This provides the subject information for the certificate. It includes:

/C=US: Country code (United States)

/ST=NY: State or Province (New York)

/O=DEE, BOODAH.: Organization name (DEE, BOODAH.)

/OU=IT: Organizational Unit (IT)

/CN=deeboodah.com: Common Name (deeboodah.com)

This has just created a public and private key.

#Changing the IP address in our reverse_proxy c2_config.c2 file.

I will install nano to use as my text editor using the command apk install nano

navigate to c2_config.c2 in /etc/nginx/conf.d/ and nano c2_config.c2 

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/b9d4c1ef-e596-4d1f-ad9b-91609d3843b9)

We will update who the specified IP address which is your reverse_proxy (172.17.0.4)which will be the default server.   

Along editing the attack server which Forwards requests to the specified backend server using HTTPS.

![image](https://github.com/Will-Tauferner/Cybersecurity-Homelab/assets/112906919/21ef9f91-aaf1-4426-83aa-bede04771e04)



