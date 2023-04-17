# hello-world-go-docker-minikube

This a simple microservice application written in Go language. https://go.dev/doc/ .
This application has been containerised using docker.
The application image can be found and downloaded from here: https://hub.docker.com/r/ranjanm1/hello-world-ranjan .
The application can be installed on minikube or any kubernetes cluster.
The complete code for application development and how to test on a local minikube cluster can be found in this github page.

## How to run on a local minikube cluster
### Pre-Requisites
- Docker desktop https://www.docker.com/products/docker-desktop/
- Minikube https://minikube.sigs.k8s.io/docs/start/
- Visual Studio Code https://code.visualstudio.com/

### Download dource code
Start Docker desktop
Download the repo from git: $ git clone https://github.com/ranjanm1/hello-world-go-docker-minikube.git
Open Visual Studio code and open the downloaded folder

### Prepare local kubernetes cluster using docker and minilube 

- start minikube
```
minikube start

```
- Move to the application folder and check all files downloaded correctly.
```
$ cd hello-world-go-docker-minikube/
$ ls -ltr
total 16
-rw-r--r-- 1 rupan 197609 7169 Apr 17 23:29 LICENSE
-rw-r--r-- 1 rupan 197609  798 Apr 17 23:29 README.md
drwxr-xr-x 1 rupan 197609    0 Apr 17 23:29 app-code/
drwxr-xr-x 1 rupan 197609    0 Apr 17 23:29 k8s-code/

$ cd k8s-code/
$ ls -ltr
total 2
-rw-r--r-- 1 rupan 197609 491 Apr 17 23:29 hello-world-app.yml
-rw-r--r-- 1 rupan 197609 399 Apr 17 23:29 hello-world-service.yml
```
Create the kubernetes deployment
```
$ kubectl create -f .
deployment.apps/hello-world-ranjan created
service/load-balancer-hello-world created

$ kubectl get deployment
NAME                 READY   UP-TO-DATE   AVAILABLE   AGE
hello-world-ranjan   5/5     5            5           38s

$ kubectl get svc
NAME                        TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)          AGE
kubernetes                  ClusterIP      10.96.0.1     <none>        443/TCP          14d
load-balancer-hello-world   LoadBalancer   10.99.89.56   <pending>     8080:32212/TCP   64s
```
Since we are runnng this application on Minikube which runs a virtual machine, a tunnel need to be created to access the application from the host machine. Use the service name to expose the application. The below command will automatically a browser window and the application can be accessed.

```
$ minikube service load-balancer-hello-world
|-----------|---------------------------|-------------|---------------------------|
| NAMESPACE |           NAME            | TARGET PORT |            URL            |
|-----------|---------------------------|-------------|---------------------------|
| default   | load-balancer-hello-world |        8080 | http://192.168.49.2:32212 |
|-----------|---------------------------|-------------|---------------------------|
🏃  Starting tunnel for service load-balancer-hello-world.
|-----------|---------------------------|-------------|------------------------|
| NAMESPACE |           NAME            | TARGET PORT |          URL           |
|-----------|---------------------------|-------------|------------------------|
| default   | load-balancer-hello-world |             | http://127.0.0.1:55646 |
|-----------|---------------------------|-------------|------------------------|
🎉  Opening service default/load-balancer-hello-world in default browser...
❗  Because you are using a Docker driver on windows, the terminal needs to be open to run it.
```
Addituionally, you can use the curl command -

Open another terminal window and curl
```
http://127.0.0.1:55646
```
# Clean Up
Run the following command to clean up the deployment. Make sure you are in the /k8s-code directory and run the below command.

```
$ kubectl delete -f .
deployment.apps "hello-world-ranjan" deleted
service "load-balancer-hello-world" deleted
```
Verify no deployment and load balancer service are present.
```
$ kubectl get deployments
No resources found in default namespace.

$ kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   14d
```

## Application Development
### Pre-Requisites
- Go Dev package https://go.dev/
