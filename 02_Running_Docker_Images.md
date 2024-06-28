# Running Docker Images

`$ docker container run -d --rm --name dtnginx nginx`

This command will download nginx image and run it as dtnginx. The image will be detached (-d) and run in the background. The image will also be removed once it is stopped

​

`$ docker ps -a`

This will show that nginx container is running; note the container id and name

​

You can login into your container by running the following command:

`$ docker exec -it dtnginx /bin/bash`

​

Inside the docker you can run curl command to verify that nginx is running

`dtnginx# curl localhost`

`dtnginx# exit`

​

To stop the image, run the following command:

`$ docker stop dtnginx`

​

If the image is not run with --rm, you can clean up your docker host by removing the image manually

`$ docker rm dtnginx`
