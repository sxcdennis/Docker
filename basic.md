# Docker Concepts and Containers

Docker has 4 main products. **Docker EE**(Enterprise Edition), **Docker CE** (Community edition),  **Docker-Compose**, and **Docker Machine**.

**Docker CE** (Community edition) is the Open Source Community edition of previously named Docker Engine. Which is now just called Docker.

**Docker EE**(Enterprise Edition) is Docker CE with certifications on some systems and support by Docker Inc.

**Docker-Compose** is used to manage multiple container applications.

**Docker Machine** Docker Machine is a tool for provisioning and managing your Dockerized hosts (hosts with Docker Engine on them).

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

Now we're going to go through the process of building an app. We will first start by going over how to run.

To run a **container** we must first build an **image**.
To build an **image** we must first create an **environment**.
To create an **environment** we must create a file called **Dockerfile**

**1.** First we'll create a directory.

`mkdir -p /home/docker/environments/test1`

**2.** Next we'll create a the file **Dockerfile**

**Dockerfile**

```
# Use an official Python runtime as a parent image
FROM python:3.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]

```


**3.** Create the two files needed **requirements.txt** and **app.py**

**requirements.txt**

```
Flask
Redis

```

**app.py**

```

from flask import Flask
from redis import Redis, RedisError
import os
import socket

# Connect to Redis
redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

app = Flask(__name__)

@app.route("/")
def hello():
    try:
        visits = redis.incr("counter")
    except RedisError:
        visits = "<i>cannot connect to Redis, counter disabled</i>"

    html = "<h3>Hello {name}!</h3>" \
           "<b>Hostname:</b> {hostname}<br/>" \
           "<b>Visits:</b> {visits}"
    return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)

```


Now we can build the image.

**4.** Build the image with a tag name

`docker build -t=helloworld1 .`

view the image

`docker images`

**5.** Run the **image** in a **container** with machine port **4000** and containers port **80** using -p.

`docker run -p 4000:80 helloworld1`

**6.** Now we can view the image by either using url or a web browser

`curl http://localhost:4000 `
or

![docker2](https://github.com/sxcdennis/Docker/blob/master/images/docker2.png?raw=true)

**7.** You can also run a container in the background by using the -d option (detach).

`docker run -dp 4000:80 helloworld1`

**8.** Now we can stop the container by using either container ID or name.

To view containers use `docker container ls`

 ![docker3](https://github.com/sxcdennis/Docker/blob/master/images/docker3.png?raw=true)

Our ID is **ddf1212ee202** and our name is **infallible_davinci**

To stop the container we use

```
docker container stop ddf1212ee202
or
docker container stop infallible_davinci
```

## Removing Containers and Images

To remove an **image** you must first remove the **container**
To remove a **container** you must first stop it (if it's running).

### Removing Containers

You can remove containers by ID or name. You can remove multiple containers by listing them.

`docker rm ID1 or Name1 ID2 or Name2 IDx or Namex`

**Example:**

```
docker rm ddf1212ee202
or
docker rm infallible_davinci
```

### Removing Images

To remove an image you remove them by image names using `rmi`

`docker rmi Image1 Image2 Imagex`

**Example:**

`docker rmi helloworld helloworld1 friendlyhello`

**removing all images**

`docker rmi $(docker images -aq)`

### Remove container upon exit

You can run a container then remove it upon exit.

**Run and Remove:**

`docker run --rm image_name`

**Example**

`docker run -dp 4000:80 --rm helloworld1`

This will run the image helloworld1 and remove the container once stopped.

## Sharing your image

To share your image online you must create an account on hub.docker.com
Then to log into it using shell terminal use

`docker login`

### Tag the Image

To tag an image you can use the tag option  with username

`docker tag image username/repository:tag`

**Example:**

`docker tag helloworld1 115v/helloworld1:test1`

### Publish the Image

Upload your tagged image to the repository:

`docker push username/repository:tag`

**Example:**

`docker push 115v/helloworld1:test1`

### Pull and run the image from the remote repository
To pull and run the image remotely from your repostory

`docker run -p 4000:80 username/repository:tag`

**Example:**

`docker run -p 4000:80 115v/helloworld1:test1`

Docker will see if the image is already on locally, if it isn't it'll pull the image and run it.


[< Back: Table of Contents](https://github.com/sxcdennis/Docker) || [Next: Services >](https://github.com/sxcdennis/Docker/blob/master/services.md)
