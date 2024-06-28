# Docker External Networking

Within the host, you should be able to access docker image using the IP Address, however other host will not be able to access the image.

​

**Port Forwarding**

`$ docker run -d -p8888:80 --rm --name dtnginx nginx`

-p8888:80 will map the host port 8888 to port 80 of nginx

`192.168.18.10$ curl localhost:8888`

`192.168.18.11$ curl 192.168.18.10:8888`

​

**macvlan Driver**

​

Docker host and its images will not be able to contact each other, but other host within the macvlan network will be able to communicate with the image

​

`$ docker network create -d macvlan --subnet=192.168.18.0/24 --gateway=192.168.18.254 -o parent=ens192 ftlan`

`$ docker run -d --rm --network ftlan --ip 192.168.18.15 --name dtnginx nginx`

**--ip 192.168.18.15** will assign the ip address of 192.168.18.15 to dtnginx

​

You can try to access nginx image from other workstation within the same network as ftlan

`192.168.18.11$ curl 192.168.18.15`
