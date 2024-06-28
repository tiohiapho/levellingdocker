# Multiple Services with Compose

Although it is possible to run multiple service within the same docker container, in general each container should only run single process. Currently our customized docker image is running both php-fpm and nginx in the same container. We will try to split this into two docker image and run both services using docker compose.

Create data/www.conf for php-fpm

	[www]
	user = www
	group = www
	listen = 9000
	listen.owner = www
	listen.group = www
	listen.mode = 0660
	pm = dynamic
	pm.max_children = 5
	pm.start_servers = 2
	pm.min_spare_servers = 1

Notice that we change **listen** parameter to port **9000** instead of the local sock file.

Create the following Dockerfile for omphpfpm image

	FROM openmandriva/cooker
	RUN dnf install -y php-fpm
	COPY ./data/www.conf /etc/php-fpm.d/www.conf
	CMD php-fpm -F

Build omphpfpm image

`$ docker build -t omphpfpm .`

Update data/php.conf with the following content

	index index.html index.htm index.php;
	location / {
		try_files $uri $uri/ /index.php$is_args$args;
	}
	location ~ \.php$ {
	fastcgi_split_path_info ^(.+\.php)(/.+)$;
	fastcgi_pass dtphp:9000;
	fastcgi_index index.php;
	include fastcgi.conf;
	}

omphpfpm will run as dtphp, hence php.conf is updated to point **fastcgi_pass** to **dtphp:9000**

Update omnginx Dockerfile as follow:

	FROM openmandriva/cooker
	RUN dnf install -y nginx
	COPY ./data/default.conf /etc/nginx/sites-enabled/default.conf
	COPY ./data/php.conf /etc/nginx/php.conf
	CMD nginx -g 'daemon off;'

Build omnginx image

`$ docker build -t omnginx .`

Create docker-compose.yml to start both docker image

	services:
	  dtphp:
	    container_name: dtphp
	    image: omphpfpm
	    networks:
	      - phpnginx
	    volumes:
	      - "./data:/srv/www/html"
	  dtnginx:
	    container_name: dtnginx
	    image: omnginx
	    network:
	      - phpnginx
	    ports:
	      - "8888:80"
	    volumes:
	      - "./data:/srv/www/html"
	networks:
	  phpnginx:
	    name: phpnginx
	    driver: bridge

Let's test our docker compose configuration

`$ docker compose up -d`

`$ curl localhost:8888`

`$ curl localhost:8888/index.php`

docker compose will create custom bridge network using current directory name and connect both services.
