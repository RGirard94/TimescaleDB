ansible-traefik-in-docker
=========
:shipit: Ansible galaxy role to deploy [Traefik](https://traefik.io/) in docker

The purpose of this Ansible role is to allow you to manage a
[Traefik](https://traefik.io/) reverse proxy.

With this role you will be able to generate deploy a [Traefik](https://traefik.io/) container on you server,
configure desired HTTP/HTTPS redirections and generate HTTPS associated certificates (thanks to [Let's Encrypt](https://letsencrypt.org/)).


Requirements
------------

The following packages have to be installed and well configured on the host :
- [Docker-ce](https://docs.docker.com/engine/installation/)

Role Variables
--------------

### User defined variables
The following vars have to be defined on each execution by the user to configure the reverse proxy mappings

#### Mandatory - Endpoints list configuration
```yaml
traefik_endpoints:
  endpoint_name:
    # Fontend match on host to trigger the redirection
    host: "test.localhost"
    # Backend url to redirect to
    dest: "http://perdu.com"
    # If true generate let's encrypt certificate for specified host
    host_gen_cert: true
```

### Overridable default variables
#### Name of the traefik docker container
```yaml
traefik_container_name: "traefik"
```

#### Path on the host filesystem where will be the traefik conf file
```yaml
traefik_root_location: "/opt/docker-data/{{ traefik_container_name }}"
traefik_conf_location: "{{ traefik_root_location }}/conf"
```

#### Host exposed HTTP/HTTPS/Admin ports for traefik
```yaml
traefik_http_port: 80
traefik_https_port: 443
traefik_http_admin_port: 8080
```

#### Let's Encrypt CA Server to use. If false or unset prod CA Server used
```yaml
traefik_le_staging: true
```
[Let's encrypt staging environnement documentation](https://letsencrypt.org/docs/staging-environment/)

#### Docker networks
```yaml
traefik_docker_networks: []
#traefik_docker_networks:
#  - name: "docker_network"
```

#### Docker container restart policy
```yaml
traefik_container_restart_policy: "always" #(always, no, on-failure, unless-stopped)
```

Dependencies
------------

--

Example Playbook
----------------

See tests/test.yml  
`ansible-playbook tests/test.yml --ask-sudo-password -e traefik_root_location="$(PWD)/.workdir/"`
```yaml
- hosts: localhost
  roles:
    - role: ../ansible-traefik-in-docker
      traefik_http_port: 8090
      traefik_http_admin_port: 8080
      traefik_https_port: 4443
      traefik_le_staging: true
      traefik_endpoints:
        influxdb:
          host: "test.localhost"
          dest: "http://perdu.com"
          host_gen_cert: false
```
License
-------

MIT

Author Information
------------------

https://github.com/BastienF/ansible-traefik-in-docker
