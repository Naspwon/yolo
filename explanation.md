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

Steps to Run application locally
git clone https://github.com/Naspwon/yolo.git 
cd yolo
Build and run application using docker-compose
docker-compose up --build

Verify docker containers running
docker ps
CONTAINER ID   IMAGE                                   COMMAND                  CREATED         STATUS         PORTS                                                                                      NAMES
dcf669a7a147   missnayomie/brian-yolo-client:v1.0.0    "npm start"              6 minutes ago   Up 6 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp                                                  brian-yolo-client
ac34903c4d35   missnayomie/brian-yolo-backend:v1.0.0   "node server.js"         6 minutes ago   Up 6 minutes   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp                                                  brian-yolo-backend
e6458576537f   mongo                                   "docker-entrypoint.s…"   6 minutes ago   Up 6 minutes   0.0.0.0:27017->27017/tcp, :::27017->27017/tcp                                              app-mongo

The output should display the backend, frontend, and MongoDB containers running with their respective ports exposed: Backend (Node.js): Port 5000 Frontend (React): Port 3000 MongoDB: Port 27017

Docker Images
nash@nash-HP-Spectre:~/Documents/Moringa/IP-2/yolo$ docker images
REPOSITORY                       TAG       IMAGE ID       CREATED          SIZE
missnayomie/brian-yolo-client    v1.0.0    e571bc06c777   8 minutes ago    303MB
missnayomie/brian-yolo-backend   v1.0.0    1f6457794630   11 minutes ago   80.5MB
mongo                            latest    d3295cd2d11f   9 days ago       854MB
