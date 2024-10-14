
## YOLO Application Containerization with Docker
Overview
This project demonstrates the containerization of an e-commerce web application using Docker and Docker Compose. The backend connects to a MongoDB database, and the frontend. The database, backend, and frontend are all run in separate containers orchestrated using Docker Compose.

All development work was done on the main branch and merged to the master branch before final deployment.

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
nash@nash-HP-Spectre:~/Documents/Moringa/yolo$ docker ps
CONTAINER ID   IMAGE                                   COMMAND                  CREATED         STATUS         PORTS                                                                                      NAMES
5675df2a6877   missnayomie/brian-yolo-client:v1.0.1    "npm start"              9 minutes ago   Up 9 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp                                                  brian-yolo-client
21a7c7502858   missnayomie/brian-yolo-backend:v1.0.1   "node server.js"         9 minutes ago   Up 9 minutes   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp                                                  brian-yolo-backend
07a42fdbff89   mongo                                   "docker-entrypoint.s…"   9 minutes ago   Up 9 minutes   0.0.0.0:27017->27017/tcp, :::27017->27017/tcp                                              app-mongo

The output should display the backend, frontend, and MongoDB containers running with their respective ports exposed:
Backend (Node.js): Port 5000
Frontend (React): Port 3000
MongoDB: Port 27017


## Docker images
nash@nash-HP-Spectre:~/Documents/Moringa/yolo$ docker images
REPOSITORY                       TAG       IMAGE ID       CREATED             SIZE
missnayomie/brian-yolo-client    v1.0.0    2dd1a63d75b6   About an hour ago   298MB
missnayomie/brian-yolo-backend   v1.0.0    c0d474403fe1   About an hour ago   81.3MB



## docker-compose images
nash@nash-HP-Spectre:~/Documents/Moringa/yolo$ docker-compose ps
       Name                    Command             State                      Ports                    
-------------------------------------------------------------------------------------------------------
app-mongo            docker-entrypoint.sh mongod   Up      0.0.0.0:27017->27017/tcp,:::27017->27017/tcp
brian-yolo-backend   node server.js                Up      0.0.0.0:5000->5000/tcp,:::5000->5000/tcp    
brian-yolo-client    npm start                     Up      0.0.0.0:3000->3000/tcp,:::3000->3000/tcp 


## Docker network created: nash-network
nash@nash-HP-Spectre:~/Documents/Moringa/yolo$ docker network ls
NETWORK ID     NAME              DRIVER    SCOPE
8ca0b0744fad   app-net           bridge    local
8f463adaa301   bridge            bridge    local
c708d20e9969   host              host      local
defd41aaad0d   jenkins-network   bridge    local
f79a6e00204f   nash-network      bridge    local
56ee508d410a   none              null      local


## Pushing to docker hub
nash@nash-HP-Spectre:~/Documents/Moringa/yolo$ docker compose push
WARN[0000] /home/nash/Documents/Moringa/yolo/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
[+] Pushing 6/9
 ✔ app-ip-mongo Skipped                                                                                                                                                                                       0.0s 
 ✔ Pushing missnayomie/brian-yolo-client:v1.0.0: 3ecaca46c20f Pushed                                                                                                                                        149.8s 
 ✔ Pushing missnayomie/brian-yolo-backend:v1.0.0: 7eb0cb0bde85 Pushed                                                                                                                                        45.9s 
 ✔ Pushing missnayomie/brian-yolo-client:v1.0.0: 8c565ee6b15e Pushed                                                                                                                                        124.1s 
 ✔ Pushing missnayomie/brian-yolo-backend:v1.0.0: 20104517976a Pushed                                                                                                                                       116.7s 
 ✔ Pushing missnayomie/brian-yolo-client:v1.0.0: 9c7a032f10f1 Pushed                                                                                                                                         35.3s 
 ⠙ Pushing missnayomie/brian-yolo-backend:v1.0.0: 9c7a032f10f1 Mounted from missnayomie/brian-yolo-client                                                                                                   158.1s 
 ⠙ Pushing missnayomie/brian-yolo-client:v1.0.0: 0e182002b05f Mounted from library/alpine                                                                                                                   158.1s 
 ⠙ Pushing missnayomie/brian-yolo-backend:v1.0.0: 0e182002b05f Mounted from missnayomie/brian-yolo-client  


Snippets found in product-image folder
dockerhub-image: contains a screenshot of the image pushed to docker hub
products-image: contains a screenshot of products successfully added and persisted.
clientimagewithtagging and backendimage with tagging both show the versions used

## To access the application
Frontend: Open your browser and navigate to http://localhost:3000.

## To stop and remove the running containers
docker-compose down