## Consul Agent Example

Oficial documentation [here](https://www.consul.io/docs/agent)

### About
This small projects will:
    - Uploading the agent
    - Seeing the members of the cluster
    - DNS searching
    - Consul API access

### Useful commands
> Run all these commands inside the container `docker exec -it consul* sh`

Raising the consul on dev mode: `consul agent -dev`
Showing members (to know everyone in the cluster): `consul members` 
Checking the catalog API: `curl localhost:8500/v1/catalog/nodes`
Installig 'dig' on Linux Alphine: `apk -U add bind-tools`
Searching on DNS: `dig @localhost -p 8600 consul01.node.consul`; `dig @localhost -p 8600 nginx.service.consul`
Update the services: `consul reload`
Listing all nodes from catalog: `consul catalog nodes -detailed`
Searching for services with DNS (inside the client with the service): `dig @localhost -p 8600 web.nginx.service.consul`
Generating encrypted key: `consul keygen`

#### Server
> Run all these commands inside the container `docker exec -it consulserver* sh`

Raising the consul on server mode: `consul agent -server -bootstrap-expect=3 -node=consulserver01 -bind=<container_ip> -data-dir=/var/lib/consul -config-dir=/etc/consul.d`
- container_ip: inside the container, run `ifconfig`

- Expecting 3 servers

- Node will be consulserver01

- data-dir: Dir where consul will host it´s files

- config-dir: Dir where consul will host it´s conf files

To make 2 consuls servers communicate with each other: `consul join <other_container_ip>` 
Raising the consul on server mode using server.json:
- `docker exec -it consulserver02 sh`

- `consul agent -config-dir=/etc/consul.d`

#### Client
> Run all these commands inside the container `docker exec -it consulclient* sh`

Raising the consul on client mode: `consul agent -client -node=consulserver01 -bind=<container_ip> -data-dir=/var/lib/consul -config-dir=/etc/consul.d`
- container_ip: inside the container, run `ifconfig`

- Node will be consulclient01

- data-dir: Dir where consul will host it´s files

- config-dir: Dir where consul will host it´s conf files

To make 2 consuls communicate with each other: `consul join <other_container_ip>`

#### Service Discovery
> Run all these commands inside the container `docker exec -it consulclient* sh`

Raising the consul on client mode and automaticaly join with a server: `consul agent -client -node=consulserver01 -bind=<container_ip> -data-dir=/var/lib/consul -config-dir=/etc/consul.d -retry-join=<some_server_ip> -retry-join=<some_other_server_ip>`
Searching for services in the cluster with DNS (inside the client with the service): `dig @localhost -p 8600 web.nginx.service.consul`