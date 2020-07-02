# Virtualization

---
## Vagrant
`vagrant init ubuntu/bionic64`
`vagrant box add centos/8`
`vagrant status`

`vagrant up`
`vagrant up <hostname>`
`vagrant suspend` stores state on RAM
`vagrant halt` stores state on Disk
`vagrant destroy -f` removes stored state

`vagrant ssh <hostname>`
`vagrant ssh-config`

### Boxes
https://app.vagrantup.com/boxes/search

### Synced Folders
"By default, Vagrant shares your project directory (remember, that is the one with the Vagrantfile) to the /vagrant directory in your guest machine."

### Vagrantfile
```
Vagrant.configure("3") do |config|
    # create mongo VM
    config.vm.define "mongo" do |mongo|
        mongo.vm.box = "bento/ubuntu-16.04"
        mongo.vm.provider "virtualbox" do |vb|
            vb.memory = "512"
        end
        mongo.vm.network "private_netowrk", ip: "10.8.0.20"
        mongo.vm.provision "file", source: "files/mongo.conf",  destination: "~/mongod/conf"
        # try provisioning with ansible instead of shell script
        mongo.vm.provision "shell", path: "provisioners/install-mongo.sh"
    end

    # create node VM
    config.vm.define "node" do |node|
        node.vm.box = "bento/ubuntu-16.04"
        node.vm.network "fowarded_port", guest: 3000, host:8000
        node.vm.provider "virtualbox" do |vb|
            vb.memory = "512"
        end
        node.vm.network "private_network", ip: "10.8.0.21"
        # try provisioning with ansible instead of shell script
        node.vm.provision "shell", path: "privisioners/install-node.sh"
    end

    # creates 3 ubuntu VMs
    (5..7).each do |i|
        config.vm.define "ubuntu#{i}" do |ubuntu|
            ubuntu.vm.box = "ubuntu/bionic64"
            ubuntu.vm.network "private_network", ip: "10.8.0.#{i}"
        end
    end

    config.vm.provider "virtualbox" do |v|
        v.memory = 8192
        v.cpus = 4
    end
end
```

---

## Containerization

### Docker
`docker exec`
`docker events`

### Docker Compose

### Binding the Docker Socket
Running docker commands from inside a container

- Linux: `-v /var/run/docker.sock:/var/run/docker.sock`
- Windows: `-v //var/run/docker.sock:/var/run/docker.sock`

---

## Container Orchestration

## Kubernetes
[AKS Concepts](https://docs.microsoft.com/en-us/azure/aks/concepts-clusters-workloads)

### Core Concepts
- **Nodes**: a single machine, cluster resource to store data, run jobs, maintain workloads and create network routes
- **Node Pools**:
- **Control Plane**: set of containers (can be multiple master nodes) that manages the cluster: contains controller manager, API server, etcd, scheduler, CoreDNS, etc.
- **kubelet**: kubernetes agent running on worker nodes
- **kube-proxy**:
- **Container runtime**:
- **Kubetctl**: CLI to configure kubernetes and deploy & manage applications
- **Pods**: can be comprised of multiple containers deployed on a single node, receives 1 IP
- **Namespaces**:
- **Labels & Annotations**: allows to filter on related resources in the cluster
- **Service**: network endpoint to connect to a pod, 4 types:
    - ClusterIP: only available within the cluster
    - NodePort: high port allocated on each node to be reachable outside of the cluster
    - LoadBalancer: controls an external load balancer endpoint external to the cluster
    - ExternalName: adds CNAME DNS record to CoreDNS to allow pods to use a specified DNS name to reach a service outside of the cluster
- **Service Discovery**: custom DNS server that all Pods use to resolve names of other services, IPs and ports
- **ReplicaSets**: used to scale pods to a desired state and hold the current status for the system
- **DaemonSets**: used to deploy a single instance of an application on each node (e.g. log/metric exporter)
- **StatefulSets**: used to deploy applications that require the use of the same node and data persistence
- **Jobs/CronJobs**: used to deploy a container to complete a specific workload and be destory on successful completion
- **ConfigMaps & Secrets**: key-value environment variables to be passed to running workloads to determine different runtime behaviors, secrets are the envrypted version
- **Deployments**: a set of metadata to describe the requirements of a running workload
- **Scheduler**: ensures that if pods or nodes encounter problems, additional pods are ran on healthy nodes
- **Storage**: an abstraction layer to manage data persistency to outlast the lifetime of a pod
- **CRDs**: custom resource definitions

### Minikube
- Install minikube
- Install kubectl

###### Create clutser
- `minikube start` (defaults to 2GB RAM, 2 CPUs, 20GB disk space)
- Bigger applications: `minikube start --memory=8192 --cpu=4 --disk-size=50g`
- Get the cluster IP: `minikube ip`

###### Profiles
`minikube start -p <profile> --memory=8192 --cpu=4 --disk-size=50g`

###### Invoke a kubernetes service
`minikube service <service_name>`


### Kubectl
- [Cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- `kubectl version`
- `kubectl apply -f filename.yml`:
- Watch pods: `kubectl get pods -w`
- `kubectl get pods --all-namespaces`
- `kubectl get service`
- `kubectl get nodes`
- `kubectl get all`
- Docs: `kubectl explain services --recursive`
    - Filter: `kubectl explain services.spec`

#### YAML Configuration
- Requirements:
    - `kind`
    - `apiVersion`
    - `metadata`
    - `spec`

###### Example of a Service & Deployment defined in the same file
```
kind: Service
apiVersion: v1
metadata:
    name: app-nginx-service
spec:
    type: NodePort
    ports:
    - port: 80
    selector:
        app: app-nginx
---
kind: Deployment
apiVersion: apps/v1
metadata:
    name: app-nginx-deployment
spec:
    replicas: 3
    selector:
        matchLabels:
            app: app-nginx
    template:
        metadata:
            labels:
                app: app-nginx
                mycustomlabel: "true"
        spec:
            containers:
            - name: nginx
              image: nginx:stable
```

###### Testing
- Manual deploy: `docker run` equivalent `kubectl run my-app --image app:latest`
- Cleanup/`docker rm` equivalent: `kubectl delete deployment my-app`
- Manual scale: `kubectl scale deploy/my-app --replicas 2` (`kubectl scale deployment my-app` also works)
- Manually create service: `kubectl expose`
- Logs of single pod: `kubectl logs deploy/my-app --folow --tail 1`
- Logs of up to 5 pods: `kubectl logs deploy/my-app -l run=my-app `
- Inspect a specific pod: `kubectl descibe pod/my-app-74dh8h72d7-2j6kv`
- `kubectl create deployment sample --image my-app:latest --dry-run -o yaml`


### Helm



### Logging
`kubectl -n <namespace> logs -f deployment/<app-name> --all-containers=true --since=10m`

###### Stern
`stern -n <namespace> <app-name> -t --since 10m`

###### Kail
`kail -n <namespace> -d <deployment> --label size=large`
`kail -n <namespace> --svc <service> --ignore size=large --since=2h`