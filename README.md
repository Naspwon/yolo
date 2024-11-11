## YOLO Application Containerization with Docker, Ansible, Terraform
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

### ANSIBLE AUTOMATION
add a vagrant vm 
 - vagrant box add geerlingguy/ubuntu2004
 - vagrant box list
 - vagrant init geerlingguy/ubuntu2004
 - vagrant up
 - vagrant status
 - vagrant ssh-config
configure vagrant file to specify ansible playbook location
implement ansible tasks using ansible galaxy init
run vagrant provision

## Run the file using  vagrant provision
    default: Running ansible-playbook...

PLAY [Set up and run Yolo e-commerce app] **************************************

TASK [Gathering Facts] *********************************************************
[WARNING]: Platform linux on host default is using the discovered Python
interpreter at /usr/bin/python3.8, but future installation of another Python
interpreter could change the meaning of that path. See
https://docs.ansible.com/ansible-
core/2.17/reference_appendices/interpreter_discovery.html for more information.
ok: [default]

TASK [docker : Install Python3 and pip3] ***************************************
ok: [default]

TASK [docker : Update pip] *****************************************************
ok: [default]

TASK [docker : Install 'Docker SDK for Python' using pip] **********************
ok: [default]

TASK [docker : Add Docker GPG key] *********************************************
ok: [default]

TASK [docker : Add Docker official repository] *********************************
ok: [default]

TASK [docker : Install Docker packages] ****************************************
ok: [default]

TASK [docker : Start Docker service] *******************************************
ok: [default]

TASK [docker : Add current user to docker group] *******************************
ok: [default]

TASK [docker : Download Docker Compose binary] *********************************
ok: [default]

TASK [docker : Verify Docker Compose installation] *****************************
changed: [default]

TASK [docker : Clone GitHub repository] ****************************************
changed: [default]

TASK [docker : Check if docker-compose.yml exists] *****************************
ok: [default]

TASK [docker : Copy docker-compose.yml] ****************************************
skipping: [default]

TASK [docker : Ensure correct permissions for Docker Compose file] *************
ok: [default]

TASK [docker : Run Docker Compose pull] ****************************************
changed: [default]

TASK [docker : Deploy Docker Compose] ******************************************
changed: [default]

TASK [frontend : Pull image from repository] ***********************************
ok: [default]

TASK [frontend : Build frontend Docker image] **********************************
changed: [default]

TASK [mongo : Ensure mynetwork exists] *****************************************
ok: [default]

TASK [mongo : Ensure MongoDB data volume exists] *******************************
changed: [default]

TASK [mongo : Run MongoDB container] *******************************************
changed: [default]

TASK [backend : Pull image from repository] ************************************
ok: [default]

TASK [backend : Create Node.js Backend Container] ******************************
changed: [default]

TASK [backend : Build backend Docker image] ************************************
changed: [default]

PLAY RECAP *********************************************************************
default                    : ok=24   changed=9    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0  


### pulled images in vagrant machine
vagrant@vagrant:~/YoloApp$ docker images
REPOSITORY                 TAG       IMAGE ID       CREATED          SIZE
missnayomie/yolo-client    v1.0.0    7a9c91f89c4b   9 minutes ago    380MB
missnayomie/yolo-backend   v1.0.0    6bed1c3bae60   12 minutes ago   144MB
mongo                      latest    77c59b638412   2 days ago       855MB


vagrant@vagrant:~/YoloApp$ docker ps
CONTAINER ID   IMAGE                             COMMAND                  CREATED         STATUS         PORTS                                           NAMES
4191cec5282a   missnayomie/yolo-client:v1.0.0    "docker-entrypoint.s…"   8 minutes ago   Up 8 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp       yolo-client
4b141fbf8c54   missnayomie/yolo-backend:v1.0.0   "docker-entrypoint.s…"   8 minutes ago   Up 8 minutes   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp       yolo-backend
b0da010c96ac   mongo:latest                      "docker-entrypoint.s…"   8 minutes ago   Up 8 minutes   0.0.0.0:27017->27017/tcp, :::27017->27017/tcp   yoloapp_mongodb_1

vagrant@vagrant:~/YoloApp$ docker-compose images
    Container              Repository           Tag       Image Id       Size  
-------------------------------------------------------------------------------
yolo-backend        missnayomie/yolo-backend   v1.0.0   6bed1c3bae60   143.8 MB
yolo-client         missnayomie/yolo-client    v1.0.0   7a9c91f89c4b   379.6 MB
yoloapp_mongodb_1   mongo                      latest   77c59b638412   855.2 MB


## Orchestrating to GCP
Create the GKE Cluster
- I set my desired region and zone
	`gcloud config set compute/zone us-central1-a`
