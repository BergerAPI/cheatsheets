Having a private registry might benefit one it many ways, as it allows to keep private images private, and is also easy to setup. This example will use `docker-compose` to deploy both a registry and `nginx`, a reverse proxy.

### Creating the Compose-File
As already announced, the `docker-compose.yml` will consist out of 2 services. Only one of them will be exposing to the public, as there is no need to expose an unsecured registry to the internet.

```yaml
version: '3'

services:
    nginx:
        image: "nginx:alpine"
        ports:
            - 5020:5020
        links:
            - registry:registry
        volumes:
            - ./auth:/etc/nginx/conf.d
            - ./nginx.conf:/etc/nginx/nginx.conf:ro

    registry:
        image: registry:2
        volumes:
            - ./data:/var/lib/registry
```

## Configuring Nginx
To configure Nginx, we just need to create a file. This is described in the ``docker-compose`` file.
One needs to replace ``your.registry.com`` with their domain.

(nginx.conf)
```nginx
events {
    worker_connections  1024;
}

http {

  upstream docker-registry {
    server registry:5000;
  }
  
  map $upstream_http_docker_distribution_api_version $docker_distribution_api_version {
    '' 'registry/2.0';
  }

  server {
    listen 5020;
    server_name your.registry.com

    client_max_body_size 0;

    # required to avoid HTTP 411: see Issue #1486 (https://github.com/moby/moby/issues/1486)
    chunked_transfer_encoding on;

    location /v2/ {
      # Do not allow connections from docker 1.5 and earlier
      if ($http_user_agent ~ "^(docker\/1\.(3|4|5(?!\.[0-9]-dev))|Go ).*$" ) {
        return 404;
      }

      auth_basic "Registry realm";
      auth_basic_user_file /etc/nginx/conf.d/nginx.htpasswd;

      add_header 'Docker-Distribution-Api-Version' $docker_distribution_api_version always;

      proxy_pass                          http://docker-registry;
      proxy_set_header  Host              $http_host;
      proxy_set_header  X-Real-IP         $remote_addr;
      proxy_set_header  X-Forwarded-For   $proxy_add_x_forwarded_for;
      proxy_set_header  X-Forwarded-Proto https;
      proxy_read_timeout                  900;
    }
  }
}
```

## Authentication
At last, one wants to have a secure registry. A Docker-Registry just uses Basic-HTTP-Authentication.  Creating such authentication is as easy as running one command and creating a directory.

```shell
mkdir auth
cd auth
htpasswd -Bc nginx.htpasswd username
```

This will prompt for a password, and once one has entered and verified it, the registry is fully functional and accessible.

## Running the registry
it's as simple as one command.

```shell
docker compose up
```
The registry is now listening for traffic on port ``5020``

## (Sub-)Domain
If one wants to have a domain or subdomain, one needs to configure a DNS-Entry pointing at a server, and configure the nginx instance to listen on port 80, or use another reverse-proxy like ``Nginx Proxy Manager``.

## Using the registry
First, one needs to authorize, which is done via the ``docker login`` command.

```shell
docker login your.registry.com
```

Now one can easily push to the registry.

```shell
docker pull hello-world
docker tag hello-world your.registry.com/hello-world
docker push your.registry.com/hello-world
```