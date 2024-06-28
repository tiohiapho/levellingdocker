# Uploading to Docker Hub

In order to upload to docker hub, you will need to create docker hub account

Login to docker hub using your username (may not be the same as your email address)

`docker login --username [username]`

Tag local images with the username and push it to docker hub

```
docker tag omnginx [username]/omnginx:latest
docker tag omphpfpm [username]/omphpfpm:latest
docker push [username]/omnginx
docker push [username]/omphpfpm
```
