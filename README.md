# OWnCloud-Docker-Compose-IIEC_RISE-Project
ownCloud is a self-hosted file sync and share server. It provides access to your data through a web interface, sync clients or WebDAV while providing a platform to view, sync and share across devices easily—all under your control. And to don't get any failure of data loss and making automation for files have used docker here.

With this repo you can Automatically:-
1. Pull images from [DockerHub](https://hub.docker.com/)
2. Create Docker volumes for permanent use.
3. Set Up MySQl Database.
4. Set up ownCloud self-hosted file sync and share server.

## Built with
- RedHat Linux RHEL8
- Docker
- owncloud
- MYSQl

## Requirements/Installation
Make sure you have the latest versions of **Docker** and **Docker Compose** installed on your machine.
##### If Not than run the following commands:-
For Docker installation on linux[Ubuntu Distro/Similar commands for other distro as well]
```
sudo apt-get update
sudo apt-get remove docker docker-engine docker.io
sudo apt install docker.io
```
## For Docker Installation on RedHAt/Centos:
- Configure yum by adding ***docker.repo and dvd.repo*** inside the `/etc/yum.repos.d` for local installation using  https://download.docker.com/linux/centos/docker-ce.repo   
- Now, run command `yum install docker-ce --nobest`

## Start Docker:
```
$ sudo systemctl start docker
```
After Docker installation Now install Docker Compose:-
```
sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
After installation of the above files:-
```
sudo systemctl start docker
sudo systemctl enable docker
```
 ##  Download the Required Images
Download The NextCloud and MySQL image from https://hub.docker.com using 
    
    docker pull owncloud:latest
    docker pull mysql:5.7 
![img00](https://github.com/rohitg00/OWnCloud-Docker-Compose-IIEC_RISE-Project/blob/master/owncloud%20pull.png)
![img000](https://github.com/rohitg00/OWnCloud-Docker-Compose-IIEC_RISE-Project/blob/master/mysql%20pull.png)
commands.
 ## MySQL setup
Write a command 

    docker -i -t -e MYSQL_ROOT_PASSWORD=(your_password) -e MYSQL_USER=(username) -e MYSQL_PASSWORD=(your_password) -e MYSQL_DATABASE=       (any_database_name) --name dbos mysql:5.7
 ## MySQL client
If you want to verify that your database folder has been created or not, install MySQL client software using yum install mysql. After installation run this command : 
   
    mysql -h 172.17.0.0/16 (your MySQL container IP) -u (username) -p
 ## Docker-Compose
Install a docker-compose software from https://docs.docker.com/compose/install. 
Make a compose file using 

    mkdir mycompose. 
You can create/edit your docker-compose file using 
    
    vim docker-compose.yml
The file name should always be docker-compose.yml
![img0](https://github.com/rohitg00/OWnCloud-Docker-Compose-IIEC_RISE-Project/blob/master/compose%20install.png)
![img4](https://github.com/rohitg00/OWnCloud-Docker-Compose-IIEC_RISE-Project/blob/master/compose-yml.png)
## Usage

Open a terminal and `cd` to the folder in which `docker-compose.yml` is saved and run:-

```
docker-compose up
```
Hola!, Everything is Set now the containers are now built and running. You should be able to access the WordPress installation with the configured IP in the windows browser address by `http://192.168.xx.xx:8081` and for linux OS `http://172.1x.x.x:8081`.[Change it accordingly]

## Here is an instance of my ownCloud server webssite which I built using this repo:- 
![img3](https://github.com/rohitg00/OWnCloud-Docker-Compose-IIEC_RISE-Project/blob/master/owncloud%20mp.png)
![img1](https://github.com/rohitg00/OWnCloud-Docker-Compose-IIEC_RISE-Project/blob/master/owncloud%20with%20server.png)
![img2](https://github.com/rohitg00/OWnCloud-Docker-Compose-IIEC_RISE-Project/blob/master/owncloud%20sp.png)

### Fore more reference on docker-compose 

Start the containers with the `up` command in daemon mode (by adding `-d` as an argument) or by using the `start` command:

```
docker-compose start
```

### Stopping containers

```
docker-compose stop
```
   #### Volumes : 
All our data will be permanent if we mount a volume to the folders where OwnCloud and MySQL stores data. The data will remain permanent if any of the container terminates. For that you have to create volumes first. 
   #### Depends_on : 
OwnCloud uses database server. We have to specify which database container it should depend on.
   #### Ports : 
To expose our container to outside world by using PAT.
   
   ## Troubleshooting the errors
Linux firewall won't allow to connect to MySQL database server and to outside world. Hence following commands should be implemented first in order to connect to server.
```
- selinux 0
- iptables -F
- iptables -P FORWARD ACCEPT
- firewall-cmd --zone=trusted --change-interface=docker0 --permanent (If there are any other networks for docker add them too like br-xxxxx)
- firewall-cmd --zone=trusted --add-masquerade --permanent
- firewall-cmd --add-port=3306/tcp
- firewall-cmd --reload
- systemctl restart docker
- Change your network settings to Bridge Adapter
```
## Author
[**Rohit Ghumare**](https://github.com/rohitg00)

# About
This is a docker project built for need of automation and maintaining the server database in cases when local system crashes, Docker is too fast in building OS i.e. in few seconds this will help owncloud to maintain file storage safe.
