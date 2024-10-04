## Docker

--

Docker Compose is just a client that lets you define the containers in a YAML file. 

Containers are not running “in Docker Compose” and the “platform” options still exists in the yaml but it was never for running Linux containers on Windows or Windows containers on Linux. It is only for running (for example) amd64 binaries on an arm64 CPU or arm64 binaries on amd64 CPU. ON Linux you can only run Linux containers and on Windows you can only run Windows containers. Docker Desktop creates a virtual machine. This is why you can run Linux containers inside that VM, but you can’t use it for running Windows containers and Linux containers at the same time. Since you will always need to run a virtual machine for Linux containers on Windows an Windows containers are almost always using the “hyperv” isolation not the “process” isolation you will most likely run virtual machines, You could never run Windows containers and Linux containers “on the same node” in the sense that you could run Linux containers on the same node.

If you need Linux containers and Windows containers you can use Docker Desktop to run Windows containers and WSL to run Linux containers and forward the necessary ports to the WSL host from the Windows host.

Defining these containers in the same yaml will not work since Docker can connect to only one endpoint at a time…

yum erase podman buildah

https://unix.stackexchange.com/questions/611228/getting-series-of-file-conflicts-like-runc-and-containerd-when-trying-to-install