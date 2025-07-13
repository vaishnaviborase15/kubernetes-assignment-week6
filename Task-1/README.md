# Week 6 Task 1 - Kubernetes on Minikube

```powershell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\Windows\system32> minikube start --driver=virtualbox

* minikube v1.35.0 on Microsoft Windows 11 Pro 10.0.22000.2538 Build 22000.2538
E0713 16:08:43.803551   12796 start.go:812] api.Load failed for minikube: filestore "minikube": Docker machine "minikube" does not exist. Use "docker-machine ls" to list machines. Use "docker-machine create" to add a new one.
E0713 16:08:43.806898   12796 start.go:812] api.Load failed for minikube: filestore "minikube": Docker machine "minikube" does not exist. Use "docker-machine ls" to list machines. Use "docker-machine create" to add a new one.
* Using the virtualbox driver based on existing profile
* Starting "minikube" primary control-plane node in "minikube" cluster
* Creating virtualbox VM (CPUs=2, Memory=2200MB, Disk=20000MB) ...|
! Failing to connect to https://registry.k8s.io/ from inside the minikube VM
* To pull new external images, you may need to configure a proxy: https://minikube.sigs.k8s.io/docs/reference/networking/proxy/
* Preparing Kubernetes v1.32.0 on Docker 27.4.0 ...
  - Generating certificates and keys ...
  - Booting up control plane ...
  - Configuring RBAC rules ...
* Configuring bridge CNI (Container Networking Interface) ...
  - Using image gcr.io/k8s-minikube/storage-provisioner:v5
* Enabled addons: storage-provisioner, default-storageclass
* Verifying Kubernetes components...

! C:\Windows\system32\kubectl.exe is version 1.18.0, which may have incompatibilities with Kubernetes 1.32.0.
  - Want kubectl v1.32.0? Try 'minikube kubectl -- get pods -A'
* Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

PS C:\Windows\system32> kubectl get nodes
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   73s   v1.32.0

PS C:\Windows\system32> mkdir c:\week6task1

    Directory: C:\

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        13-07-2025     16:14                week6task1


PS C:\Windows\system32> cd c:\week6task1

PS C:\week6task1> kubectl apply -f replicacontroller.yaml
replicationcontroller/rc-example created

PS C:\week6task1> kubectl get rc
NAME         DESIRED   CURRENT   READY   AGE
rc-example   2         2         0       14s

PS C:\week6task1> kubectl get pods
NAME               READY   STATUS              RESTARTS   AGE
rc-example-fkl7q   0/1     ContainerCreating   0          25s
rc-example-wpvcr   0/1     ContainerCreating   0          25s

PS C:\week6task1> kubectl apply -f replicaset.yaml
replicaset.apps/rs-example created

PS C:\week6task1> kubectl get rs
NAME         DESIRED   CURRENT   READY   AGE
rs-example   3         3         0       0s

PS C:\week6task1> kubectl get pods
NAME               READY   STATUS              RESTARTS   AGE
rc-example-fkl7q   0/1     ContainerCreating   0          71s
rc-example-wpvcr   0/1     ContainerCreating   0          71s
rs-example-cx4vn   0/1     ContainerCreating   0          0s
rs-example-g4882   0/1     ContainerCreating   0          0s
rs-example-tn9pm   0/1     ContainerCreating   0          0s


PS C:\week6task1> kubectl apply -f deploy.yaml
deployment.apps/deploy-example created

PS C:\week6task1> kubectl get deployments
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
deploy-example   0/3     3            0           8s

PS C:\week6task1> kubectl get pods
NAME                              READY   STATUS              RESTARTS   AGE
deploy-example-76fc958687-4sxfc   0/1     ContainerCreating   0          11s
deploy-example-76fc958687-6frj8   0/1     ContainerCreating   0          11s
deploy-example-76fc958687-xqpvk   0/1     ContainerCreating   0          11s
rc-example-fkl7q                  0/1     ContainerCreating   0          3m27s
rc-example-wpvcr                  0/1     ContainerCreating   0          3m27s
rs-example-cx4vn                  0/1     ContainerCreating   0          2m16s
rs-example-g4882                  0/1     ContainerCreating   0          2m16s
rs-example-tn9pm                  0/1     ContainerCreating   0          2m16s


PS C:\week6task1> kubectl get all
NAME                                  READY   STATUS              RESTARTS   AGE
pod/deploy-example-76fc958687-4sxfc   0/1     ContainerCreating   0          22s
pod/deploy-example-76fc958687-6frj8   0/1     ContainerCreating   0          22s
pod/deploy-example-76fc958687-xqpvk   0/1     ContainerCreating   0          22s
pod/rc-example-fkl7q                  0/1     ContainerCreating   0          3m38s
pod/rc-example-wpvcr                  0/1     ContainerCreating   0          3m38s
pod/rs-example-cx4vn                  0/1     ContainerCreating   0          2m27s
pod/rs-example-g4882                  0/1     ContainerCreating   0          2m27s
pod/rs-example-tn9pm                  0/1     ContainerCreating   0          2m27s

NAME                               DESIRED   CURRENT   READY   AGE
replicationcontroller/rc-example   2         2         0       3m38s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   10m

NAME                             READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/deploy-example   0/3     3            0           22s

NAME                                        DESIRED   CURRENT   READY   AGE
replicaset.apps/deploy-example-76fc958687   3         3         0       22s
replicaset.apps/rs-example                  3         3         0       2m27s

PS C:\week6task1> kubectl delete all --all
pod "deploy-example-76fc958687-4sxfc" deleted
pod "deploy-example-76fc958687-6frj8" deleted
pod "deploy-example-76fc958687-xqpvk" deleted
pod "rc-example-fkl7q" deleted
pod "rc-example-wpvcr" deleted
pod "rs-example-cx4vn" deleted
pod "rs-example-g4882" deleted
pod "rs-example-tn9pm" deleted
replicationcontroller "rc-example" deleted
service "kubernetes" deleted
deployment.apps "deploy-example" deleted
replicaset.apps "rs-example" deleted

PS C:\week6task1>

