# minio-swarm

Deploys Minio to Docker Swarm

## Requirements

You will need:

* A working Docker Swarm cluster
* An even number of nodes (4 ideally)
* A reverse proxy is optional but highly recommended. (eg: [jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy))

Note that this *will only* configure Minio, it will not set up S3FS or the S3FS Docker Volume Driver. Those are up to you.

``` yaml
# these are SAMPLE values and should be changed for production usage.
minio_access_key: AKIAIOSFODNN7EXAMPLE
minio_secret_key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

# if using a custom backend for docker secrets, set it here.
minio_secret_driver: ""

# this must be set to a list of docker swarm node names
# if left empty, groups.all will be used instead.
minio_swarm_nodes: []

# local storage on nodes themselves is the only supported method currently.
minio_storage_path: /opt/minio

# docker image info
minio_image: minio/minio
minio_image_tag: latest

# in case you need to add env vars to the container
minio_extra_env_vars: {}

# use this if you need to connect minio to other clusters in the swarm, eg: for a reverse proxy
minio_extra_networks: []

# if you're planning to run multiple stacks, be sure to change this
minio_stack_name: minio
```

## Supported Platforms

* RHEL / CentOS: 6, 7      
* Fedora: 24-28      
* Debian:
    - jesse
    - sid
    - stretch
    - buster      
* Ubuntu
    - bionic
    - artful
    - zesty
    - yakkety
    - xenial
* Alpine

## Usage

``` bash
$ ansible-galaxy install inhumantsar.minio_swarm
$ echo """
minio_access_key: somelongkey
minio_secret_key: someotherlongkey
minio_swarm_nodes: [swarm_manager, worker1, worker2, worker3]
""" > vars.yml
$ echo """
- hosts: all
  roles: 
    - inhumantsar.minio_swarm
""" > playbook.yml
$ ansible-playbook -i swarm_manager, -e @vars.yml playbook.yml
```

## License
[BSD](LICENSE)

## Authors
[Shaun Martin](https://github.com/inhumantsar)