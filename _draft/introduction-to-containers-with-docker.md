# Introduction

Container technology, popularized with Docker, has become indispensable for daily software development tools. Containers have proven to be game-changing in developing software. Running applications in containers and making them talk to each other in a network amazingly reduces the complexity of the process and nowadays we make use of container technology in every step of the software lifecycle. The purpose of this article is to demystify the container concept, why it became so essential in the industry, and how we can make use of it.

# What is a Container

## REAL CONTAINER EXAMPLE
<img src='https://i2.wp.com/hip2behome.com/wp-content/uploads/sites/2/2019/02/before-and-after-junk-drawer.jpg?resize=1024%2C395&strip=all&ssl=1' />

In software world, a container is a standardized package of a software. Applications are commonly assembled from many components and a program may require one or more other programs or system services to be available. Also some files may be needed to be present in the runtime environment for the software to operate. Computers need to be configured to include these `dependencies` for every application that it is going to run. Considering that a computer can run many applications and each application has a unique set of dependency, it is no suprise that configuring and maintaining these runtimes is a hassle. 

<img src='https://s3.eu-central-1.amazonaws.com/tutorial.assets/containers-101/container-package.png' />

This is where containers comes into play by abstracting applications from the environment in which they run. Application packaged with its runtime enviroment into the container. The container provides the simple operating system with services, libraries and settings required to run the application. Now it is enough to configure the computer to run containers instead of being configured to support multiple runtimes. Therefore a container is a standard unit to distribute application from one computing environment to another quickly and reliably. Container-based applications can deployed consistently to any environment with container support, regardless of whether the target environment is a private data center, the public cloud, or even a developer’s personal laptop. Containerization provides a clean separation of concerns, where developers can focus on building the application whereas while IT operations teams can focus on deployment and management of the containers without bothering with application details such as specific software versions and configurations specific to the app. 

# Container vs VM 

A virtual machine (VM) is an emulation of a computer system. VMs make multiple virtual computers  to run inside a single computer possible. A hypervisor virtualizes the hardware required to run VMs. Guest operating system is copied to Virtual Machine such that VM can run on the virtualised hardware. Guest operating systems and its applications share hardware resources from a single host server, or from a pool of host servers.

<img src='https://www.backblaze.com/blog/wp-content/uploads/2018/06/vms.png'/>

In contrast with VMs, containers visualize the OS instead of virtualizing the underlying hardware. Containers sit on top of the physical server's OS and each container share the host OS kernel. Sharing these resources such as libraries and binaries significantly reduces the need to reproduce the operating system code, and a server can run multiple these items with a single operating system installation.

<img src='https://www.backblaze.com/blog/wp-content/uploads/2018/06/containers.png'/>

 Containers are lightweight in size and take just seconds to start. Compared to containers, VMs take minutes to run and are an order of magnitude larger than an equivalent container. It is possible to run multiple containers in VM (this is actually how the world works). However, the exact opposite is not possible (an unnecessary) to run a VM in a container.

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

Docker introduced creating docker images from a simple, easy-to-create file called `Dockerfile`. Creating and consuming a container become a matter of running some simple commands and developers started adopting. Emerged as the first major opensource project, Docker immediately became the standard of the industry. Docker images become like US Dollar in world markets. Docker desktop start to appear in most developers' laptops as well as major platforms provide support for docker containers. 

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