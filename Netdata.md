Netdata is a metric collection and visualization software with a web front-end. It can be used locally or in a large infrastructure. 
Although various methods exist to install, I have chosen docker because it is very easy to setup and remove (when not needed). 

Steps to install:

1. Make sure docker is installed on the Pi. Portainer works too as whole installation can be done via the web GUI through that.

2. I created a file named netdata_run.sh and made it executable.

3. Paste the following into the file:
```
docker run -d --name=netdata \
  -p 19999:19999 \
  -v netdataconfig:/etc/netdata \
  -v netdatalib:/var/lib/netdata \
  -v netdatacache:/var/cache/netdata \
  -v /etc/passwd:/host/etc/passwd:ro \
  -v /etc/group:/host/etc/group:ro \
  -v /proc:/host/proc:ro \
  -v /sys:/host/sys:ro \
  -v /etc/os-release:/host/etc/os-release:ro \
  --restart unless-stopped \
  --cap-add SYS_PTRACE \
  --security-opt apparmor=unconfined \
  netdata/netdata
```
4. Ran the file from bash and waited for docker to pull the image.

5. Access Netdata via the following URL -> http://<YOUR_IP>:19999
