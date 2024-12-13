# miageGR1

Start Docker.

Start the Kubernetes cluster:
```
minikube start --cpus=2 --memory=5000 --driver=docker
```

## Appropriation

Change the code.

### Build du code

Compile the Java project:
```
./gradlew build
```
Under Linux, or
```
.\gradlew build
```
Under Windows

Build the docker image:
```
docker build -t myservice .
```

Check the image:
```
docker images
```

Start the container:
```
docker run -p 4000:8080 -t myservice
```

8080 is the port of the web service, while 4000 is the port for accessing the container. Test the web service using a web browser: http://localhost:4000 It displays hello.

Ctrl-C to stop the Web Service.

Check the containerID:
```
docker ps
```

Stop the container:
```
docker stop containerID
```

### Publish the image to the Docker Hub

Retreive the image ID:
```
docker images
```

Tag the docker image:
```
docker tag imageID yourDockerHubName/imageName:version
```

Example: `docker tag 1dsd512s0d myDockerID/myservice:1`

Login to docker hub:
```
docker login
```
or
```
docker login http://hub.docker.com
```
or
```
docker login -u username -p password
```

Push the image to the docker hub:
```
docker push yourDockerHubName/imageName:version
```

Example: `docker push myDockerID/myservice:1`

### Change the image in the deployment file

https://github.com/charroux/miageGR2/blob/main/deployment.yml

### Deploy the app in Minikube
```
kubectl apply -f deployement.yml
```


## Install Istio
https://istio.io/latest/docs/setup/getting-started/

Select a release and download it (here the version 1.19)

```
cd istio-1.19.0
cd bin    
istioctl install --set profile=demo -y
cd ..   
```
Enable auto-injection of the Istio side-cars when the pods are started:
```
kubectl label namespace default istio-injection=enabled
```
Install the Istio addons (Kiali, Prometheus, Jaeger, Grafana):
```
kubectl apply -f samples/addons
```
## 
Enable auto-injection of the Istio side-cars when the pods are started:
```
kubectl label namespace default istio-injection=enabled
```

Configure Docker so that it uses the Kubernetes cluster:
```
minikube docker-env
eval $(minikube -p minikube docker-env)
eval $(minikube docker-env)  
```

Check the route of the microservice in the deployment fine (see Virtual Service):

https://github.com/charroux/miageGR2/blob/main/deployment.yml

Apply again the deployment:

Get an access to the gateway:
```
kubectl -n istio-system port-forward deployment/istio-ingressgateway 31380:8080
```

Test in your Web browser:
```
http://localhost:31380/miagegr2/cars
```

## Launch a workflow when the code is updated

Create a new branch:
```
git branch newcarservice
```
Move to the new branch:
```
git checkout newcarservice
```
Update the code and commit changes:
```
git commit -a -m "newcarservice"
```
Push the changes to GitHub:
```
git push -u origin newcarservice
```
Create a Pull request on GitHub and follow the workflow.

Merge the branch on Github is everything is OK.

Then pull the new main version:

```
git checkout main
```
```
git pull origin main
```

If necessary delete the branch:

```
git branch -D newcarservice
```
```
git push origin --delete newcarservice
```
