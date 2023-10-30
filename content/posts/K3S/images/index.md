



What you’ll need 
2 CPUs or more
2GB of free memory
20GB of free disk space
Internet connection
Container or virtual machine manager, such as: Docker, QEMU, Hyperkit, Hyper-V, KVM, Parallels, Podman, VirtualBox, or VMware Fusion/Workstation






## install 

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```
```
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

### Start your cluster
```
minikube start
```
Interact with your cluster
```
kubectl get po -A
```
or
```
minikube kubectl -- get po -A
```
You can also make your life easier by adding the following to your shell config:
```
alias kubectl="minikube kubectl --"
```



Create a sample deployment and expose it on port 8080:
```
kubectl create deployment hello-minikube --image=docker.io/nginx:1.23
kubectl expose deployment hello-minikube --type=NodePort --port=80
```

test
```
kubectl get deployment
```
```
kubectl get describe deployment hello-minicube
```
```
kubectl get replica set
```
```
kubectl get pod
```




```
kubectl get services hello-minikube
```



Alternatively, use kubectl to forward the port:
```
kubectl port-forward service/hello-minikube 7080:8080
```
Tada! Your application is now available at http://localhost:7080/.

see kubectl


Pause Kubernetes without impacting deployed applications:
```
minikube pause
```
Unpause a paused instance:
```
minikube unpause
````
Halt the cluster:
```
minikube stop
```
Change the default memory limit (requires a restart):
```
minikube config set memory 9001
```
Browse the catalog of easily installed Kubernetes services:
```
minikube addons list
```
Create a second cluster running an older Kubernetes release:
```
minikube start -p aged --kubernetes-version=v1.16.1
````
Delete all of the minikube clusters:
```
minikube delete --all
```