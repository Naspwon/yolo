## YOLO Application Containerization with Docker
Overview
This project demonstrates the containerization of an e-commerce web application using Docker and Docker Compose. The backend connects to a MongoDB database, and the frontend. The database, backend, and frontend are all run in separate containers orchestrated using Docker Compose.

All development work was done on the main branch and merged to the master branch.

Requirements
Before you start, ensure that you have the following installed on your machine:
Docker
Docker Compose
Node.js (for local development/testing)

## Steps to Run application locally
git clone https://github.com/Naspwon/yolo.git
cd yolo

## Build and run application using docker-compose
docker-compose up --build

## Verify docker containers running
nash@nash-HP-Spectre:~/Documents/Moringa/IP-2/yolo$ docker ps
CONTAINER ID   IMAGE                                   COMMAND                  CREATED          STATUS          PORTS                                                                                      NAMES
74151f72ca0e   missnayomie/brian-yolo-client:v1.0.0    "npm start"              27 minutes ago   Up 27 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp                                                  brian-yolo-client
e0620fc56116   missnayomie/brian-yolo-backend:v1.0.0   "node server.js"         27 minutes ago   Up 27 minutes   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp                                                  brian-yolo-backend
d2a609eede46   mongo                                   "docker-entrypoint.s…"   27 minutes ago   Up 27 minutes   0.0.0.0:27017->27017/tcp, :::27017->27017/tcp                                              app-mongo


The output should display the backend, frontend, and MongoDB containers running with their respective ports exposed:
Backend (Node.js): Port 5000
Frontend (React): Port 3000
MongoDB: Port 27017

# Docker compose images
nash@nash-HP-Spectre:~/Documents/Moringa/IP-2/yolo$ docker-compose images
    Container                  Repository              Tag       Image Id       Size  
--------------------------------------------------------------------------------------
app-mongo            mongo                            latest   d3295cd2d11f   854.2 MB
brian-yolo-backend   missnayomie/brian-yolo-backend   v1.0.0   40b8cbefb28b   81.35 MB
brian-yolo-client    missnayomie/brian-yolo-client    v1.0.0   2d6d3bd184b4   298.8 MB


## Docker images
nash@nash-HP-Spectre:~/Documents/Moringa/IP-2/yolo$ docker images
REPOSITORY                       TAG       IMAGE ID       CREATED              SIZE
missnayomie/brian-yolo-client    v1.0.0    2d6d3bd184b4   About a minute ago   299MB
missnayomie/brian-yolo-backend   v1.0.0    40b8cbefb28b   4 minutes ago        81.3MB


## docker-compose images
nash@nash-HP-Spectre:~/Documents/Moringa/yolo$ docker-compose ps
       Name                    Command             State                      Ports                    
-------------------------------------------------------------------------------------------------------
app-mongo            docker-entrypoint.sh mongod   Up      0.0.0.0:27017->27017/tcp,:::27017->27017/tcp
brian-yolo-backend   node server.js                Up      0.0.0.0:5000->5000/tcp,:::5000->5000/tcp    
brian-yolo-client    npm start                     Up      0.0.0.0:3000->3000/tcp,:::3000->3000/tcp 


## Docker network created: nash-network
nash@nash-HP-Spectre:~/Documents/Moringa/IP-2/yolo$ docker network ls
NETWORK ID     NAME              DRIVER    SCOPE
f87993566d46   app-net           bridge    local
1d23b9a0caf3   bridge            bridge    local


## Pushing to docker hub
nash@nash-HP-Spectre:~/Documents/Moringa/IP-2/yolo$ docker compose push
WARN[0000] /home/nash/Documents/Moringa/IP-2/yolo/docker-compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
[+] Pushing 8/9
 ✔ app-ip-mongo Skipped                                                                                                                                                                                                                                                    0.0s 
 ✔ Pushing missnayomie/brian-yolo-client:v1.0.0: d724e7cfd571 Pushed                                                                                                                                                                                                     266.0s 
 ✔ Pushing missnayomie/brian-yolo-client:v1.0.0: b499e649aaae Pushed                                                                                                                                                                                                     194.9s 
 ✔ Pushing missnayomie/brian-yolo-backend:v1.0.0: 1771558eda96 Pushed                                                                                                                                                                                                     68.8s 
 ✔ Pushing missnayomie/brian-yolo-client:v1.0.0: 70284fad86d7 Pushed                                                                                                                                                                                                      12.1s 
 ✔ Pushing missnayomie/brian-yolo-client:v1.0.0: 0e182002b05f Layer already exists                                                                                                                                                                                         8.1s 
 ✔ Pushing missnayomie/brian-yolo-backend:v1.0.0: 1b994be226d5 Pushed                                                                                                                                                                                                    213.8s 
 ⠹ Pushing missnayomie/brian-yolo-backend:v1.0.0: 70284fad86d7 Mounted from missnayomie/brian-yolo-client                                                                                                                                                                275.2s 
 ✔ Pushing missnayomie/brian-yolo-backend:v1.0.0: 0e182002b05f Layer already exists 


 
# Snippets found in images-ip2 folder
yolo-backend-tags and yolo-client-tagging indicate the tags on dockerhub
web-image has a snippet of products added on the website and persist
docker-images-pushed: snippet of images pushed to dockerhub

## To access the application
Frontend: Open your browser and navigate to http://localhost:3000.

## To stop and remove the running containers
docker-compose down
