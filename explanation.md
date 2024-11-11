## Explanation of Implementation
1. Choice of Kubernetes Objects for Deployment
For this project, I used StatefulSets to deploy MongoDB because it requires stable storage and consistent network names.
    I used Deployments for the backend and client services because they don’t need stable network identities and can easily be scaled up or down without worrying about data persistence. In short, I used StatefulSets for MongoDB to ensure it has stable storage and network names, and Deployments for the backend and client services since they don’t need that stability.

2. How Pods Are Exposed to Internet Traffic
To make the pods accessible from the internet, I used a LoadBalancer service. This automatically gives an external IP address that routes traffic to the backend service.
    
3. Use of Persistent Storage
For MongoDB, I used Persistent Storage through StatefulSets and Persistent Volume Claims (PVCs).
    MongoDB needs persistent storage to keep data safe even if the pod is restarted. StatefulSets automatically create PVCs for each MongoDB pod, which are then linked to Persistent Volumes (PVs).
    For the backend and client services, I didn’t use persistent storage because they are stateless, meaning they don’t need to store data permanently. I deployed these using Deployments, which don’t require PVCs. In conclusion, persistent storage was used only for MongoDB, while the backend and client services don’t need it.

4. Git Workflow
I worked on the main branch and later merged my changes to the master branch.
    I updated the README.md file to explain the steps I took to set up the application after completing IP2 and IP3.

5. Successful Running of the Applications and Debugging Steps
To make sure everything was working, I tested the deployment by checking:
    Backend Connectivity: I confirmed that the backend was connected to MongoDB and could handle requests.
    Service Exposure: I tested that the backend was accessible from the internet via the LoadBalancer service.

If any issues came up, I checked:
    Logs: I used kubectl logs <pod-name> to check the logs of the backend and MongoDB pods for any connection or configuration issues.
    Pod Health: I used kubectl describe pod <pod-name> to check the pod status and identify issues like crashes or resource problems.
    Service Configuration: I made sure the service was correctly set up to expose the pods to the internet.

If the application wasn’t working as expected, I restarted the pods, fixed environment variables, or updated Kubernetes configurations.
For Docker images, I followed a version-based tagging system to easily track different versions. The format I used was:
    <repository>:<version>, e.g., gcr.io/yolo-devops/missnayomie/brian-yolo-backend:v1.0.0.


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
