Portainer is a web GUI tool for managing containers running on servers. It can also be used to manege Docker Swarms and Kubernetes clusters. 

1. Need to have docker installed as a pre-requisite.

2. Run Portainer as a docker container itself. The official image can be found on the "Docker Hub" website.
I am using the CE (Community Edition) since it is freely available and is more than enough for my hobby needs.

3. The official documentation page describes how to install Portainer via docker. I just made a shell script and pasted the commands inside.  
I created a file called portainer_run.sh and pasted the following into it
```
pi@RPi4:~ $ cat portainer_run.sh

#!/bin/bash
echo 'Removing Old Portainer Image-'  #Portainer can't be updated from within its UI.....  
sudo docker rmi portainer -f          
echo 'Done...'

echo 'Removing Old Container-'        #.....so additional steps need to be taken
sudo docker rm portainer
echo 'Done....'

sudo docker run -d \
-p 8000:8000 -p 9000:9000 \
--name=portainer --restart=always \
-v /var/run/docker.sock:/var/run/docker.sock \
-v portainer_data:/data \
portainer/portainer-ce:latest

```

4. Need to create a volume which maps the Portainer data directory (inside the container) to an actual directory on my hard disk. 
Since containers are ephemeral they lose their data when destroyed and recreated. So to persist data it is needed
```
pi@RPi4:~ $ sudo docker volume create portainer_data

pi@RPi4:~ $ sudo docker volume ls
DRIVER    VOLUME NAME
local     portainer_data
```

5. I also made the file (portainer_run.sh created in step #3) executable and ran it from the command line. 
```
pi@RPi4:~ $ sudo docker ps -a
CONTAINER ID   IMAGE                    COMMAND        STATUS          PORTS                                                                                  NAMES
e17fd4f6745f   portainer/portainer-ce   "/portainer"   Up 19 minutes   0.0.0.0:8000->8000/tcp, :::8000->8000/tcp, 0.0.0.0:9000->9000/tcp, :::9000->9000/tcp   portainer
```
Portainer runs on port 9000 and can be reached via the browser by going to http:<YOUR_RPI_IP>:9000

Upon opening the above mentioned URL, Portainer prompts you to create an admin password.

Then you'll be asked to chose the environment you want to connect to and manage. 
In my case I chose Docker but a Kubernetes Cluster or a Docker Swarm can also be managed via Portainer
