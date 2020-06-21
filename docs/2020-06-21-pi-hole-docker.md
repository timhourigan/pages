# Pi-Hole in Docker on 64 bit Ubuntu 20.04

How to setup Pi-Hole on 64 bit Ubuntu 20.04.

## Setup Docker

```shell
# Update package info
$ sudo apt update
# Install packages to allow apt use HTTPS based repositories, some may be installed already
$ sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
# Add Docker GPG key
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
# (Optional) Verify key
$ sudo apt-key fingerprint 0EBFCD88
# Add Docker stable repository
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
# Update package info and install Docker
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
# Test Docker
$ sudo docker run hello-world
...
> Hello from Docker!
> This message shows that your installation appears to be working correctly.
...
```

## Related Information

FIXME

## Sources
https://docs.docker.com/engine/install/ubuntu/
