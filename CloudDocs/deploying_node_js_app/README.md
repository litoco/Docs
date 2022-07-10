# Deploying node js server to kubernetes

## Summary
1. [Background](#background)
2. [Steps to follow to deploy our node js server to kuberentes](#steps)


### Background
Before we go ahead and start our deployment, we will try to understand why we are taking the extra pain of deploying our server to kubernetes. 

Kubernetes is a container orchestration system. Which means that it can deploy, scale and manage multiple containers. Container are the executable unit of 
softwares. They are the running instance of an image (i.e. containers are built from these images). An image is an immutable file that contains all the 
necessary details required to run the application. An application is anything that you have build, for example here it is our node js server.

So to answer why we need a kuberenetes deployment lies in the benefits of using kuberentes. Suppose because of some problem our node js server that we had 
for serving the requests is down, this would mean a downtime which would result in loss of users. So to counter that we must have multiple instance of the 
node js server running. These instances must have some common resources like memory, CPU, network connections etc. So we must manage the resources effectively
across all these servers. If any of server goes down there has to be a mechanism to redirect traffic other server instances and while trying to solve this
faulty server to bring it back up and running.

Now we can see how complex it becomes to handles these multiple server instances. It would be really difficult to handle and manage these problems on our
own. This is where kubernetes comes into picture. It takes the heavy load of managing multiple instance of an application and provides us a simple mechanism
to interact with these server instances.

I hope the above argument answered question of why we need to deploy our nodejs server on kubernetes. Now we will go through the [steps](#steps) to do so.


### Steps
To deploy the node js server we need to do the following steps:
  - [Get the nodejs server running](#get-the-nodejs-server-running)
  - [Create a docker image of this node js server](#create-a-docker-image-of-the-nodejs-server-created-above)
  - [Push this image to docker registry](#push-image-to-the-docker-registry)
  - [Create a kubernetes deployment using the above image](#creating-kubernetes-deployment-with-the-pushed-image)
  - [Create a service to access the nodejs server](#creating-kubernetes-service-to-access-the-nodejs-server)
 
 
###### Get the nodejs server running

For the first we need to create and [verify that our node server is running](https://github.com/litoco/SmallApps/tree/main/nodejs-server).
Once you have successfully created your nodejs server go ahead you are good to follow the next steps.


###### Create a docker image of the nodejs server created above

To work with docker we need docker installed in your machine. Go to [this link](https://docs.docker.com/get-docker/) to install docker desktop for your 
machine.

We need to modify the server to listen ip address `0.0.0.0` to get it working with docker. You can read more about why is it required from 
[here](https://stackoverflow.com/questions/59179831/docker-app-server-ip-address-127-0-0-1-difference-of-0-0-0-0-ip).

In order to create the docker image we need a `dockerfile`. A `dockerfile` is a text file that contains instructions to build a docker `image`. More about 
this [here](https://docs.docker.com/engine/reference/builder).

So our `dockerfile` for creating a nodejs server image will look like the following

`Dockerfile`
```
FROM node:18-alpine3.15

WORKDIR /nodejs_server/

COPY ./package.json /nodejs_server/package.json

RUN npm install

COPY . /nodejs_server/

EXPOSE 5000

CMD ["npm", "start"]

```

After creating this `Dockerfile`. We need to  build the `image`, for that we will issue the command `docker build . -t nodejs_server_image:v1`\
Once that command is successful, we will have our `image` created. It can be verified by issuing the command `docker images`. Output 
of this command will show you the image with image name that you provided while building the image (here it is `nodejs_server_image:v1`).

We can verify that the image is running fine by issuing the command `docker run -p 8000:5000 nodejs_server_image:v1` and then requesting `http://0.0.0.0:8000`
from the browser.


###### Push image to the docker registry

To push the image to docker registry we need to tag it with our dockerhub_username.
Eg: my dockerhub username is `fortestingandplaying`, so I tagged my image by executing `docker tag nodejs_server_image:v1 fortestingandplaying/test_image`

Once tagged, we can push this image to the `docker registry`. A `docker registry` is the place where you store and 
distribute docker `images`. It can be both public or private. Here we will be using one such `docker registry` called `dockerhub`.

In the `Docker Desktop` app we need to login with our `dockerhub` credentials to be able to push our docker image that we created above. Once the login is
successful, we can push this image to `dockerhub` by executing the command `docker push fortestingandplaying/test_image`.

Now we will use this pushed image to run it on kubernetes.


###### Creating kubernetes deployment with the pushed image

To enable kuberentes from your `docker desktop` go to `settings > kubernetes > tick enable kuberentes checkbox`  and click on apply and restart button.

Once kubernetes is enabled we are ready to deploy our server on it. For that we need to go though the following steps one by one
1. Verify that kubernetes is running by executing `kubectl get nodes`. The output should show a `docker-desktop` node in `ready` state.
2. We will then create a namespace to keep our project organised. To do that we will execute the command `kubectl create namespace test-namespace`
3. Then we will create a deployment file called `deployment.yaml`
`deployment.yaml`
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nodejs-server-deployment
  labels:
    app: nodejs-server
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nodejs-server
  template:
    metadata:
      labels:
        app: nodejs-server
    spec:
      containers:
      - name: nodejs-server
        image: fortestingandplaying/test_image
        ports:
        - containerPort: 5000
```
4. Then we create this deployment in the `test-namespace` by running `kubectl apply -n test-namespace -f deployment.yaml`
5. To check if the server was running fine or not we can issue command `kubectl -n test-namespace get pods` then grab the name of any one pod, (Pod is the 
   one that runs the app inside itself) and check the its log by executing the command `kubectl -n test-namespace logs nodejs-server-deployment-6d5b88f49f-rxv9f`\
   We get output as `Server running at http://0.0.0.0:5000/`which means that our servers are working.
 
 
 ###### Creating kubernetes service to access the nodejs server
 
 In order to access this nodejs sever we need to create a service. To create a service we will create `service.yaml` file\
   `Service.yaml`
   ```
apiVersion: v1
kind: Service
metadata:
  name: nodejs-server-service
  labels:
    app: nodejs-server
spec:
  type: LoadBalancer
  selector:
    app: nodejs-server
  ports:
    - protocol: TCP
      name: http
      port: 5000
      targetPort: 5000
   ```
We then need to create this service in the `test-namespace` by executing `kubectl apply -n test-namespace -f service.yaml`

To verify whether the service got created successfully or not we need to issue the command `kubectl -n test-namespace get service`. The output should show
a service with name `nodejs-server-service`

And to access the nodejs server we can go to `http://localhost:5000/` in the browser. 


**Thats all folks, all the 3 instances of the server are running. So we have successfully deployed our nodejs server to kubernetes**
