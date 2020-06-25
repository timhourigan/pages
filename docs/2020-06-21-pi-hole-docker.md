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
# (Optional) All Docker to be run without root
# Note 1 - Can be used to gain root
# Note 2 - May require a new session to be effective ($ groups)
$ sudo usermod -aG docker $USER
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
# Disable the DNS Stub resolver
$ sudo sed -r -i.orig 's/#?DNSStubListener=yes/DNSStubListener=no/g' /etc/systemd/resolved.conf
# Remove the existing resolv.conf symbolic link, which is pointing to /run/systemd/resolve/stub-resolv.conf
$ sudo rm /etc/resolv.conf
# Create a new resolv.conf symbolic link, pointing to standard resolv.conf
$ ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf
# Restart the service
$ systemctl restart systemd-resolved
# (Optional) Verify the service is running
$ sudo systemctl restart systemd-resolved
```

## Setup /etc/environment

* Setup the `/etc/environment` files with environment information to be used by the Docker Compose file.

```shell
# Add Timezone - See https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
# Note - May require a new session to be effective ($ env | grep TZ)
echo 'TZ="Antarctica/South_Pole"' | sudo tee -a /etc/environment
```

* FIXME - Logout and in again?

## Docker Compose

* Create a Pi-Hole Docker Compose file

```yaml
version: "3"

services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp"
      - "80:80/tcp"
      - "443:443/tcp"
    environment:
      TZ: ${TZ}
      DNS1: 1.1.1.1
      DNS2: 1.0.0.1
      # WEBPASSWORD: 'set a secure password here or it will be random'
    volumes:
      - './etc-pihole/:/etc/pihole/'
      - './etc-dnsmasq.d/:/etc/dnsmasq.d/'
    dns:
      - 127.0.0.1
      - 1.1.1.1
    # Only required for DHCP, removing
    # cap_add:
    #  - NET_ADMIN
    restart: unless-stopped
```

## Run Pi-Hole

* Run Pi-Hole for the first time.

```shell
$ docker-compose --file docker-compose-pihole.yml up
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
https://evanshortiss.com/raspberry-pi/2019/11/13/pi-hole-setup.html
https://www.smarthomebeginner.com/run-pihole-in-docker-on-ubuntu-with-reverse-proxy/
