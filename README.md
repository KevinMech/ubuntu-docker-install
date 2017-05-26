# ubuntu-docker-install
A simple guide to downloading an Ubuntu image to Docker, and executing it as a container.
This simple guide will walk you through the process of using docker in order to download an ubuntu image, and execute it as a container. 
For this guide, I will be using Docker 1:17.05.0-1 CE (Community Edition).

## Installation
The installation for docker will be different everyone, depending on what operating system you are using. Covering how to install docker on each system will take far too much time, so for that reason alone, I will be documenting how I installed it on my system, and linking the official installation guide for anyone who is running a different operating system. 

1. Begin by opening your terminal (ctrl + alt + T) and entering this command:
```
sudo pacman -S docker
``` 

2a. Once docker is finished installing, start the docker service by executing this command:
```
sudo systemctl start docker
```

2b. Alternatively, if you dont want to have to start the service manually each time you reboot, you can use this command to start the docker service on system boot:
```
sudo systemctl enable docker
```