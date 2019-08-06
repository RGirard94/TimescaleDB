ansible-role-grafana-in-docker
=========
Ansible galaxy role to deploy [Grafana](https://grafana.com/) in docker


Requirements
------------

The following packages have to be installed and well configured on the host :
- [Docker-ce](https://docs.docker.com/engine/installation/)

Role Variables
--------------

### Mandatory variables (Grafana will not be connected to any database if those are not set)
The following variables should be set to specify the datasources Grafana should connect to :
```yaml
datasources: [] # datasources is a list of dictionaries containing the following keys : datasource_name, datasource_type, datasource_database, datasource_hostname and datasource_port
```
Some more explanations :
datasource_name: "" # must be unique, but whatever you like
datasource_type: "" # can be CloudWatch, Elasticsearch, Graphite, InfluxDB, MySQL, OpenTSDB, PostgreSQL or Prometheus
datasource_database: ""
datasource_hostname: "" # hostname of the server the db is running on
datasource_port: "" # database port
 

### Overridable default variables
```yaml
grafana_container_name: "grafana"
grafana_docker_networks: []
grafana_container_restart_policy: "always"
grafana_conf_location: "/opt/docker-data/{{ grafana_container_name }}/conf"
grafana_dashboards_location: "/opt/docker-data/{{ grafana_container_name }}/dashboards"
grafana_dashboards_location_on_container: "/home/dashboards" # directory where to store/load dashboards on container
```
grafana_dashboards_location variable defines the location on host where dashboard (json files) should be put. None exist by default. You would then need to add a play in tasks/main.yml to copy your dashbords to host. This could look like this : 
```yaml
- name: "Copy dashbords folder"
  become: true
  synchronize:
    src: some/relative/path/on/source/host
    dest: /some/absolute/path/on/destination/host
```

Finally, grafana admin password (in defaults/main.yml file) can be updated in secret.yml file that should be in .gitignore file
```yaml
grafana_admin_password: "admin"
```

Dependencies
------------


Example Playbook
----------------

Checkout tests/test.yml file :

	- hosts: localhost
	  roles:
	    - ../ansible-role-grafana-in-docker

Usage :

```bash
ansible-playbook tests/test.yml --ask-become-pass -e grafana_conf_location="$(pwd)/.workdir/conf" -e grafana_dashboards_location_on_host="$(pwd)/.workdir/dashboards" -e datasource_hostname="influx" -e datasource_port="1234"
```

On MacsOS due to Docker Machine root limitation, add : -e ansible_become_user="$(whoami)"


License
-------

MIT

Author Information
------------------

https://github.com/lribiere/ansible-role-grafana-in-docker.git
