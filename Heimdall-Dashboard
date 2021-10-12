Heimdall is a dashboard for self-hosted app. Once you start self hosting apps, managing them becomes easy via something like Portainer or Rancher. 
However, remembering which Application runs on which port. Heimdall helps you with that as it can be configured to show a unified landing page for all your hosted apps.

I prefer to install with docker so that is what I did. I first created a file called heimdall_run.sh and made it executable. Contents of the file are like this:
```
#!/bin/bash
sudo docker run -d \
  --name=heimdall \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Asia/Kolkata \
  -p 8082:80 \
  -p 4430:443 \
  -v heimdall:/config \
  --restart unless-stopped \
  ghcr.io/linuxserver/heimdall
```
You need to change the TZ or the timezone to your region. Also I changed the mapping of port 80 (container) to 8082 on the Pi (since 8080 was already taken). 
Same for port 4430 for https. I also created a docker volume called 'heimdall' but you can map the 'config' directory of the container to any where on your storage as per your wishes.

Execute the file and observe in Portainer. The app then can be reached at http://<YOUR_IP>:8082
