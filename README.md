# miageGR1

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
