# Customize Docker Image

You can update existing docker image using Dockerfile and docker build command.
​
Create data/www.conf for php-fpm configuration

	user = www group = www
	listen = /run/php-fpm/php.sock
	listen.owner = www listen.group = www listen.mode = 0660
	pm = dynamic
	pm.max_children = 5
	pm.start_servers = 2
	pm.min_spare_servers = 1
	pm.max_spare_servers = 3

Create data/default.conf for nginx configuration

	server {
		listen 80 default_server;
		listen [::]:80 default_server;
		server_name _;
		root /srv/www/html;
		include php.conf;
	}

Create Dockerfile with the following content:

	ROM openmandriva/cooker
	RUN dnf install -y nginx php php-fpm
	COPY ./data/default.conf /etc/nginx/sites-enabled/default.conf
	COPY ./data/www.conf /etc/php-fpm.d/www.conf
	CMD php-fpm -D && nginx -g 'daemon off;

Run the following command to build the image:

`$ docker build -t omnginx .`

Create data/index.php with the following content

	<?php
		phpinfo();
	?>

Start and test the docker

`$ docker run -d --rm -p8888:80 -v./data:/srv/www/html --name dtnginx omnginx`

`$ curl localhost:8888/index.php`

Clean up the docker environment

`$ docker stop dtnginx`
