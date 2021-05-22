Mongodb
=========

Installation and simple configuration of Mongodb.

Requirements
------------

None.

Role Variables
--------------

The variable "mongodb_root_pass" are required. if variables are not defined, the default variables are used.

| Variable                            | Default                                                               | Description                                                      |
| ------------------------------------| ----------------------------------------------------------------------| -----------------------------------------------------------------|
| **mongodb_version**                 | '4.4'                                                                 | Version of Mongodb                                               |
| **mongodb_key_url**                 | 'https://www.mongodb.org/static/pgp/server-{{ mongodb_version }}.asc' | URL of Mongodb key.                                              |
| **mongodb_pkg_url**                 | 'https://repo.mongodb.org/apt/{{ ansible_distribution | lower }}'     | URL of Mongodb repo package.                                     |
| **mongodb_root_pass**               | ''                                                                    | Password of root user.                                           |
| **mongodb_users**                   | []                                                                    | List of dicts of users (check default below).                    |
| **mongodb_conf_storage**            | {}                                                                    | Dict of Mongodb storage config (check default below).            |
| **mongodb_conf_systemLog**          | {}                                                                    | Dict of Mongodb systemLog config (check default below).          |
| **mongodb_conf_net**                | {}                                                                    | Dict of Mongodb net config (check default below).                |
| **mongodb_conf_processManagement**  | {}                                                                    | Dict of Mongodb processManagement config (check default below).  |
| **mongodb_conf_security**           | {}                                                                    | Dict of Mongodb security config (check default below).           |
| **mongodb_conf_operationProfiling** | {}                                                                    | Dict of Mongodb operationProfiling config.                       |
| **mongodb_conf_replication**        | {}                                                                    | Dict of Mongodb replication config.                              |
| **mongodb_conf_sharding**           | {}                                                                    | Dict of Mongodb sharding config.                                 |


Creating users:

User dict with fields "name", "password", "db" and default values are "roles" = read and "state" = present.

```yaml
mongodb_users:
  - name: dorotheae
    password: '{{ vault_mongodb_dorotheae_pass }}'
    db: lithops
    roles: readWrite,dbAdmin
  - name: francisci
    password: '{{ vault_mongodb_francisci_pass }}'
    db: lithops
  - name: echeveria
    db: lithops
    state: absent
```

Default configuration for storage (mongodb_conf_storage variable):

```yaml
mongodb_conf_storage: 
  dbPath: /var/lib/mongodb
  journal:
    enabled: true
```

Default configuration for systemLog (mongodb_conf_systemLog variable):

```yaml
mongodb_conf_systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log
```

Default configuration for net (mongodb_conf_net variable):

```yaml
mongodb_conf_net:
  port: 27017
  bindIp: 127.0.0.1
```

Default configuration for processManagement (mongodb_conf_processManagement variable):

```yaml
mongodb_conf_processManagement:
  timeZoneInfo: /usr/share/zoneinfo
```

Default configuration for security (mongodb_conf_security variable):

```yaml
mongodb_conf_security:
  authorization: enabled
```

Dependencies
--------------

None.

Example Playbook
----------------

```yaml
- hosts: Mongodb
  become: true
  vars:
    mongodb_root_pass: '{{ vault_mongodb_root_pass }}'
    mongodb_users:
      - name: acreana
        password: '{{ vault_mongodb_acreana_pass }}'
        db: monstera
        roles: readWrite,dbAdmin
      - name: deliciosa
        password: '{{ vault_mongodb_deliciosa_pass }}'
        db: monstera
        roles: readWriteAnyDatabase
  roles:
    - role: Mongodb
```
