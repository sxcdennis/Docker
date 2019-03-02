# Services

A service is containers in production. A service can only run one image but multiple containers. To run multiple containers, we're going to need **docker-compose**.

As stated earlier, **Docker-Compose** is used to manage multiple container applications.

In this example, we'll scale our application and enable load balancing.


### Install Docker Compose

```
curl -L https://github.com/docker/compose/releases/download/1.24.0-rc1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose

```

### Create docker-compose.yml file

**docker-compose.yml**

```
version: "3"
services:
  web:
    # replace 115v/helloworld1:test1 with your name and image details
    image: 115v/helloworld1:test1
    deploy:
      replicas: 5
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
      restart_policy:
        condition: on-failure
    ports:
      - "4000:80"
    networks:
      - webnet
networks:
  webnet:

```

### Breakdown

- Pulls image: `115v/helloworld1:test1`

- Runs instance of that image called web while limiting the cpu usage to `10%` across all cores and `50M` ram.

- Restarts container if one fails.

- Maps port `80` on container to `4000 `on local host.

- Instruct` web's` containers to share port `80` via a load-balanced network called `webnet`.

- Define the `webnet` network with the default settings (which is a load-balanced overlay network).

### Run new App

**1.** Before running the app we must first initialize the environment.

`docker swarm init`

![docker4](https://github.com/sxcdennis/Docker/blob/master/images/docker4.png?raw=true)

**2.** Now lets run the app and give it a name.

`docker stack deploy -c docker-compose.yml lbtest`

**3.** Now lets see if the service is running

`docker service ls`

**4.** You can list the tasks in the service by using the following:

`docker service ps lbtest_web`

### Testing the app

Now that you have it running you can  test to see if the app works.

You can do `curl -4 http://localhost:4000` several times or go to the URL in a browser and hit refresh a few times.

If the container ID changes, you know that the load-balancer works.

### Scaling the App

You can scale the app by changing the amount of `replicas` in the `docker-compose.yml` file. Then re-running the deploy command.

`docker stack deploy -c docker-compose.yml lbtest`

**Note:** Docker performs an in-place update, so no need to tear the stack down first or kill any containers.

Now, re-run `docker container ls -q` to see the deployed instances reconfigured. If you scaled up the replicas, more tasks, and hence, more containers, are started.

### Take down the app and the swarm

To take down the app:

`docker stack rm lbtest `

To take down the swarm:

`docker swarm leave --force `

That's pretty much it for services. If you want to know more about compose-files: [Click Here](https://docs.docker.com/compose/compose-file/ "Here")


[< Back: Docker Concepts and Containers](https://github.com/sxcdennis/Docker/blob/master/basic.md) || [Next: Swarms >](https://github.com/sxcdennis/Docker/blob/master/swarms.md)
