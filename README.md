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

Logout and login. Or try `newgrp docker` in the terminal where to test or use docker.

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

If the image is < none>  the above won't work, use:

    docker rmi $(docker images -f "dangling=true" -q) --force

(tested ok)
See: https://stackoverflow.com/questions/33913020/docker-remove-none-tag-images

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

### Delete all container:

    docker container prune

### clear build cache

    docker builder prune -a

### setup mirror to update ubuntu 

configure Docker to use a mirror for pulling Ubuntu images, you need to modify the daemon.json file and restart the Docker daemon. Specifically, you'll add a registry-mirrors entry to this file, listing the desired mirror URL(s)
    Edit daemon.json:
        Open the Docker daemon configuration file: sudo nano /etc/docker/daemon.json
        If the file doesn't exist, create it.
        Add the registry-mirrors key and a list of your desired mirror URLs: 


   {
     "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn/"]  // Example: USTC mirror
   }

We can add multiple mirrors, separated by commas: 


   {
     "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn/", "https://registry-1.docker.io/"]
   }

Restart the Docker daemon:
    
    sudo systemctl restart docker
see:
https://stackoverflow.com/questions/62034545/dockerfile-how-to-set-apt-mirror-based-on-the-ubuntu-release
and
https://docs.docker.com/docker-hub/image-library/mirror/#:~:text=Configure%20the%20Docker%20daemon%20Either%20pass%20the,and%20value%2C%20to%20make%20the%20change%20persistent.

Not tested!


## Problems, issues

### permission denied while trying to connect to the Docker daemon socket

    $ docker image ls
    permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Head "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied
    $ sudo chmod 666 /var/run/docker.sock
    $ docker image ls
    REPOSITORY   TAG                        IMAGE ID       CREATED        SIZE
    sdkmanager   2.0.0.11405-Ubuntu_22.04   16af7c06fd7a   7 months ago   904MB
    sdkmanager   latest                     16af7c06fd7a   7 months ago   904MB



