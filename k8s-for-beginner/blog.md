Before understanding kubernetes let's first understand why something like this ever came to existence. So running applications in containers is not a new concept but after Docker it became easy and popular to deploy applications through containers. If you don’t know what Docker is, I suggest you read [this article](https://dev.to/keshavcodex/what-is-docker-2o49) first.
![Pets Vs Cattle](https://miro.medium.com/v2/resize:fit:1200/1*FfjAeWA69BaoaaDfQRnPjQ.png)
But when more and more containers started getting deployed it became difficult for the operations team to manage all those. So it’s like pets and cattle thing when you have a pet you give them a name show and them love and when it falls ill you start getting panic but when you have thousands of containers running you start treating them like cattles you don’t care much if one or two are not behaving properly you just replace them with new one. Here the kubernetes is one who will be managing those cattles i mean those containers and that is why it is called a container orchestration tool. 

## <u>History of Kubernetes</u>
![borg image](https://s3-eu-west-1.amazonaws.com/risingstack-resources/History+of+Kubernetes/borg-omega-and-kubernetes.jpg)
Kubernetes is not so old it was built by google so what happened is that google was actually running such container technology internally in the company for a really long time with the help of an engine called BORG. So with the knowledge of BORG they created another container orchestration tool kubernetes and donated it to CNCF in 2015.
[CNCF](https://landscape.cncf.io/) is the Cloud Native Computing Foundation and kubernetes was the first project to graduate from there. Kubernetes is also known as k8s (pronounced as kates) because eight letters are present in between ‘k’ and ‘s’ in kubernetes.

Now when k8s started getting popular most of the cloud providers started providing native support for it like GCP provides GKE(Google Kubernetes Engine), Azure provides AKS(Azure kubernetes service), AWS provides EKS(Elastic kubernetes service) and since this project was open source so a large community from around the world started improving it.

## <u>Setting up the environment</u>
Now we will set up the kubernetes in our local system. But if you face any issue always feel free to check the official documentation.
If you are running the k8s in your local system you need to set up kubectl which is a kubernetes command line tool that allows you to run commands against kubernetes clusters and minikube which provides a virtual environment for running clusters inside your computer.

- [Install kubectl in Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)
- [Install kubectl in Mac](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/)
- [Install kubectl in Window](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/) 

Now you need to install minikube but remember before using minikube you need to have docker or some similar container runtime environment inside your computer.
[Minikube setup guide](https://minikube.sigs.k8s.io/docs/start/)
And everytime you power on your computer you need to start your minikube with the `minikube start` command, you can stop this with the `minikube stop` command and you can check its status with the `minikube status` command.
![minikube start img](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hnnztycny5qks1h6pxph.png)

## <u>Getting started</u>
Kubernetes does not provide any options to build images so you need to build it on your own and deploy it to either [docker-hub](https://hub.docker.com) or [google container registry](https://cloud.google.com/container-registry) then k8s will pull it (download it) and start a container out of it.
When you deploy Kubernetes, you get a cluster and k8s never talks to containers directly, it is actually the node that hosts pods and containers runs inside the pod (we will learn about pods in later sections) and every application or service runs inside the container.
![Kubernetes Cluster Image](https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg)

## <u>Pods & Service</u>
There are two types of nodes. The first one is the worker node and the other is the service node.
Worker node's job to run the application lets say you created a react app so the worker node will be looking at the pods inside which the react app container is present. A node may be a virtual or physical machine, depending on the cluster. Each node is managed by the control plane and contains the services necessary to run Pods and a Service is a method for exposing a network application that is running as one or more Pods in your cluster. It means that the service node will provide a port to connect your react app from the worker pod. It handles all those networking stuff internally. You don’t need to make any changes in your application to work inside the kubernetes cluster.
So you will be creating two yaml files one for service and other for worker pod and the kind of those files will be service and pod respectively as shown below.
#### Get the configuration files [here](https://github.com/keshavcodex/k8s-blog/tree/main/k8s-for-beginner)
## <u>Commands</u>
to start those pods you need to run but remember to run minikube start first
`kubectl apply -f service.yaml`
`kubectl apply -f client-pod.yaml`

Now you can check if those pods are running via 
kubectl get pods 
And to check that website you can visit your `minikube ip colon 31010`, here 31010 port because we have declared that in the yaml file.
Since my minikube ip is 192.168.49.2 so i will visit 192.168.49.2:31010 and to check your minikube ip just type minikube ip in your terminal and if your ip is 172.156.1.1 then you need visit here `172.156.1.1:31010`
The nodeport range lies between (30000-32767) so you are free to use any by change these yaml files.

## <u>Dashboard</u>
![Dashboard](https://d33wubrfki0l68.cloudfront.net/349824f68836152722dab89465835e604719caea/6e0b7/images/docs/ui-dashboard.png)
Kubernetes also provides a dashboard to manage your cluster through UI, to visit the dashboard you need to first setup the dashboard, don’t worry it's very easy
First you need to deploy the dashboard with this command
`kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml`
Then you need to run these two yaml files of admin-user and role-binding and then start the dashboard service with proxy.
`kubectl apply -f admin-user.yaml`
![admin-user yaml file](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/nj2xhvcfeu1iwy6rtjeg.png)
`kubectl apply -f role-binding.yaml`

![role-binding file](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/us9vx7zv8s7mwheuyxit.png)
`kubectl proxy`
If you see “Starting to serve on 127.0.0.1:8001” then you can visit here [http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/)
and see the cluster but if it’s not showing starting to serve… then may be you have made a typo or may be you have not started minikube, you can start minikube with `minikube start`
You can check status with `minikube status`
And you can stop it with
`minikube stop`.
There are some more commands like `kubectl describe pod` which provides the details of pod.
And to delete a pod just write `kubectl delete pod`
You can replace this pod with the actual pod name means the metadata from yaml file, for e.g `kubectl delete task-client`
I you like this please put a comment below and I will bring more such blogs then you can follow me on [Linkedin](https://www.linkedin.com/in/keshavcodex), [twitter](https://twitter.com/keshavcodex/) and [github](https://github.com/keshavcodex). 
