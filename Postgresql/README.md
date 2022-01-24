Postgresql
=========

Installation and simple configuration of Postgresql.

Requirements
------------

None.

Role Variables
--------------

None of the variables are required. if variables are not defined, the default variables are used.

| Variable                 | Default                                              | Description                                                          |
| -------------------------| -----------------------------------------------------| ---------------------------------------------------------------------|
| **postgresql_version**   | '14'                                                 | Version of Postgresql.                                               |
| **postgresql_key_url**   | 'https://www.postgresql.org/media/keys/ACCC4CF8.asc' | URL of Postgresql key.                                               |
| **postgresql_pkg_url**   | 'http://apt.postgresql.org/pub/repos/apt'            | URL of Postgresql apt repo.                                          |
| **postgresql_channel**   | 'main'                                               | Channel of Postgresql installation.                                  |
| **postgresql_config**    | {}                                                   | Dict of Postgresql configuration (check default below).              |
| **postgresql_hba**       | []                                                   | List of dicts of Postgresql HBA configuration (check default below). |
| **postgresql_databases** | []                                                   | List of dicts of dbs (check usage below).                            |
| **postgresql_users**     | []                                                   | List of dicts of users (check usage below).                          |


Default configuration for postgresql.conf (postgresql_config variable):

```yaml
postgresql_config:
  data_directory: '/var/lib/postgresql/{{ postgresql_version }}/main'	
  hba_file: '/etc/postgresql/{{ postgresql_version }}/main/pg_hba.conf'
  ident_file: '/etc/postgresql/{{ postgresql_version }}/main/pg_ident.conf'
  external_pid_file: '/var/run/postgresql/{{ postgresql_version }}-main.pid'
  listen_addresses: '*'
  port: '5432'
  max_connections: '100'
  unix_socket_directories: '/var/run/postgresql'
  ssl: 'on'
  ssl_cert_file: '/etc/ssl/certs/ssl-cert-snakeoil.pem'
  ssl_key_file: '/etc/ssl/private/ssl-cert-snakeoil.key'
  shared_buffers: '128MB'
  dynamic_shared_memory_type: 'posix'
  max_wal_size: '1GB'
  min_wal_size: '80MB'
  log_line_prefix: '%m [%p] %q%u@%d '
  log_timezone: 'Etc/UTC'
  cluster_name: '{{ postgresql_version }}/main'	
  stats_temp_directory: '/var/run/postgresql/{{ postgresql_version }}-main.pg_stat_tmp'
  datestyle: 'iso, mdy'
  timezone: 'Etc/UTC'
  lc_messages: 'C.UTF-8'
  lc_monetary: 'C.UTF-8'
  lc_numeric: 'C.UTF-8'
  lc_time: 'C.UTF-8'
  default_text_search_config: 'pg_catalog.english'
  include_dir: 'conf.d'
```

Default configuration for pg_hba.conf (postgresql_hba variable):

```yaml
postgresql_hba:
  - {type: 'local', database: 'all', user: 'postgres', address: '',  method: 'peer'}
  - {type: 'local', database: 'all', user: 'all', address: '',  method: 'peer'}
  - {type: 'host', database: 'all', user: 'all', address: '127.0.0.1/32',  method: 'scram-sha-256'}
  - {type: 'host', database: 'all', user: 'all', address: '::1/128',  method: 'scram-sha-256'}
  - {type: 'local', database: 'replication', user: 'all', address: '',  method: 'peer'}
  - {type: 'host', database: 'replication', user: 'all', address: '127.0.0.1/32',  method: 'scram-sha-256'}
  - {type: 'host', database: 'replication', user: 'all', address: '::1/128',  method: 'scram-sha-256'}
```

Creating databases:

DB dict with fields "name" and default values are "encoding" = UTF-8, "lc_collate" = C.UTF-8, lc_ctype = C.UTF-8, "template" = template0, "owner" = postgres and "state" = present.

```yaml
postgresql_databases:
  - name: 'jaxa'
    lc_collate: 'ja_JP.UTF8'
    lc_ctype: 'ja_JP.UTF8'
  - name: esa
    lc_collate: 'en_GB.UTF8'
    lc_ctype: 'en_GB.UTF8'
```

Creating users:

User dict with fields "name", "password", "privs", "type", "objs", "db" and default value its "state" = present.

```yaml
postgresql_users:
  - name: xrism
    password: '{{ vault_postgresql_xrism_pass }}'
    privs: 'ALL'
    type: 'default_privs'
    objs: ALL_DEFAULT'
    db: jaxa
  - name: swas
    password: '{{ vault_postgresql_swas_pass }}'
    privs: 'ALL'
    type: 'default_privs'
    objs: ALL_DEFAULT'
    db: esa
```

Dependencies
--------------

None.

Example Playbook
----------------

```yaml
- hosts: postgresql
  become: true
  vars:
    postgresql_databases:
      - name: 'jaxa'
        lc_collate: 'ja_JP.UTF8'
        lc_ctype: 'ja_JP.UTF8'
    postgresql_users:
      - name: xrism
        password: '{{ vault_postgresql_xrism_pass }}'
        privs: 'ALL'
        type: 'default_privs'
        objs: ALL_DEFAULT'
        db: jaxa
  roles:
    - role: Postgresql
```
