#Build Gramps Connect in a Docker container
#HomePage: https://www.docker.com/
#Docs: https://docs.docker.com/
#Docker Cheat Sheet: https://github.com/wsargent/docker-cheat-sheet

#Build container - uses Dockerfile
docker build -t grampsconnect .

#start Gramps Connect - returns containername
docker run -P -d grampsconnect

#Find out which IP the container <containername> is using
#On my machine it's usually 172.17.0.2
docker inspect -f '{{ .NetworkSettings.IPAddress }}' <containername>

#Access Gramps Connect URL (same machine as it is running on)
#see below for access from another machine
http://172.17.0.2:8085/
Login as demo/demo

#stop Gramps Connect (actually stops all running docker containers)
docker kill $(docker ps -q)

#Override entrypoint and start a /bin/bash instead
docker run -it --entrypoint=/bin/bash grampsconnect 

#cleanup
#Delete stopped containers
docker rm -v `docker ps -a -q -f status=exited`

#Delete dangling images
docker rmi $(docker images -q -f dangling=true)

#Access running container from another machine on the same network
#There are probably better/safer/simpler ways to do this but it works for me
#assuming that the container is running on host <docker-host-ip>
sudo route add 172.17.0.2 gw <docker-host-ip>
