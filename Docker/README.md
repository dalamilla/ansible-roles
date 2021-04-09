Docker
=========

Installation and simple configuration of Docker.

Requirements
------------

None.

Role Variables
--------------

None of the variables are required. if variables are not defined, the default variables are used.

| Variable                     | Default                                                                | Description                                              |
| -----------------------------| -----------------------------------------------------------------------| ---------------------------------------------------------|
| **docker_url**               | 'https://download.docker.com/linux/{{ ansible_distribution \| lower }}' | URL of docker repository.                                |
| **docker_channel**           | 'stable'                                                               | Channel of docker installation.                          |
| **docker_config**            | {}                                                                     | Dict of docker daemon.json config (check default below). |


Default configuration for daemon.json (docker_config variable):

```yaml
  storage-driver: 'overlay2'
  log-driver: 'json-file'
  log-opts:
    max-size: '10m'
    max-file: '3'
  live-restore: true
```

Dependencies
--------------

None.

Example Playbook
----------------

```yaml
- hosts: docker
  become: true
  vars:
    docker_config:
      storage-driver: 'btrfs'
  roles:
      - role: docker
```
