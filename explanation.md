## 1. Choice of Base Image
The base image used to build the containers is `node:16-alpine3.16`, based on Alpine Linux distribution, known for its minimal size. 
 Used 
 1. Client:`node:16-alpine3.16`
 2. Backend: `node:16-alpine3.16`
 3. Mongo : `mongo`: For the database container
       

## 2. Dockerfile directives used in the creation and running of each container.
 Two dockerfiles were created: one for the Client(frontend) and the other one for the backend.

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

# Copy only the necessary files from the build stage thus minimising final image size
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
The (docker-compose.yml) configures the networking configuration for the project, for all containers to communicate with one another. It includes the allocation of application ports. The relevant sections are as follows:


```
services:
  backend:
    # ...
    ports:
      - "5000:5000"
    networks:
      - mynetwork

  client:
    # ...
    ports:
      - "3000:3000"
    networks:
      - mynetwork
  
  mongodb:
    # ...
    ports:
      - "27017:27017"
    networks:
      - mynetwork

networks:
  mynetwork:
    name: app-net
    driver: bridge
```
In this configuration, the backend container is mapped to port 5000 of the host, the client container is mapped to port 3000 of the host, and mongodb container is mapped to port 27017 of the host. All containers are connected to the nash-net bridge network.

## 4.  Docker Compose Volume Definition and Usage
The Docker Compose file includes volume definitions for MongoDB data storage. This volume ensures that MongoDB data is not lost even if the container is stopped or removed:

yaml

```
volumes:
  mongodata:  # Define Docker volume for MongoDB data
    driver: local

```
This prevents data loss by persisting it outside of the container

## 5. Git Workflow to achieve the task

To achieve the task the following git workflow was used:

1. Fork the repository from the original repository.
2. Clone the repo: `git clone https://github.com/Naspwon/yolo.git`
3. Create a .gitignore file to exclude unnecessary     files and directories from version control.
4. Switched to main branch
`git checkout 07000b7ad2e30a7fe4f03b3748bd544dcf28cf25`
`git checkout -b main`
`git push origin main`
5. Added Dockerfile for the client to the repo:
`git add client/Dockerfile`
6. Add Dockerfile for the backend to the repo:
`git add backend/dockerfile`
7. Committed the changes:
`git commit -m "Added Dockerfiles"`
8. Added docker-compose file to the repo:
`git add docker-compose.yml`
9. Committed the changes:
`git commit -m "Added docker-compose file"`
10. Pushed the files to github:
`git push `
11. Built the client and backend images:
`docker-compose up --build`
12. Pushed the built imags to docker registry:
`docker compose push`
13. Merge back to master
`git checkout master`
`git pull origin master`
`git merge main`
`git push origin master`