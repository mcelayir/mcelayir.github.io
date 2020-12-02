# Introduction

Container technology, popularized with Docker, has become indispensable for daily software development tools. Containers have proven to be game-changing in developing software. Running applications in containers and making them talk to each other in a network amazingly reduces the complexity of the process and nowadays we make use of container technology in every step of the software lifecycle. The purpose of this article is to demystify the container concept, why it became so essential in the industry, and how we can make use of it.

# What is a Container

## REAL CONTAINER EXAMPLE

A container is a standardized package of a software. Generally packaged software maps to an executable file where executing this file is actually `running the program`. For example after compiling our Java code, we generally get ourselves a jar file and we can execute this jar file in every computer which has any compatible version Java runtime installed. However, software systems and products are commonly assembled from many software components. A program may require one or more other programs to run and also some files may be needed to be present in the runtime environment for the software to operate. Computers need to be configured to include these `dependencies` for every application that it is going to run. Considering that a computer can run many applications and each application has a unique set of dependency, it is no suprise that configuring and maintaining these runtimes is a hassle. 

## PUT THIS IMAGE https://youtu.be/gFozhTXOx18?t=57

This is where containers comes into play by abstracting applications from the environment in which they actually run. Software packaged with its runtime enviroment are called containers. Now computer has to be configured only to run this containers instead of being configured to support multiple runtimes. Therefore a container `is a standard unit of software that application runs quickly and reliably from one computing environment to another`. Container-based applications can consistently, regardless of whether the target environment is a private data center, the public cloud, or even a developer’s personal laptop. Containerization provides a clean separation of concerns, where developers can focus on building the application whereas while IT operations teams can focus on deployment and management of the containers without bothering with application details such as specific software versions and configurations specific to the app. 

# Container vs VM 

A virtual machine (VM) is an emulation of a computer system. VMs make running multiple virtual computers inside a single computer possible. The operating systems (OS) and their applications share hardware resources from a single host server, or from a pool of host servers. Each VM requires `its own underlying OS, and the hardware is virtualized`. A hypervisor, or a virtual machine monitor, is software, firmware, or hardware that creates and runs VMs. It sits between the hardware and the virtual machine and is necessary to virtualize the server.

<img src='https://www.backblaze.com/blog/wp-content/uploads/2018/06/vms.png'/>

In contrast with VMs, containers visualize the OS instead of virtualizing the underlying computer like a virtual machine (VM).
Containers sit on top of a physical servers OS and each container shares the host OS kernel. Sharing these resources such as libraries and binaries significantly reduces the need to reproduce the operating system code, and a server can run multiple these items with a single operating system installation.

<img src='https://www.backblaze.com/blog/wp-content/uploads/2018/06/containers.png'/>

 Containers are lightweight in size and take just seconds to start. Compared to containers, VMs take minutes to run and are an order of magnitude larger than an equivalent container. Moreover, since a VM is a virtualization of computer and has its own guest OS running on top of the host OS, it is possible to run multiple containers in VM (this is actually how the world works). However, the exact opposite is not possible (an unnecessary) to run a VM in a container.

# Why Containers

Containers solve the problem of moving software from one environment to another reliably. This environment can be developer's laptop or production server. Containers isolate the application's dependency to the platform that is running by creating the habitat needed for the application to run and stay healty. Creating an instance of the container means actually running the application so it the application can be deployed to anywhere that the container runtime is installed.

Constainers also bring agility in many aspects. Unlike VMs, contaniers are lightweight and can spin up from scratch in seconds. This means an instance of any application can be created and destroyed immediatly, therefore
- Less resources needed to run
- No more platform dependency, take your container anywhere you go
- Gain from the time spent for maintenance
- Experiment with different technologies and products
- Faster prototyping and distribution with versioned images

# Docker

Containers (sort of) existed even before docker. However creating and running them was to complicated for common folks like me. Instead it required a technical expertise in Unix processes and so on. 

Docker introduced creating docker images from a simple, easy to create file called `Dockerfile`. Creating and consuming a container become a matter of running some simple commands and developers started adopting. Emerged as the first major opensource project, Docker immediately became the standard of the industry. Docker images become like US Dollar in world markets. Docker desktop start to appear in most developers' laptops as well as major platforms provide support for docker containers. 

# Docker Engine

Docker Engine is responsible for building images from docker files, creating container instances from images, mananing the network and volumes for container runtime.
Images built by docker engine stored locally and re-used  until a newer version requested by the user. 

<img src='https://docs.docker.com/engine/images/engine-components-flow.png' />

# Images

As Docker becomes the de-facto standard in the industry, images become the standard way of creating and distributing containers. Images can be created for different container runtines. Container images are the code that can be run by the container runtime. They are immutable which means that image with version x will always be the same and work as the same in anywhere, and ephemeral which means that containers will live for short periods when needed. The container engine will create an instance from the image and keep the instance alive until it is not needed anymore.



<img src='https://s3.eu-central-1.amazonaws.com/tutorial.assets/containers-101/container-lifecycle.jpeg'/>

# Docker

docker run -p 80:80 docker/getting-started 

# Misconceptions with Docker

https://containerjournal.com/features/imagining-a-world-without-docker/



# Reading list

As a last world, I recommend reading following articles to have a better understanding of the Containers.

- [What are containers and why do you need them](https://www.cio.com/article/2924995/what-are-containers-and-why-do-you-need-them.html)
- https://www.backblaze.com/blog/vm-vs-containers/

- https://containerjournal.com/topics/container-ecosystems/5-container-alternatives-to-docker/

- https://www.docker.com/101-tutorial