# Pi-Hole in Docker on 64 bit Ubuntu 20.04

How to setup Pi-Hole on 64 bit Ubuntu 20.04.

## Setup Docker

### Install Docker

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
# (Optional) Test Docker with interactive Ubuntu
$ sudo docker run -it ubuntu bash
# (Optional) All Docker to be run without root - Note, can be used to gain root
$ sudo usermod -aG docker <username>
```

### Install Docker Compose

* Determine the latest release from https://github.com/docker/compose/releases

```shell
# Example download of Docker Compose version 1.26.0
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# Make docker-compose executable
$ sudo chmod +x /usr/local/bin/docker-compose
# (Optional) Test Docker Compose
$ docker-compose --version
> docker-compose version 1.26.0, build d4451659
````

## Modify systemd-resolved

* Ubuntu 20.04 (>17.10) includes systemd-resolved, which acts as a DNS stub resolver, preventing Pi-Hole from listening to port 53. This needs to be disabled for Pi-Hole to work.

```shell
# Remove the existing resolv.conf symbolic link, which is pointing to /run/systemd/resolve/stub-resolv.conf
$ sudo rm /etc/resolv.conf
# Create a new resolv.conf symbolic link, pointing to standard resolv.conf
$ ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf
# Restart the service
$ systemctl restart systemd-resolved
# (Optional) Verify the service is running
$ sudo systemctl restart systemd-resolved
```

## Todo
* /etc/environment
* Pi-Hole
* Reverse proxy

## Related Information

FIXME

## Resources
https://docs.docker.com/engine/install/ubuntu/
https://github.com/pi-hole/docker-pi-hole/#running-pi-hole-docker
