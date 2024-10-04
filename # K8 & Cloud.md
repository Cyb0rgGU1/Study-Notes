# K8 & Cloud 

**Kubernetes** is a popular open source platform for managing containerized workloads; provides a framework that enables you to build an _individual_ platform for your workload. 

## Monolithic vs Microservices: Pros and Cons

Legacy applications are usually designed with a **monolithic** approaches in mind:
- self contained
- single code base
- single binary file that can run on a server. 

> Monoliths are difficult to manage due to the complexity, scale development, implement changes fast, and fails with scaling the application out. 

**Cloud native architecture's** basic idea is to _break down applications to smaller pieces, making them more manageable._

> _Microservices_ are multiple decoupled applications that communicate with each other in a network that provide functionality. 

**Microservices** make it possible to have multiple teams, each holding ownership of different funtions of your application, but also gives the _ability to scale them out individually_. 


## Characteristics of Cloud Native Arch
- High level of automation
    - CI/CD pipelines
- Self healing
    - monitor the applicatio from the inside and automatically restart
- Scalable
    - handling more load
- Cost-Efficient
    - k8 help with more efficient and denser placing of applications
- Easy to maintain
    - using _microservices_
- Secure by default
    - requiring authentication from every user and process

### **Autoscaling**
- pattern that describes the dynamic adjustment of resources based on the current demand
-  CPU and memory are the obvious metrics to decide on 
- other methods based on time or business metrics can also be considered 

### *I. Horizontal*
- When talking about *autoscaling*, talking about _horizontal scaling_ which is the process of spawning new computer resources. 
- Can be new copies of your application process, virtual machines, new racks of servers and/or other hardware. 

### *II. _Vertical_*
- describes the change in size of the underlying hardware
- scaling up by allowing them to consume more CPU and memory
- a limit is defined by the compute and memory capacity of the underlying hardware

### *III. Example*
- Vertical scaling
    - If you pick up a heavy object, you build muscle to carry it yourself but your upper body has a limit of strength
- Horizontal scaling
    - if you call your friends to help and share the work

> The ability to scale your application can increase availability and resilence of your services in more traditional enviornments. 

## Serverless 
- Servers are still required as the basis of the application. 
    - configure several resources
        - network, vms, os, and load balancers
- serverless computing as an even stronger focus on the on-demand provisioning and scaling applications. 
- **Autoscaling is an important core concept of servless**
    - helps with precise billing
- **FaaS**
    - vendors have offerings of properetary serverless runtimes and subsets
    - provide the underlying INF so devs can deploy software
    - often used in combination or as an extension of existing platforms 
    - allow for fast deployment and excellent testing/sandbox env
    - struggles with standardization 
### Open standards
- Cloud native technologies rely heavily on open source software
- Everyone can collaborate
- Easy to implement indutry-wide standards
- _containers_ evolved as a standardized way to package and ship modern software

~**STANDARDS WHICH DEFINE THE WAY HOW TO BUILD AND RUN CONTAINERS**~
| image-spec | runtime-spec | distribution-spec |
|----| ---- | ---- |
|defines how to build and package container images| specifies the configuration, execution environment and lifecycle of containers| provides a standard for the distrbution of content in general and container images in particular|  

|OCI Spec| CNI | CRI | CSI | SMI |
| ---- | ---- | ---- | ---- | ---- |
| - | Container network interface | Container runtime interface | Container storage interface | Service Mesh Interface | 
| image, runtime and distribution specification on how to run and build a container | how to implement networking for Containers | how to implement container runtimes in orchestration systems | implement storage in container orchestration systems | implement Service Meshs in container systems **with a focus on K8**

## **Containers**
- Sysad who provide the INF, installs all depends, configures system which can very error-prone and hard to maintain 
- servers are only configured for a single purpose like running a database or an application server. 
- For more efficent use, VM can be used to emulate a full server with cpu, memory, storage, networking, OS and software. **Always comes with overhead if you have to run a lot of servers.** 
- Containers can used to solve these problems 
- managing applications and running more efficently. 

### I. _Container Basics_
- Earliest ancestor of modern container technology is `chroot`. 
- `chroot` used to isolate a process from the root filesystem and hide files from process
- isolate process, linux kernels provide features like **namespace** and **cgroups**

> Namespaces are used to isolate various resources

