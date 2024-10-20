<<<<<<< HEAD
## 1. Choice of Base Image
 The base image used to build the containers is `node:16-alpine3.16`. It is derived from the Alpine Linux distribution, making it lightweight and compact. 
 Used 
 1. Client:`node:16-alpine3.16`
 2. Backend: `node:16-alpine3.16`
 3.Mongo : `mongo:6.0 `
       

## 2. Dockerfile directives used in the creation and running of each container.
 I used two Dockerfiles. One for the Client and the other one for the Backend.

**Client Dockerfile**

```
# Build stage
FROM node:16-alpine3.16 as build-stage

# Set the working directory inside the container
WORKDIR /client

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies and clears the npm cache and removes any temporary files
RUN npm install --only=production && \
    npm cache clean --force && \
    rm -rf /tmp/*

# Copy the rest of the application code
COPY . .

# Build the application and  remove development dependencies
RUN npm run build && \
    npm prune --production

# Production stage
FROM node:16-alpine3.16 as production-stage

WORKDIR /client

# Copy only the necessary files from the build stage
COPY --from=build-stage /client/build ./build
COPY --from=build-stage /client/public ./public
COPY --from=build-stage /client/src ./src
COPY --from=build-stage /client/package*.json ./

# Set the environment variable for the app
ENV NODE_ENV=production

# Expose the port used by the app
EXPOSE 3000

# Prune the node_modules directory to remove development dependencies and clears the npm cache and removes any temporary files


# Start the application
CMD ["npm", "start"]

```
**Backend Dockerfile**

```
# Set base image
FROM node:16-alpine3.16

# Set the working directory
WORKDIR /backend

# Copy package.json and package-lock.json to the container
COPY package*.json ./

# Install dependencies and clears the npm cache and removes any temporary files
RUN npm install --only=production && \
    npm cache clean --force && \
    rm -rf /tmp/*

# Copy the rest of the application code
COPY . .

# Set the environment variable for the app
ENV NODE_ENV=production

# Expose the port used by the app
EXPOSE 5000

# Prune the node_modules directory to remove development dependencies and clears the npm cache and removes any temporary files
RUN npm prune --production && \
    npm cache clean --force && \
    rm -rf /tmp/*

# Start the application
CMD ["npm", "start"]

```

## 3. Docker Compose Networking
The (docker-compose.yml) defines the networking configuration for the project. It includes the allocation of application ports. The relevant sections are as follows:


```
services:
  backend:
    # ...
    ports:
      - "5000:5000"
    networks:
      - yolo-network

  client:
    # ...
    ports:
      - "3000:3000"
    networks:
      - yolo-network
  
  mongodb:
    # ...
    ports:
      - "27017:27017"
    networks:
      - yolo-network

networks:
  yolo-network:
    driver: bridge
```
In this configuration, the backend container is mapped to port 5000 of the host, the client container is mapped to port 3000 of the host, and mongodb container is mapped to port 27017 of the host. All containers are connected to the yolo-network bridge network.


## 4.  Docker Compose Volume Definition and Usage
The Docker Compose file includes volume definitions for MongoDB data storage. The relevant section is as follows:

yaml

```
volumes:
  mongodata:  # Define Docker volume for MongoDB data
    driver: local

```
This volume, mongodb_data, is designated for storing MongoDB data. It ensures that the data remains intact and is not lost even if the container is stopped or deleted.

## 5. Git Workflow to achieve the task

To achieve the task the following git workflow was used:

1. Fork the repository from the original repository.
2. Clone the repo: `git@github.com:Maubinyaachi/yolo-Microservice.git`
3. Create a .gitignore file to exclude unnecessary     files and directories from version control.
4. Added Dockerfile for the client to the repo:
`git add client/Dockerfile`
5. Add Dockerfile for the backend to the repo:
`git add backend/dockerfile`
6. Committed the changes:
`git commit -m "Added Dockerfiles"`
7. Added docker-compose file to the repo:
`git add docker-compose.yml`
8. Committed the changes:
`git commit -m "Added docker-compose file"`
9. Pushed the files to github:
`git push `
10. Built the client and backend images:
`docker compose build`
11. Pushed the built imags to docker registry:
`docker compose push`
12. Deployed the containers using docker compose:
`docker compose up`

13. Created explanation.md file and modified it as the commit messages in the repo will explain.

=======
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
>>>>>>> main
