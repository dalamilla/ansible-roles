MariaDB
=========

Installation and configuration of Mariadb.

Requirements
------------

None.

Role Variables
--------------

The variable "mariadb_root_pass" are required. if variables are not defined, the default variables are used.

| Variable               | Default                                  | Description                                           |
| -----------------------| -----------------------------------------| ------------------------------------------------------|
| **mariadb_version**    | '10.5'                                   | Version of mariadb.                                   |
| **mariadb_url**        | 'https://mariadb.org'                    | URL of mariadb key.                                   |
| **mariadb_mirror_url** | 'http://sfo1.mirrors.digitalocean.com'   | URL of mariadb mirror package.                        |
| **mariadb_channel**    | 'main'                                   | Channel of mariadb installation.                      |
| **mariadb_root_pass**  | ''                                       | Password of root user.                                |
| **mariadb_conf_biadd** | '0.0.0.0'                                | Mariadb bind addrees configuration.                   |
| **mariadb_databases**  | []                                       | List of dicts of dbs (check usage below).             |
| **mariadb_users**      | []                                       | List of dicts of users (check usage below).           |

Creating databases:

DB dict with fields "name" and default values are "encoding" = utf8 and "state" = present.

```yaml
mariadb_databases:
  - name: milkyway
    encoding: latin1
  - name: leo
    state: absent
```

Creating users:

User dict with fields "name", "password", "privs" and default value are "host" = localhost, "append_privs" = no and "state" = present.

```yaml
mariadb_users:
  - name: user1
    password: '{{ vault_mariadb_user1_pass }}'
    priv: 'milkyway.*:ALL'
    append_privs: 'yes'
  - name: user2
    password: '{{ vault_mariadb_user2_pass }}'
    priv: '*.*:ALL,GRANT'
    host: 10.0.0.10
  - name: user3
    password: '{{ vault_mariadb_user3_pass }}'
    priv: 'leo.*:ALL'
    state: absent
```

Dependencies
--------------

None.

Example Playbook
----------------

```yaml
- hosts: mariadb
  become: true
  vars:
    mariadb_version: '10.5'
    mariadb_mirror_url: 'http://nyc2.mirrors.digitalocean.com'
    mariadb_root_pass: '{{ vault_mariadb_root_pass }}'
    mariadb_databases:
      - name: milkyway
    mariadb_users:
      - name: galileo
        password: '{{ vault_mariadb_galileo_pass }}'
        host: '10.0.0.%' 
        priv: 'milkyway.*:ALL'
  roles:
    - role: Mariadb
```
