## Installation Docker Swarm 

#### Create 3 Node



| Master        | Worker-01     | Worker-02  |
| ------------- |:-------------:| -----:	 |
|RAM 2Gb		|RAM 1 Gb		|RAM 1 Gb	 |
|CPU 2 Core		|CPU 1 Core		|CPU 1 Core	 |
|HDD 20 Gb		|HDD 20 Gb		|HDD 20 Gb	 |

###### Change Hostname Master :
```
# vi /etc/hostsname
Master
```

###### 1. Change Hostname Worker-01 :
```
# vi /etc/hostsname
Worker-01
```

###### 2. Change Hostname Worker-02 :
```
# vi /etc/hostsname
Worker-02
```


###### 3. Add hosts All Node :
```
# vi /etc/hosts

192.168.0.103	Master
192.168.0.103	Worker-01
192.168.0.103	Worker-02

```
###### 4. Restart All Node

```
reboot
```

###### 5. Update & Upgrade All Node

```
sudo apt-get update -y && sudo apt-get upgrade -y
```

##### 6. install docker version 17.03.2 All Node
```
curl https://releases.rancher.com/install-docker/17.03.sh | sh
```

##### 8. install docker-compose Node Master
```
curl -L "https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

###### 9. configure firewall All Node
```
sudo ufw allow 2376/tcp && sudo ufw allow 7946/udp && sudo ufw allow 7946/tcp && sudo ufw allow 80/tcp && sudo ufw allow 2377/tcp && sudo ufw allow 4789/udp
```

###### 10. reload the UFW firewall and enable it to start on boot All Node
```
sudo ufw reload && sudo ufw enable
```

###### 11. Restart the Docker service to affect the Docker rules

```
sudo systemctl restart docker
```

##### 12. Create Docker Swarm cluster
```
docker swarm init --advertise-addr 192.168.0.103
```
###### output
```
Swarm initialized: current node (iwjtf6u951g7rpx6ugkty3ksa) is now a manager.
To add a worker to this swarm, run the following command:
docker swarm join --token SWMTKN-1-5p5f6p6tv1cmjzq9ntx3zmck9kpgt355qq0uaqoj2ple629dl4-5880qso8jio78djpx5mzbqcfu 192.168.0.103:2377
To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

###### 13. Show node
>docker node ls

###### output
```
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
iwjtf6u951g7rpx6ugkty3ksa *   Master        	  Ready               Active              Leader
```

###### 14. Add Worker Node to swarm cluster
```
docker swarm join --token SWMTKN-1-5p5f6p6tv1cmjzq9ntx3zmck9kpgt355qq0uaqoj2ple629dl4-5880qso8jio78djpx5mzbqcfu 192.168.0.103:2377
```
###### output
```
This node joined a swarm as a worker.
```

###### 15. Run Command 
>Docker node ls

###### output
```
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
iwjtf6u951g7rpx6ugkty3ksa *   Manager-Node        Ready               Active              Leader
snrfyhi8pcleagnbs08g6nnmp     Worker-01-Node      Ready               Active
asadjkjqwdojdwqekkkdlkdl2     Worker-02-Node      Ready               Active

```

###### 16. Test Launch web service in Docker Swarm
>docker service create --name webserver -p 80:80 httpd


###### 17. check the running service
>docker service ls

###### output
```
ID                  NAME                MODE                REPLICAS            IMAGE               PORTS
nnt7i1lipo0h        webserver           replicated          0/1                 apache:latest       *:80->80/tcp
```

###### 18. Scale the web server service across two containers
>docker service scale webserver=2

###### 19. check the status of web server service
>docker service ps webserver

###### output
```
ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE                  ERROR               PORTS
7roily9zpjvq        webserver.1         httpd:latest        Worker-01-Node         Running             Preparing about a minute ago
r7nzo325cu73        webserver.2         httpd:latest        Manager-Node        Running             Preparing 58 seconds ago
```

>Apache web server is now running on Manager Node.
You can now access web server by pointing your web browser to the Manager
Node IP http://192.168.0.103 or Worker Node IP http://192.168.0.104 as shown below: