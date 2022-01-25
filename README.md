# nginx-proxy using docker compose

Docker compose file for launching nginx-proxy container

## Custom nginx configuration

To add server-wide configuration, you can put the file to `conf.d` directory, any file with `*.conf` will be picked up by nginx.

To add site-wide configuration, you can put the file to `vhost.d` directory using filename matching exactly with `VIRTUAL_HOST` variable configured in backend container.

For more information, please check this guide https://github.com/nginx-proxy/nginx-proxy#custom-nginx-configuration

## nginx-proxy network

The compose file is configured to create a bridge network with the name of `nginx-proxy`, in order to allow connectivity between `nginx-proxy` container and backend container. The backend container must be connected to `nginx-proxy` network

For example, docker-compose.yml

```yml
version: '3'

services:
 whoami:
    image: jwilder/whoami
    expose:
      - "8000"
    networks:
      - default # this allow connectivity within compose stack
      - nginx-proxy # this allow connectivity with nginx-proxy
    environment:
      - VIRTUAL_HOST=whoami.local
      - VIRTUAL_PORT=8000

# ask for nginx-proxy network
networks:
  nginx-proxy:
    external: true
```

