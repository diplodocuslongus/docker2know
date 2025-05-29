# docker notes 

(from a beginner)

## installation

Use the convenience script here:

curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh

By default docker requires root, to grand access to any user we do:

    sudo groupadd docker

Add your user to the docker group.

    sudo usermod -aG docker $USER

Logout and login.

On ubuntu and derived, docker start on boot by default. On other linux, or if it doesn't, do:

    sudo systemctl enable docker.service
    sudo systemctl enable containerd.service

To disable docker from starting at boot:

    sudo systemctl disable docker.service
    sudo systemctl disable containerd.service

Verify with : 

    docker run hello-world

And we can use docker as rootless:

Source: https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script
https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user
https://docs.docker.com/engine/security/rootless/

## first steps

### pull an image

`docker image pull` can be replaced by `docker pull`!

docker image hello-world

Let it run so it download the image.

docker image pull ros:humble

Will pull the humble ros2 image.

### delete an image

docker image rm or docker rmi


docker image rm -f name_of_the_docker

ex docker image rm -f hello-world

### run a container

docker run -it ros:humble

-it: i = interactive, t = tty

### see a container list

docker container ls or docker ps

docker ps -a

Will list all the container, even the stopped ones.

### stop a container

ctl-d

Or docker container stop NAME_OF_THE_CONTAINER (from docker ps).

### remove a container

docker container rm NAME_OF_THE_CONTAINER

or docker rm NAME_OF_THE_CONTAINER

Delete all container:

docker container prune



## Problems, issues

### permission denied while trying to connect to the Docker daemon socket

    $ docker image ls
    permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Head "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied
    $ sudo chmod 666 /var/run/docker.sock
    $ docker image ls
    REPOSITORY   TAG                        IMAGE ID       CREATED        SIZE
    sdkmanager   2.0.0.11405-Ubuntu_22.04   16af7c06fd7a   7 months ago   904MB
    sdkmanager   latest                     16af7c06fd7a   7 months ago   904MB



