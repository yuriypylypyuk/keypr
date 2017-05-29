# KEYPR test assignment for DevOps


This project is intended to be installed under Linux, feel free to use it with other OS, just follow installation guite on [minikube](https://github.com/kubernetes/minikube/releases) and [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) pages
In order to start this project, you'll need kubectl and minikube
If you have VirtualBox installed, just follow this guide, if you're using another VM driver, add the appropriate `--vm-driver=xxx` flag to `minikube start`


Install kubectl binary via curl

`curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl`

Make the kubectl binary executable.

`chmod +x ./kubectl`

Move the binary in to your PATH.

`sudo mv ./kubectl /usr/local/bin/kubectl`


Install minikube via curl.

`curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.19.0/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/`

Start VM and let it download ISO + set your kubectl to face minikube.

`minikube start`

Clone the repository containing kubernetes config and Dockerfiles used for this project.

`git clone https://github.com/yuriypylypyuk/keypr.git`

Create kubernetes deployments.

`kubectl create -f keypr/kubernetes/grav_deployment.yaml`

Create Kubernetes services.

`kubectl create -f keypr/kubernetes/grav_services.yaml`

Check the process of startup via `kubectl get pods` untill you see something like 

`NAME                                READY     STATUS    RESTARTS   AGE`

`nginx-deployment-3097956576-t4ngb   1/1       Running   0          1m`

`php7-deployment-2208436050-t8fdl    1/1       Running   0          1m`

To verify, that project is up and running use `curl $(minikube service nginx-service --url)`
Additionally, you can type in the console `minikube service nginx-service --url` and point the browser to the responce of that command, it should be something like `http://192.168.99.100:30135`

To handle the data persistance, you would need to remove

`COPY src/ /var/www/html/`

`RUN cd /var/www/html && bin/grav install`

from the `/docker/php/Dockerfile` and add 

`        volumeMounts:`
`        - mountPath: /var/www/html`
`          name: src`
`      volumes:`
`      - name: src`
`        hostPath:`
`          path: /hosthome/$USERNAME/keypr/docker_php/src`

to the keypr/kubernetes/grav_deployment.yaml

as VirtualBox is passing /home/ of the host os, to the guest os
