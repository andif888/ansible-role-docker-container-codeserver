# ansible-role-docker-container-codeserver

Role to run codeserver in a docker container

## Table of content

- [Requirements](#requirements)
- [Default Variables](#default-variables)
  - [docker_container_codeserver_command](#docker_container_codeserver_command)
  - [docker_container_codeserver_comparisons](#docker_container_codeserver_comparisons)
  - [docker_container_codeserver_env](#docker_container_codeserver_env)
  - [docker_container_codeserver_image](#docker_container_codeserver_image)
  - [docker_container_codeserver_labels](#docker_container_codeserver_labels)
  - [docker_container_codeserver_name](#docker_container_codeserver_name)
  - [docker_container_codeserver_networks](#docker_container_codeserver_networks)
  - [docker_container_codeserver_ports](#docker_container_codeserver_ports)
  - [docker_container_codeserver_restic_enable](#docker_container_codeserver_restic_enable)
  - [docker_container_codeserver_restic_retention](#docker_container_codeserver_restic_retention)
  - [docker_container_codeserver_restic_s3_bucket_name](#docker_container_codeserver_restic_s3_bucket_name)
  - [docker_container_codeserver_restic_s3_endpoint](#docker_container_codeserver_restic_s3_endpoint)
  - [docker_container_codeserver_restic_s3_repo](#docker_container_codeserver_restic_s3_repo)
  - [docker_container_codeserver_restic_s3_repo_access_key](#docker_container_codeserver_restic_s3_repo_access_key)
  - [docker_container_codeserver_restic_s3_repo_password](#docker_container_codeserver_restic_s3_repo_password)
  - [docker_container_codeserver_restic_s3_repo_secret_key](#docker_container_codeserver_restic_s3_repo_secret_key)
  - [docker_container_codeserver_restic_tag](#docker_container_codeserver_restic_tag)
  - [docker_container_codeserver_volume_dir](#docker_container_codeserver_volume_dir)
  - [docker_container_codeserver_volumes](#docker_container_codeserver_volumes)
  - [docker_image_codeserver_name](#docker_image_codeserver_name)
  - [docker_image_codeserver_pull](#docker_image_codeserver_pull)
  - [docker_network_codeserver_name](#docker_network_codeserver_name)
- [Discovered Tags](#discovered-tags)
- [Dependencies](#dependencies)
- [License](#license)
- [Author](#author)

---

## Requirements

- Minimum Ansible version: `2.1`

## Default Variables

### docker_container_codeserver_command

#### Default value

```YAML
docker_container_codeserver_command: [--disable-telemetry]
```

### docker_container_codeserver_comparisons

Allows to specify how properties of existing containers are compared with module options
to decide whether the container should be recreated / updated or not.

#### Default value

```YAML
docker_container_codeserver_comparisons:
  image: strict
  env: strict
  volumes: strict
```

### docker_container_codeserver_env

Dictionery of key,value pairs for docker
environment variables to configure codeserver.

Example:

```yaml

docker_container_codeserver_env:

codeserver__mailer__ENABLED: "false"

```

#### Default value

```YAML
docker_container_codeserver_env:
  DOCKER_USER: coder
```

### docker_container_codeserver_image

Repository path and tag used to create the container.
If an image is not found or pull is true, the image will be pulled from the registry.
If no tag is included, latest will be used.

#### Default value

```YAML
docker_container_codeserver_image: '{{ docker_image_codeserver_name }}'
```

### docker_container_codeserver_labels

Dictionary of key value pairs for container labels.

Example:

```yaml

docker_container_codeserver_labels:

traefik.enable: "true"

```

#### Default value

```YAML
docker_container_codeserver_labels: {}
```

### docker_container_codeserver_name

Name for the container

#### Default value

```YAML
docker_container_codeserver_name: codeserver
```

### docker_container_codeserver_networks

List of networks the container belongs to.

#### Default value

```YAML
docker_container_codeserver_networks:
  - name: '{{ docker_network_codeserver_name }}'
```

### docker_container_codeserver_ports

List of ports to publish from the container to the host.

#### Default value

```YAML
docker_container_codeserver_ports:
  - 8080:8080
```

### docker_container_codeserver_restic_enable

Enable restic backup for the container's mounted volumes.

#### Default value

```YAML
docker_container_codeserver_restic_enable: false
```

### docker_container_codeserver_restic_retention

Retention settions for `restic forget` after the `restic backup`.

#### Default value

```YAML
docker_container_codeserver_restic_retention:
  keep_last: 1
  keep_daily: 7
  keep_weekly: 4
```

### docker_container_codeserver_restic_s3_bucket_name

Minio S3 bucket name for restic backup storage.

#### Default value

```YAML
docker_container_codeserver_restic_s3_bucket_name: restic-{{ docker_container_codeserver_name
  }}
```

### docker_container_codeserver_restic_s3_endpoint

Minio S3 endpoint for restic backup storage.

Example:

```yaml

docker_container__base__restic_s3_endpoint: "https://minio.{{ dns_domain }}"

docker_container_codeserver_restic_s3_endpoint: "{{ docker_container__base__restic_s3_endpoint }}"

```

#### Default value

```YAML
docker_container_codeserver_restic_s3_endpoint: '{{ docker_container__base__restic_s3_endpoint
  }}'
```

### docker_container_codeserver_restic_s3_repo

Minio S3 repo URL for restic backup storage.

#### Default value

```YAML
docker_container_codeserver_restic_s3_repo: s3:{{ docker_container_codeserver_restic_s3_endpoint
  }}/{{ docker_container_codeserver_restic_s3_bucket_name }}
```

### docker_container_codeserver_restic_s3_repo_access_key

Minio S3 repo access key for restic backup storage.

#### Default value

```YAML
docker_container_codeserver_restic_s3_repo_access_key: '{{ docker_container__base__restic_s3_repo_access_key
  }}'
```

### docker_container_codeserver_restic_s3_repo_password

Minio S3 repo password for restic backup storage.

#### Default value

```YAML
docker_container_codeserver_restic_s3_repo_password: '{{ docker_container__base__restic_s3_repo_password
  }}'
```

### docker_container_codeserver_restic_s3_repo_secret_key

Minio S3 repo secret key for restic backup storage.

#### Default value

```YAML
docker_container_codeserver_restic_s3_repo_secret_key: '{{ docker_container__base__restic_s3_repo_secret_key
  }}'
```

### docker_container_codeserver_restic_tag

Tag for the `restic backup` command

#### Default value

```YAML
docker_container_codeserver_restic_tag: '{{ docker_container_codeserver_name }}'
```

### docker_container_codeserver_volume_dir

Volume mount host directory, where Treafik config files are stored.

#### Default value

```YAML
docker_container_codeserver_volume_dir: '{{ docker_container__base__volume_dir }}/{{
  docker_container_codeserver_name }}'
```

### docker_container_codeserver_volumes

List of volumes to mount within the container.

#### Default value

```YAML
docker_container_codeserver_volumes:
  - '{{ docker_container_codeserver_volume_dir }}/.local:/home/coder/.local'
  - '{{ docker_container_codeserver_volume_dir }}/.config:/home/coder/.config'
  - '{{ docker_container_codeserver_volume_dir }}/project:/home/coder/project'
```

### docker_image_codeserver_name

Repository path and tag for the container image.

#### Default value

```YAML
docker_image_codeserver_name: codercom/code-server:latest
```

### docker_image_codeserver_pull

Indicate to always pull the docker image.

#### Default value

```YAML
docker_image_codeserver_pull: false
```

### docker_network_codeserver_name

Name of the docker network created for codeserver.

#### Default value

```YAML
docker_network_codeserver_name: '{{ docker_container_codeserver_name }}_backend'
```

## Discovered Tags

**_docker-container-backup-all_**\
&emsp;Backup all containers' volume mounts.

**_docker-container-backup-codeserver_**\
&emsp;Backup codeserver volume mounts.

**_docker-container-backup-init-all_**\
&emsp;Run init backup task for all container.

**_docker-container-backup-init-codeserver_**\
&emsp;Run init backup task for codeserver if restic is enabled.

**_docker-container-backup-list-all_**\
&emsp;List all containers' backups.

**_docker-container-backup-list-codeserver_**\
&emsp;List codeserver backups.

**_docker-container-prereq-all_**\
&emsp;Ensure all pre-requisites are installed

**_docker-container-prereq-codeserver_**\
&emsp;Ensure all pre-requisites for codeserver are installed

**_docker-container-purge-all_**\
&emsp;Remove all containers and delete volume mounts.

**_docker-container-purge-codeserver_**\
&emsp;Remove codeserver and delete volume mounts.

**_docker-container-remove-all_**\
&emsp;Remove all containers.

**_docker-container-remove-codeserver_**\
&emsp;Remove codeserver.

**_docker-container-restore-all_**\
&emsp;Run restic restore for all restic enabled containers.

**_docker-container-restore-codeserver_**\
&emsp;Run restic restore for codeserver if restic is enabled.

**_docker-container-setup-all_**\
&emsp;Run setup task for all containers.

**_docker-container-setup-codeserver_**\
&emsp;Run setup task for codeserver.

**_never_**


## Dependencies

None.

## License

license (GPL-2.0-or-later, MIT, etc)

## Author

andif888
