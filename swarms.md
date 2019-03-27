# Swarms

A **swarm** is a group of machines that are running Docker and joined into a **cluster**. Once your **cluster** is formed, you can continue executing docker commands but they're now on executed by a **swarm manager**

Swarm managers are the only machines in a swarm that can execute your commands. Swarm managers can also authorize other machines to join the swarm as workers. **Workers** are just there to provide capacity and do not have the authority to tell any other machine what it can and cannot do.

```
curl -L https://github.com/docker/machine/releases/download/v0.16.1/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine
chmod +x /tmp/docker-machine
cp /tmp/docker-machine /usr/local/bin/docker-machine

```










[< Back: Services](https://github.com/sxcdennis/Docker/blob/master/services.md) || [Next: Stacks >](https://github.com/sxcdennis/Docker/blob/master/stacks.md)