~**Linux Kernel 5.6 provides 8 namespaces**~
| pid | net | mnt | ipc | user | uts | cgroup | time |
| -- |-- |-- |-- |-- |-- |-- |-- |
| process ID | network | mount abstracts | inter-process communications | provides process with their own User IDs and group IDs | Unix time sharing allows processes to have their own hostname and domain name | newer namespaces that allow a process to have it's own of cgroup root directories | newest timespace can be used to virtualize the clock of a system |

> Cgroups are used to organize processes in hierarchial groups and assign them resources like memory and CPU. 
>> When you want to limit your application container to 4BG, cgroups are used under the hood to ensure these limits. 

| VM | Containers |
|-- |-- |
Emulates a complete machine, including the operating system and kernel | share the kernel of the host machine and isolate processes |
| come with overhead | literally processes - smaller footprint |

### II. _Running containers_
- You don't need to use **Docker**; you can just follow the OCI runtime-spec.
> The Open Container Initiative also maintains a container runtime refernce implementation called **runC**. 

- With Docker installed, you can start containers like this:

``` docker run nginx```

- The _runtime-spec_ goes hand in hand with the _image-spec_ as it describes how to unpack an image then manage the lifecycle. 

- There are plenty of alternatives to choose from, some which are only for building images like `buildah` or `kaniko` or `podman` which is an alternative to `Docker`. 

> Podman provides a similiar API as Docker and be used as a drop in replacement. 

### III. _Docker & Podman_

* Check software version
----
```student@ubuntu:~$: docker version```

```student@ubuntu:~$: podman version ```

- Long list of commands that can be used 
----
```student@ubuntu:~$: docker --help```

```student@ubuntu:~$: podman --help```

- Running specific images from docker hub in foreground
----
```student@ubuntu:~$: docker run nginx:1.20```
- Run in background and open a port on the machine
----

```student@ubuntu:~$: docker run --detach --publish-all -nginx:1.20```

```student@ubuntu:~$: podman run --detach --publish-all -nginx:1.20```
- Check the containers running on your machine
----

```student@ubuntu:~$: docker ps ```

```student@ubuntu:~$: podman ps```

- Terminate container
----
```student@ubuntu:~$: docker stop <container-id>```

```student@ubuntu:~$: podman stop <container-id>```

- Run logs
----

```student@ubuntu:~$: docker logs -f <container-id>``` // `-f` following output

```student@ubuntu:~$: docker logs --since=1h <container-id>``` // since last 1 hour

```student@ubuntu:~$: docker logs --since=1h <container-id> > /path/to/save.txt```

~

```student@ubuntu:~$: podman logs -f <container-id>```

```student@ubuntu:~$: podman logs --since 10m <container-id>``` // last 10 minutes

```student@ubuntu:~$: podman logs --since 0 <container-id>``` // view all container logs

- Delete containers 
```student@ubuntu:~$: podman logs --since 0 <container-id>```

### IV. _Building Container Images_

- Docker reused all components to isolate process like _namespaces_ and _cgroups_ but what helped with their breakthrough was **container images**. 
- Container images are what make containers portable and easy to reuse. 

> A Docker container image is lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings. 

### V. _Container Images_ 
- Images can be built by reading instructions from a buildfile called **Dockerfile**. 

```
# Every container image starts with a base image.
# This could be your favorite linux distribution
FROM ubuntu:20.04 

# Run commands to add software and libraries to your image
# Here we install python3 and the pip package manager
RUN apt-get update && \
    apt-get -y install python3 python3-pip 

# The copy command can be used to copy your code to the image
# Here we copy a script called "my-app.py" to the containers filesystem
COPY my-app.py /app/ 

# Defines the workdir in which the application runs
# From this point on everything will be executed in /app
WORKDIR /app

# The process that should be started when the container runs
# In this case we start our python app "my-app.py"
CMD ["python3","my-app.py"]
```

- Once you have installed Docker on your machine and built up your **Dockerfile** you can build up the image with:

```
student@ubuntu:~$: docker build -t my-python-image -f Dockerfile
```
-- with the paramter `-t my-python-image` you can specify a name tag for your image and with `-f Dockerfile` you can specify where your Dockerfile can be found. 

- To distribute the images:
```
student@ubuntu:~$: docker push my-registry.com/my-python-image
student@ubuntu:~$: docker pull my-registry.com/my-python-image
```

### VII. _Example building image_

