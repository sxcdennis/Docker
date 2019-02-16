# Docker Concepts and Containers

Docker has 3 main products. **Docker EE**(Enterprise Edition), **Docker CE** (Community edition) and **Docker-Compose**.

**Docker CE** (Community edition) is the Open Source Community edition of previously named Docker Engine. Which is now just called Docker.

**Docker EE**(Enterprise Edition) is Docker CE with certifications on some systems and support by Docker Inc.

**Docker-Compose** is used to manage multiple container applications.


Docker is a tool that can deploy and run applications by using **containers**.

A container is launched by running an image. An **image** is an executable package that contains everything to run an application (code, runtime, libraries, configuration files, environment variables).

A **container** is a instance of an image. To view all the running containers you can use the command `docker ps`.


# Containers and Virtual Machines

A container runs on the Linux kernel and shares it with the host machine.

A virtual machine runs a full blown guest operating system.

![docker01](https://github.com/sxcdennis/Docker/blob/master/images/docker01.png?raw=true)

## When to use docker and when to use VM

Although there are several benefits of using docker because its way more light-weight than using a Virtual Machine, they are used for totally different things. The following are just some basic examples of why you may want to user docker or a virtual machine.

### When to use Docker

- Docker is great for test environments. You can skip the time for installing and configuring and easily dispose environments.


- You can also quickly pull images from Docker Hub if you wanted to get a solution quickly for you environment.

- You can run multiple applications on one server, keeping components of each application in separate containers will prevent problems with dependency management.

### When to use Virtual Machines

- You have a complicated application, and you will be needing to continuously edit and build configurations.

- Your application needs to be performed at full potential.

- You want to run multiple operating systems.

# Installation

`yum install -y docker`
`systemctl start docker`

## Basic Commands

`docker --version` checks version of Docker

`docker info` more detailed information of Docker

`docker run hello-world` Tests docker Installation by downloading hello-world and using it.

`docker image ls` Lists images on machine

`docker container ls --all` Lists all containers.

# Containers

Now we're going to go through the process of building an app. We will first start by going over how to run

















[< Back: Table of Contents](https://github.com/sxcdennis/Docker) || [Next: Services >](https://github.com/sxcdennis/Docker/blob/master/services.md)
