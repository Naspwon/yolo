YOLO Application Containerization with Docker

Overview 
This project demonstrates the containerization of an e-commerce web application using Docker and Docker Compose. The backend connects to a MongoDB database, and the frontend. The database, backend, and frontend are all run in separate containers orchestrated using Docker Compose.

STEPS:
Reverted back to commit (07000b7ad2e30a7fe4f03b3748bd544dcf28cf25). All development work was done on the main branch and merged to master branch.

Requirements 
Before you start, ensure that you have the following installed on your machine: 
Docker 
Docker-Compose 
Node.js (for local development/testing)

## Steps to Run application locally
git clone https://github.com/Naspwon/yolo.git 
cd yolo
Build and run application using docker-compose
docker-compose up --build

## Verify docker containers running
docker ps
CONTAINER ID   IMAGE                                   COMMAND                  CREATED         STATUS         PORTS                                                                                      NAMES
dcf669a7a147   missnayomie/brian-yolo-client:v1.0.0    "npm start"              6 minutes ago   Up 6 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp                                                  brian-yolo-client
ac34903c4d35   missnayomie/brian-yolo-backend:v1.0.0   "node server.js"         6 minutes ago   Up 6 minutes   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp                                                  brian-yolo-backend
e6458576537f   mongo                                   "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   0.0.0.0:27017->27017/tcp, :::27017->27017/tcp                                              app-mongo

The output should display the backend, frontend, and MongoDB containers running with their respective ports exposed: Backend (Node.js): Port 5000 Frontend (React): Port 3000 MongoDB: Port 27017

# Docker Images
nash@nash-HP-Spectre:~/Documents/Moringa/IP-2/yolo$ docker images
REPOSITORY                       TAG       IMAGE ID       CREATED          SIZE
missnayomie/brian-yolo-client    v1.0.0    e571bc06c777   8 minutes ago    303MB
missnayomie/brian-yolo-backend   v1.0.0    1f6457794630   11 minutes ago   80.5MB
mongo                            latest    d3295cd2d11f   9 days ago       854MB

## Docker-compose images
nash@nash-HP-Spectre:~/Documents/Moringa/IP-2/yolo$ docker-compose images
    Container                  Repository              Tag       Image Id       Size  
--------------------------------------------------------------------------------------
app-mongo            mongo                            latest   d3295cd2d11f   854.2 MB
brian-yolo-backend   missnayomie/brian-yolo-backend   v1.0.0   676a2cdb44a6   80.49 MB
brian-yolo-client    missnayomie/brian-yolo-client    v1.0.0   8f6d885a100a   302.9 MB

# Docker-network created
nash@nash-HP-Spectre:~/Documents/Moringa/IP-2/yolo$ docker network ls
NETWORK ID     NAME              DRIVER    SCOPE
20137a2c9049   app-net           bridge    local
1d23b9a0caf3   bridge            bridge    local
c708d20e9969   host              host      local
defd41aaad0d   jenkins-network   bridge    local


# Pushing to docker hub
-----------------------
nash@nash-HP-Spectre:~/Documents/Moringa/IP-2/yolo$ docker-compose push
Pushing brian-yolo-backend (missnayomie/brian-yolo-backend:v1.0.0)...
The push refers to repository [docker.io/missnayomie/brian-yolo-backend]
f87ddc535f3d: Pushed
ac7df82744aa: Pushed
1bb782f4943f: Pushed
0e182002b05f: Layer already exists
v1.0.0: digest: sha256:a344ef84fddda6e0fc05d8cccd23172ac102aa338105e81f360b02cbd279b9a1 size: 1157
Pushing brian-yolo-client (missnayomie/brian-yolo-client:v1.0.0)...
The push refers to repository [docker.io/missnayomie/brian-yolo-client]
09d8abcfe1f6: Pushed
5e238fd0d51c: Pushed
1bb782f4943f: Mounted from missnayomie/brian-yolo-backend
0e182002b05f: Layer already exists
v1.0.0: digest: sha256:373ab627220deae84d0c2ec61fb6deb97138603fc49b2b6cec77ff93e048785f size: 1158


# Snippets found in images-ip2 folder
yolo-backend-tags and yolo-client-tagging indicate the tags on dockerhub
web-image has a snippet of products added on the website and persist
docker-images-pushed: snippet of images pushed to dockerhub

# To access the application
Frontend: Open your browser and navigate to http://localhost:3000.
To stop and remove the running containers
docker-compose down