```
student@ubuntu:~$: docker images \\see images on box
student@ubuntu:~$: cd /app 
student@ubuntu:~$: vi Dockerfile

FROM ubuntu:20.04 
RUN apt-get update && \
    apt-get -y install python3 python3-pip 
COPY my-app.py /app/ 
WORKDIR /app
CMD ["python3","my-app.py"]

student@ubuntu:~$: docker build -t <container-tag-name>
```
### VIII. _Security_
- Containers have different security requirements than virtual machines. 
- Containers always share the same kernel, which becomes a risk for the whole system
- One of the greatest risks is the execution of processes with too may privlieges
    - this is a problem that got ignored a lot in the past
    - there are a lot of containers out running as root users
- A new attack surface that was introduced was the use of public images.
    - Docker hub and Quay
    - **Have to ensure the images were not modified to include MALICIOUS software**. 
- Security cannot only be achieved at container layer
    - **it's a continious process**

~  **4C's of Cloud Native Security**
|Cloud/Corporate Datacenter | > Cluster|  > Container| > Code|
| --- | --- | --- | ---- |
| Computers and Networks | K8 | Docker | Code |

![Security](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/8c2fwjtsjomf-4CsofCloudNativesecurity.png)

### IX. _Container Orchestration Fundamentals_
- Easy to run a few containers on a local machine or single server
- Modern applications can consist of a lot of containers
- If you have to manage and deploy a large amount of containers
    - problems that can solved 
        - providing compute resources
        - schedule containers to servers
        - allocate resources
        - manage availability and replace them if they fail 
        - scale containers
        - provide networking to connect containers together
        - provision storage if containers need it 
- Orchestration systems provide a way to build a cluster of multiple servers and host the containers on top. 
- Most container ORCH systems have two parts
    - **Control Plane**
        - responsible for the management of contaienrs
    - **Worker Nodes**
        - host the containers 
- Have chosen _Kubernetes_ has the standard system

### X. _Networking_
- Microservice architecture depends heavily on network communication. 
    - has an interface that can be called to make a request 
- Network _namespaces_ allow each container to have it's **own unique IP address**
    - multiple applications can open the same network port 
- to make the app accessible outside of the host, containers have the ability to map a port from the host

### XI. _Service Discovery & DNS_
- Traditionally memorization was how IPs and hostnames were remembered. 
- For containers, it's more complicated
    - solution is automation
    - all the information is put into a _Serivce Registry_. 
    - Finding other services in the network and requesting information is called _Service Discovery_.

| DNS | Key-Value-Store |
| --- | --- | 
| Modern DNS servers have a **service API** that can be used to register new services as they are created. | Using strongly consistent datastores to store information about services. Strong failover mechanism. | 
> Popular choices, especially for clustering are `etcd`, `Consul`, or `Apache Zookeeper`.

### XII. _Service Mesh_

> Networking is such a crucial part of microservices and containers

- Able to start a second container with software that can manage traffic called a **PROXY**
    - server application that shits between a client and server and can modify or filter network traffic before reaching the server
        - nginx, haproxy, or envoy
- Can use proxies to handle network communication between services
- When a service mesh is used, applications don't talk to each other directly, but traffic is routed through the proxy instead.
    - popular service meshes are:
        - istio and linkerd
- proxies in a service mesh form the _data plane_. 
    - networking rules are implemented and shape the traffic flow
- rules are managed centrally in the control plane of the service mesh 
> instead of writing code and installing libraries, you write a config file where you tell the service mesh that service A and B should always be able to communicate encyprted 
>> The configs are then uploaded to the control plane and distributed to the data plane to enforce the new rule. 

### XIII. _Storage_

Containers have a significant flaw: **They are ephemeral**. 

- Container images are _read-only_ and consist of different layers of things you added during the buliding phase. 
    - ensures every time you start a container from an image, it gives you the same behavior and functionality. 
    - to ensure writing to files, a read-write layer is put on top of the container image

- if a container needs persistant data on a host, you can use a **volume** to achieve that. 
    - instead of isolating the whole filesystem of a process, it resides on the host 
        - **THIS WEAKENS THE ISOLATION OF THE CONTAINER AS IT GIVES ACCESS TO THE HOST FILESYSTEM**. 

_DATA IS SHARED BETWEEN TWO CONTAINERS ON THE SAME HOST_

# K8 FUNDAMENTALS

- K8 is often used in clusters used to work on different tasks and to distribute the load of a system. 
- consists of two different server note types that make up a cluster

|Control Plane| Worker nodes| 
|----|----|
| Brains of the operation| Where applications run in your cluster |
| container various components to manage the cluster and control different tasks| only job of the worker nodes is to run the application |
| deployment, scheduling, and self healing| completely controlled by the control plane node |

![Security](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/lrc7lcf1ayk1-Kubernetesarchitecture.png)

