# Using Docker Compose

Create docker-compose.yml containing the following:

	services:
	dtnginx:
	container-name: dtnginx
	image: nginx
	ports:
		- "8888:80"
	volumes:
		- "./data:/usr/share/nginx/html"

`$ docker compose up -d`

The docker compose will start nginx as dtnginx and map port 8888 to 80. It will also use mount ./data to /usr/share/nginx/html.

Content of ./data/index.html should be served by dtnginx
