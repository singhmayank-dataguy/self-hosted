curl -L https://downloads.portainer.io/ce2.9.1/portainer-agent-stack.yml -o portainer-agent-stack.yml

pi@pi4-1:~ $ cat portainer-agent-stack.yml 
version: '3.2'

services:
  agent:
    image: portainer/agent:2.9.1                            #####change the version to latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /var/lib/docker/volumes:/var/lib/docker/volumes
    networks:
      - agent_network
    deploy:
      mode: global
      placement:
        constraints: [node.platform.os == linux]

  portainer:
    image: portainer/portainer-ce:2.9.1                     #####change the version to latest
    command: -H tcp://tasks.agent:9001 --tlsskipverify
    ports:
      - "9443:9443"
      - "9000:9000"
      - "8000:8000"
    volumes:
      - portainer_data:/data
    networks:
      - agent_network
    deploy:
      mode: replicated
      replicas: 1
      placement:
        constraints: [node.role == manager]

networks:
  agent_network:
    driver: overlay
    attachable: true

volumes:
  portainer_data:


pi@pi4-1:~ $ docker swarm init
Swarm initialized: current node (k7dujl01dnoz0vswelyq7ea4m) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-221ozec1t62a7bhijfglee7s4ttzzc5jbxplt286qqi0m3un47-52pp531wu6wgunylf3u2q25g0 192.168.29.201:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

pi@pi4-2:~ $ docker swarm join --token SWMTKN-1-221ozec1t62a7bhijfglee7s4ttzzc5jbxplt286qqi0m3un47-52pp531wu6wgunylf3u2q25g0 192.168.29.201:2377
This node joined a swarm as a worker.

pi@pi4-1:~ $ docker node ls
ID                            HOSTNAME   STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
k7dujl01dnoz0vswelyq7ea4m *   pi4-1      Ready     Active         Leader           20.10.9
qpstomavmnl2cu9ws7s7m9p36     pi4-2      Ready     Active                          20.10.9

pi@pi4-1:~ $ docker stack deploy -c portainer-agent-stack.yml portainer
Creating network portainer_agent_network
Creating service portainer_agent
Creating service portainer_portainer

pi@pi4-1:~ $ docker ps
CONTAINER ID   IMAGE                          COMMAND                  CREATED          STATUS         PORTS                NAMES
961f05bebbf2   portainer/portainer-ce:2.6.3   "/portainer -H tcp:/…"   4 seconds ago    Up 1 second    8000/tcp, 9000/tcp   portainer_portainer.1.mxcpsg0qx5o96ej87zuro2q66
3fe0dfcd40f1   portainer/agent:2.6.3          "./agent"                17 seconds ago   Up 2 seconds                        portainer_agent.k7dujl01dnoz0vswelyq7ea4m.p1wxsevv0hnnnx1i39mm5z5as

pi@pi4-2:~ $ docker ps
CONTAINER ID   IMAGE                   COMMAND     CREATED          STATUS         PORTS     NAMES
a15391fbe793   portainer/agent:2.6.3   "./agent"   14 seconds ago   Up 3 seconds             portainer_agent.qpstomavmnl2cu9ws7s7m9p36.9gkmsvii16b4smgn7xgtxeuls



