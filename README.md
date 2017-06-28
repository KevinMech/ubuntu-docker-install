# ubuntu-docker-install
![alt text](https://www.ubuntufree.com/wp-content/uploads/2016/09/Docker-Container-With-Ubuntu.png "Docker logo plus Ubuntu logo")

This guide will walk you through the basic process of using Docker in order to download an Ubuntu image, and then show you how to execute it as a container. 
For this guide, I will be using **Docker 1:17.05.0-1 CE (Community Edition)**.

#### Note
This guide was written with the intent of setting up your ubuntu image as a development environment for a LAMP stack, however, you are not confined to only using it for that purpose alone. If you are setting up a dev environment for LAMP, I will list a few optional commands afterwards that will make your life easier!

---

## Installation
The installation for docker will be different everyone, depending on what operating system you are using. Covering how to install docker on each system will take far too much time, so for that reason alone, I will be documenting how I installed it on my system, and linking the official installation guide for anyone who is running a different operating system. 

**For anyone running a different operating system that doesnt support pacman (Windows, Mac, Ubuntu, etc)**, use this installation guide instead, and once installed and the service is running, skip to step 3: https://docs.docker.com/engine/installation/

For those of you who use an operating system that uses pacman as its package manager, continue reading

1. Begin by opening your terminal (ctrl + alt + T) and entering this command:
```bash
sudo pacman -S docker
``` 

2. Now start the docker service
	
	a. Once docker is finished installing, start the docker service by executing this command:
	```bash
	sudo systemctl start docker
	```

	b. Alternatively, if you dont want to have to start the service manually each time you reboot, you can use this command to start the docker service on system boot:
	```bash
	sudo systemctl enable docker
	```

3. Once the service is running, test to see if docker is working by running this command:
```bash
sudo docker run hello-world
```

Congratulations! You officially ran your first container!

## Downloading an Ubuntu Image

Downloading an ubuntu image is fairly straight forward. 

1. In order to download an ubuntu image, in your terminal, run this command:

```bash
sudo docker pull ubuntu
```

2. To check to see if you have the image downloaded to your local machine, you can use this command:
```bash
sudo docker images
```

## Creating a Container

Now we will create a container using the image we downloaded. We will be using the run command, along with a few parameters we will be passing:

1. In order to create the container, use this command:
```bash
sudo docker run -it ubuntu
```

-i specifies that it will keep the STDIN open even if not attached
-t specifies that it will allocate a pseudo-tty for the container

Once run, you should see a tty like session in your terminal. You are now running ubuntu from docker!

If you want to exit the container, make sure you type ```exit```

## Commiting Changes

One thing you will notice is that whenever you exit the container, all changes will be lost. In order to save any changes that youve made to the container, we will be commiting all changes to a new image.

1. In order to do this, you will first want to make sure that the container you want to commit is currently runnning. Its safe to say that if you are already in the ubuntu container, it should be running, but it doesnt hurt to check. Open up a second terminal (ctrl + alt + T) and type this command:
```bash
sudo docker ps
```

From here, you should see the all the current containers running, along with their ID. Take note of the ID, we are going to need it for the next step.

2. Commit your changes using this command:
```bash
sudo docker commit xxx nameofyournewimage
```
replacing the xxx with the 3 digits of the container ID you wish to save, and replacing nameofyournewimage with the name of the new image you wish to save.

**Warning:** It is possible to overwrite an existing image if you give the new image the same name as the old one, so be careful!

## After Installation

Congratualations! You now have Ubuntu running in docker! This section will cover things you probably *should* do, but is completly optional.

### Update Ubuntu
First things first is updating ubuntu

1. In your terminal, enter in this command:

```bash
apt update & apt upgrade
```

### Installing sudo
Next you should install sudo. Sudo allows you to run programs with elevated permissions, so its pretty important. 

1. To install sudo, type this into your terminal:
```bash
apt install sudo
```

### Creating a new user
Some users may or may not want to login as root to preform their tasks. You can create a new user by following these steps.

1. In your terminal, type in this command:
```bash
adduser name
```
Replacing name with the name of the user you want

2. Enter in the password for the new account on the next prompt, and then retype it

3. The next few prompts will ask you for some information related to the account. You can leave these empty to assign default values to it.

#### Login to new user

Now that the account is created, I will be showing you a new argument you can use in order to login to any accounts created.
```run -u username``` allows you to login to any accounts you created on the image.
if you would like to log in to the account using the ubuntu image, follow these steps

1. First, save any changes youve made to your container using the commit command.
```bash
sudo docker commit xxx nameofyournewimage
```
replacing the xxx with the 3 digits of the container ID you wish to save, and replacing nameofyournewimage with the name of the new image you wish to save.

2. Exit the current container using this command:
```bash
exit
```

3. Then login to the new user account using the -u command:
```bash
sudo docker run -it -u username nameofyournewimage
```
Replacing the username with your new username you created, and nameofyournewimage with the name of the image you saved it as.

4. If you set a password, you will need to type it in the next prompt.

you are now logged into your new user!

## LAMP Setup

This section is also optional, and will cover some useful commands for those using Docker for a LAMP stack.

### Binding Port

Working with a LAMP stack, you are probably going to want to expose your port to access your web server. By default, all ports are closed, and no outside connections can connect with the container. By binding your hosts port to the container port, we can expose this container port to the outside world.

In order to do this, we can use the ```-p``` argument.

The -p argument works like this:

```-p hostport:containerport```

where you suply the port that you want to bind with the host where it says ```hostport```, and the port you want to bind with the container where it says ```containerport```.

So, my apache server is set to use the default port 80 on my container. So I would use this command to bind my port:

```sudo docker run -it ubuntu -p 80:80```