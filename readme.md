# Docker
***
### Creating Dockerfile
```
FROM openjdk:8-jdk-alpine
MAINTAINER xxxx
COPY build/libs/rest-service-0.0.1-SNAPSHOT.jar demo-backend.jar
ENTRYPOINT ["java","-jar","/demo-backend.jar"]
```

### Build docker image (and tag)
```
docker build --tag=fsouayah/demo-backend:latest .
```

### Running docker image
```
docker run -p 8080:8080 fsouayah/demo-backend:latest
```

### Push image to docker registry
```
docker push fsouayah/demo-backend
```

# Kubernetes
***

### Create deployment for a pod running a container with provided docker image
```
kubectl create deployment backend-node --image=fsouayah/demo-backend:latest
```

![Kubernetes cluster](/assets/k8scluster_1.png)
![Create deployment steps](/assets/create_deployment_steps.png)

### Expose the Pod to the public internet using the kubectl expose command
```
kubectl expose deployment backend-node --type="NodePort" --port 8080
```

### Get details about the service
```
kubectl describe services/backend-node
```
![Describe service](/assets/describe_k8s_service.png)

### Create port-forward to access container
Because of a bug in minikube we need to create a port forward to be able to call the application.
In the example below, we are forwarding the container's port 8080 to the local port 8085.
In this case, we should call the URL http://localhost:8085/greeting
```
kubectl port-forward service/backend-node 8085:8080
```
![Port forwarding](/assets/service_port_forward.png)