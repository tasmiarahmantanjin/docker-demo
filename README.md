### How to install docker?
  1. https://docs.docker.com/engine/install/ubuntu/

### Getting Started with coreSUP-Docker
1. Copy the following script to setup_docker.sh file and place it to project root directory. Edit Line 4, 8 & 12 of setup_docker.sh occording to your product folder location
```
#!/bin/bash
# COPY USERAPP TO PROJECT ROOT DIRECTORY OR USER THE RIGHT PATH TO DOCKERFILE
# cp -r /home/sandvik/workspace/core-carrier-dh/product/deploy/mcc_linux_debug/inst_mcc_CM/sandvik ./sandvik
cp -r path/product/deploy/mcc_linux_debug/inst_mcc_CM/sandvik ./sandvik

# COPY SANDVIK TO PROJECT ROOT OR USE THE RIGHT PATH TO DOCKERFILE
# cp -r /home/sandvik/workspace/core-carrier-dh/product/deploy/core_linux_debug/inst/ ./userapp
cp -r path/product/deploy/core_linux_debug/inst/ ./userapp

# Change the production base URL to localhost
# sed -i s/192.168.20.15:8000/127.0.0.1:8001/g /home/sandvik/workspace/DOCKER/mcc-coresup/userapp/maintenance/cfg/httpservice/webpage/configs/applicationConfig.json
sed -i s/192.168.20.15:8000/127.0.0.1:8001/g project_root_path/userapp/maintenance/cfg/httpservice/webpage/configs/applicationConfig.json

# Uncomment NO_BUS from mcon.cfg of uesrapp
sed -i 's/#NO_BUS/NO_BUS/g' userapp/maintenance/cfg/mcon/mcon.cfg
```

2. Create a Dockerfile in project root and paste the following code inside
```
FROM 138.103.127.156:8081/docker-local/core-runtime/core-runtime-x86:2.24

WORKDIR /

COPY sandvik /opt/sandvik

COPY userapp /opt/userapp

CMD bash -c "/opt/sandvik/start.sh && /opt/userapp/core_runtime_starter.sh"
```
3. Create a run.sh in project root and paste the following code inside
```
#!/bin/sh

# Run the project_setup scripts
bash workspace/core-carrier-dh/product/scripts/project/setup_docker.sh

# How to build it?
docker build -t coresup-docker-image .

# How to run it?
docker run -ti --name coresup-docker-container -p 21099:21098 -p 8001:8000 coresup-docker-image
```
4. Open browser inside sica dev-end and navigate 127.0.0.1:8001
5. Open Diagtool and Set Remote Config with 127.0.0.1:21099
6. TA-DA!!! You are good to try signal manipulations and the app is up and running with docker.

### Some Basic Docker command
1. List all the docker image
```
docker images
```
2. Delete docker image
```
docker rmi <IMAGE_NAME or IMAGE_ID>
```
3. List all the docker container
```
docker ps -a
```
4. Delete docker container
```
docker rm <CONTAINER_NAME or CONTAINER_ID>
```
5. Build a docker image
```
docker build -t <image_name_you_want_to_give> . (. means build the image in current directory)
```
6. Run a docker container with image
```
docker run -it -rm -p 4678(hostport):45678(container_port) --name <container_name_you_want_to_give> <image_name or Image_id>
# check the official docker documentation to have better idea about more flags: https://docs.docker.com/engine/reference/commandline/build/
```
7. Build a docker image from docker container
```
docker commit <CONTAINER_NAME> <image-name_you_want_to_give>
```
8. To open a new terminal indide docker container
```
docker exec -it <CONATINER_NAME> bash
```
9. Move to attach move if a container running in detach move
````
docker attach <CONTAINER_NAME>
````
10. Check the docker daemon status, stop, start, restart
````
sudo systemctl status docker
sudo systemctl start docker
sudo systemctl stop docker
sudo systemctl restart docker
````


### Some Nice Resources if you want to learn more about Docker (of course official docker documentation's are the BEST)
1. https://docs.docker.com/get-started/
2. https://docs.docker.com/engine/reference/run/
3. https://www.bogotobogo.com/DevOps/Docker/Docker-Cheat-Sheet.php
4. https://www.youtube.com/watch?v=fqMOX6JJhGo
4. https://github.com/wsargent/docker-cheat-sheet