- Created the GKE Cluster
	gcloud container clusters create yolo-cluster \
  --num-nodes=3 \
  --machine-type=e2-medium \
  --enable-autoscaling --min-nodes=1 --max-nodes=3 \
  --zone=us-central1-a


nash@nash-HP-Spectre:~/Documents/Moringa/yolo$ kubectl get nodes -o wide
NAME                                          STATUS   ROLES    AGE    VERSION               INTERNAL-IP   EXTERNAL-IP      OS-IMAGE                             KERNEL-VERSION   CONTAINER-RUNTIME
gke-yolo-cluster-default-pool-c10c8268-8vzw   Ready    <none>   131m   v1.30.5-gke.1355000   10.128.0.3    35.222.181.39    Container-Optimized OS from Google   6.1.100+         containerd://1.7.22
gke-yolo-cluster-default-pool-c10c8268-fkgj   Ready    <none>   131m   v1.30.5-gke.1355000   10.128.0.5    34.172.195.15    Container-Optimized OS from Google   6.1.100+         containerd://1.7.22
gke-yolo-cluster-default-pool-c10c8268-s86p   Ready    <none>   131m   v1.30.5-gke.1355000   10.128.0.4    34.170.249.167   Container-Optimized OS from Google   6.1.100+         containerd://1.7.22



- Enabled K8s Engine API
    `gcloud services enable container.googleapis.com`
- Authenticated to my google account after downloading gcloud sdk
	`gcloud auth login`
- Set up my project here
	`gcloud config set project yolo-devops`
- configured docker
	`gcloud auth configure-docker`
- Pulled my images from dockerhub
	`docker pull missnayomie/brian-yolo-backend:v1.0.0`
- Retagged them to:
	`docker tag missnayomie/brian-yolo-backend:v1.0.0 gcr.io/yolo-devops/missnayomie/brian-yolo-backend:v1.0.0`
- Pushed the new image name
	`docker push gcr.io/yolo-devops/missnayomie/brian-yolo-backend:v1.0.0`

	`docker pull missnayomie/brian-yolo-client:v1.0.0`
	`docker tag missnayomie/brian-yolo-client:v1.0.0 gcr.io/yolo-devops/missnayomie/brian-yolo-client:v1.0.0`
	`docker push gcr.io/yolo-devops/missnayomie/brian-yolo-client:v1.0.0`
	`docker tag mongo gcr.io/yolo-devops/mongodb-image:v1.0.0`
	`docker push gcr.io/yolo-devops/mongodb-image:v1.0.0`
- Created a new namespace
    `kubectl create namespace yolo-devops`


## Applied Kubernetes Manifests
nash@nash-HP-Spectre:~/Documents/Moringa/yolo$ kubectl apply -f manifests/yolo-frontend.yaml 
deployment.apps/yolo-frontend created
service/yolo-frontend created
nash@nash-HP-Spectre:~/Documents/Moringa/yolo$ kubectl apply -f manifests/yolo-backend.yaml 
deployment.apps/yolo-backend created
service/yolo-backend created
nash@nash-HP-Spectre:~/Documents/Moringa/yolo$ kubectl apply -f manifests/mongodb-statefulset.yaml 
statefulset.apps/mongodb created
service/mongodb created
nash@nash-HP-Spectre:~/Documents/Moringa/yolo$ kubectl apply -f manifests/network-policy.yaml 
networkpolicy.networking.k8s.io/allow-same-namespace created


## Monitor Deployment
nash@nash-HP-Spectre:~/Documents/Moringa/yolo$ kubectl get deployment
NAME            READY   UP-TO-DATE   AVAILABLE   AGE
yolo-backend    1/1     1            0           47s
yolo-frontend   1/1     1            1           65s

nash@nash-HP-Spectre:~/Documents/Moringa/yolo$ kubectl get svc
NAME            TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)          AGE
mongodb         ClusterIP      34.118.235.172   <none>          27017/TCP        45s
yolo-backend    LoadBalancer   34.118.237.3     35.222.181.39   5000:31117/TCP   55s
yolo-frontend   LoadBalancer   34.118.227.222   34.55.188.56    80:30217/TCP     73s

nash@nash-HP-Spectre:~/Documents/Moringa/yolo$ kubectl get pods
NAME                             READY   STATUS             RESTARTS      AGE
mongodb-0                        1/1     Running            0             54s
yolo-backend-fcfb5f6fb-m7926     1/1     Running            0             64s
yolo-frontend-84dfbfff7c-m8j6t   1/1     Running            0             82s

## Access the Application
Once deployments are up and running, access application using the external IP or domain name associated with your GKE LoadBalancer.
Application Accessible via: http://34.55.188.56/
