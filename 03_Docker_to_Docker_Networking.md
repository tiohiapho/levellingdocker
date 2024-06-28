# Docker to Docker Networking

**Default Bridge**

​

`$ docker container run --rm -d --name dtnginx nginx`

`$ docker inspect dtnginx | grep IPAddress`

The command inspect will give you details of the running image. The IP Address of the container can be found in IPAddress value

`$ docker container run --rm -it --name dtclient busybox sh`

This will start busybox container and run sh shell

`dtclient# wget -O - 172.17.0.2`

We can access the dtnginx via the ip address

`$ docker stop dtnginx`

​

**Custom Bridge**

​

Custom bridge allow docker image to communicate with other docker image using image name

Create the custom bridge

`$ docker network create dtnet`

`$ docker network create ftnet`

`$ docker network inspect dtnet`

`$ docker network inspect ftnet`

​

This will give you the details on the network created

`$ docker container run --rm --network dtnet -d --name dtnginx nginx`

`$ docker container run --rm --network ftnet --name dtclient -it busybox sh`

`$ docker inspect dtnginx | grep NetworkMode`

`$ docker inspect dtclient | grep NetworkMode`

​

You will see that dtnginx and the busybox is connected to different network. NetworkMode is the network the container connected to upon creation. This value might be outdated when the container is disconnected to another network.

​

`$ docker inspect dtnginx | grep IPAddress`

`dtclient# wget -O - 172.17.0.2`

You will notice that you will not be able to connect to dtnginx from busybox when using ip address since they are connected to different bridge

​

We can manually connect dtclient to dtnet using the following command:

`$ docker network connect dtnet dtclient`

`dtclient# wget -O - dtnginx`

Because custom bridge use embedded DNS, busybox container is able to connect to dtnginx using the container name

​

`$ docker network disconnect ftnet dtclient`

This will disconnect busy box from ftnet

​

We will cleanup our docker host by exiting dtclient and stopping dtnginx

`dtclient# exit`

`$ docker stop dtnginx`
