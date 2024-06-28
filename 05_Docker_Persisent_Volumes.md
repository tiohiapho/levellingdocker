# Docker Persistent Volume

Docker is able to use persistent volume to replace the image content

`$ mkdir -p data/dtnginx`

`$ vi data/dtnginx/index.html`

Add the following content to index.html

	<!doctype html>
	<html lang="en">
		<head>
			<meta charset="utf-8">
			<title>Docker DT Nginx</title>
		</head>
		<body>
			<h2>Hello from DT Nginx container</h2>
		</body>
	</html>

`$ docker run -d --rm -p 8888:80 -v ~/data/dtnginx:/usr/share/nginx/html --name dtnginx nginx`

`$ curl localhost:8888`
