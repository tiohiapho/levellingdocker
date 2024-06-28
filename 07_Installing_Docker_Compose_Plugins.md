# Installing Docker Compose Plugin

**Steps:**

1.  **Setting Default Docker Config:**
     
    ```
    $ DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
    
    ```
    
    -   This line sets an environment variable named `DOCKER_CONFIG`.
        -   If a variable named `DOCKER_CONFIG` already exists, its value is used.
        -   Otherwise, it defaults to `$HOME/.docker`. This directory stores Docker configuration files, including those for plugins.
        
2.  **Creating Plugin Directory:**
    
    ```
    $ mkdir -p $DOCKER_CONFIG/cli-plugins
    
    ```

    -   This line creates a directory named `cli-plugins` inside the `$DOCKER_CONFIG` directory (usually `~/.docker/cli-plugins`).
        -   The `-p` flag ensures the directory is created if it doesn't exist, and any missing parent directories are created as well.
        -   This directory will store Docker Compose Plugin
        
3.  **Downloading Docker Compose:**
    
    ```
    $ curl -SL https://github.com/docker/compose/releases/download/v2.20.0/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
    
    ```
    
    -   This line downloads the Docker Compose binary for Linux x86_64 architecture from the specified URL using `curl`.
        -   The `-SL` flags tell `curl` to follow redirects (`-L`) and fail silently (`-s`) if unsuccessful.
        -   The downloaded file is saved as `docker-compose` within the `cli-plugins` directory.
        
4.  **Making Executable:**
    

    ```
    $ chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose
    
    ```
    
    -   This line grants execute permissions (`+x`) to the downloaded `docker-compose` file.
        -   This is necessary for it to be run as a command.

5.  **Verifying Installation:**
    
    ```
    $ docker compose version
    
    ```
    
    -   This line attempts to run `docker-compose version` to verify the installation.
        -   If successful, it should display the installed Docker Compose version (v2.20.0 in this case).
