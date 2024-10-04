# RKE2
## _Introduction_

Rancher next generation Kubernetes that focuses on security and compliance within the **federal government sector**. 

- Provides default and configuration options that all clusters to pass CIS Benchmarks [v1.6 or v1.23]
- Enables FIPS 140-2 compliance
- Regularly scans components with **Trivy** for CVEs in pipeline

## RKE vs K3s
- **RKE2** combines 1.x version of RKE and K3s
- Inherits _usability_, _deployment models_, and _ease-of-operations_ from **K3s**
- Inherhits close alignment with upstream kubernetes from **RKE1**.
- **RKE2** does _not_ rely on Docker like RKE does. RKE used Docker to help with deploying and managing components along with container runtime for Kubernetes. 
- **RKE2** launches components as static pods, managed by the kublet; the container runtime is containered. 

> There are two names since **RKE2** is primarily trying to convey that is used for Government targets. It also relys on this name as it is the next interation of the **Rancher Kubernetes Engine** for datacenter use. **This distro runs as standalone.**
* **

## KUBERNETES

## _Concepts && Overview_

Kubernetes is a portable, extensible, open source platform used for managing containerized workloads and services that require both declarative configuration and automation. **K8** is a common abbreviation.

**Types of Deployments: Cons and Pros**

| Traditional | Virtualized  | Containerized | 
| ------ | ------ | ------ |
| Physical servers| Multiple VMs on single SVR| Lightweight |
| Resource allocation issues | Better utilization of resources |  High efficiency and density resource utilization
| Issues with scaling | Better scalability |  Predictable application performance
| Expensive| Less expensive; reduce HDW | Liberated micro-services; can be managed dynamically and not on a monolithic stack

> Containers are a good way to bundle applications and manage containers to ensure there is no downtime. **K8** provides a framework that runs distributed systems resiliently; it ensures scaling and failover for the application.
* **
## **Architecture Overview**
![N|Solid](https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg)

A K8 suite consists of two main components - the _control plane_ and the _data plane_. 

* **Control Plane**: hosts the components used to manage the K8 cluster.
* **Worker Nodes**: Can be VMs or physical machines; a node host pods, which run one or more containers. 

When you deploy Kubernetes, you will get a **cluster**. The **cluster** will consist of worker machines, called **_nodes_** that run containerized applications. Worker nodes host **_pods_** that are components of the application. The _**Control Plane**_ manages the worker nodes and the pods in the cluster. 

>_The control plane can run across multiple computers and a cluster usually runs multiple nodes._

**Control Plane Components**



These components can make global decisions about the cluster and can detect and respond to cluster events. They can be ran on any machine in the cluster that doesn't have any user containers running. The control plane maintains communication with worker nodes.

The Kubernetes control plane is a collection of processes that manages the state of a Kubernetes cluster. The control plane interacts with individual cluster nodes using the kubelet, an agent deployed on each node.

**Kube-apiserver**

The API server is a component on the **_Control Plane_** that exposes the K8 API . It is the front end of the K8 control plane. 

It is designed to scale horizontally by deploying more instances; You can easily run several instances of kube-apiserver.

**Etcd**

etcd is designed to _reliably store infrequently updated data_ and provide reliable watch queries. It exposes previous versions of key-value pairs to support inexpensive snapshots and watch history events (“time travel queries”).

etcd stores data in a multiversion persistent key-value store.

**Kublet**

It is the primary **"node agent"** that runs on each node and registers it (the node) through the **apiserver** using:
* the hostname
* a flag to override the hostname
* specific logic for a cloud provider
 
 The kubelet works in terms of a **PodSpec** ( _a YAML or JSON object that describes a pod._) The kubelet takes a set of PodSpecs that are provided through various mechanisms (_primarily through the apiserver_) and ensures that the containers described in those PodSpecs are running and healthy. **The kubelet doesn't manage containers which were not created by Kubernetes.**

Other than from a **PodSpec** from the apiserver, there are two ways that a container manifest can be provided to the kubelet.

* **File:** Path passed as a flag on the command line. Files under this path will be monitored periodically for updates. _The monitoring period is 20s by default and is configurable via a flag._
* **HTTP endpoint:** HTTP endpoint passed as a parameter on the command line. This endpoint is checked every 20 seconds (also configurable with a flag).

```sh
kubelet [flags]
```

# 





















## Quick start
**Prereqs:** If possible, ensure that if the host kernel supports AppArmor `apparmor-parser package` it should be present prior to installing RKE.





Install the dependencies and devDependencies and start the server.
