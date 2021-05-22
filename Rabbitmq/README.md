Rabbitmq
=========

Installation and simple configuration of Rabbitmq.

Requirements
------------

None.

Role Variables
--------------

None of the variables are required. if variables are not defined, the default variables are used.

| Variable                    | Default                                                                     | Description                                       |
| ----------------------------| ----------------------------------------------------------------------------| --------------------------------------------------|
| **rabbitmq_key_server**     | 'hkps://keys.openpgp.org'                                                   | URL of rabbitmq key server.                       |
| **rabbitmq_key_id**         | '0x0A9AF2115F4687BD29803A206B73A36E6026DFCA'                                | Rabbitmq key id.                                  |
| **rabbitmq_url**            | 'https://dl.cloudsmith.io/public/rabbitmq'                                  | URL of rabbitmq repo                              |
| **rabbitmq_erlang_key**     | 'gpg.E495BB49CC4BBE5B.key'                                                  | Key of rabbitmq-erlang.                           |
| **rabbitmq_server_key**     | 'gpg.9F4587F226208342.key'                                                  | Key of rabbitmq-server.                           |
| **rabbitmq_erlang_pkg_url** | '{{ rabbitmq_url }}/rabbitmq-erlang/deb/{{ ansible_distribution | lower }}' | URL of rabbitmq-erlang repo.                      |
| **rabbitmq_server_pkg_url** | '{{ rabbitmq_url }}/rabbitmq-server/deb/{{ ansible_distribution | lower }}' | URL of rabbitmq-server repo.                      |
| **rabbitmq_channel**        | 'main'                                                                      | Channel of rabbitmq installation.                 |
| **rabbitmq_config**         | {}                                                                          | Dict of rabbitmq.conf file (check conf below).    |
| **rabbitmq_users**          | []                                                                          | List of dicts of users (check conf below).     |
| **rabbitmq_vhosts**         | []                                                                          | List of dicts of vhost (check conf below).     |


Configuration dict for rabbitmq.conf (rabbitmq_config variable), if it's not defined doesn't create the file (default installation):

```yaml
rabbitmq_config:
  vm_memory_high_watermark.relative: 0.7
  disk_free_limit.relative: 1.5
```

Creating vhosts:

User dict with fields "name" and default value are "state" = present.

```yaml
rabbitmq_vhosts:
  - name: /neptune
  - name: /pluto
    state: absent
```

Creating users:

User dict with fields "name", "password" and default values are "vhost" = /, "configure_priv" = .*, "read_priv" = .*, "write_priv" = .* and "state" = present.

```yaml
rabbitmq_users:
  - name: fobos
    password: '{{ vault_rabbitmq_fobos_pass }}'
    vhost: '/mars'
    read_priv: '.'
  - name: deimos
    password: '{{ vault_rabbitmq_deimos_pass }}'
    vhost: '/mars'
  - name: 'moon'
    vhost: '/mars'
    state: absent
```

Dependencies
--------------

None.

Example Playbook
----------------

```yaml
- hosts: rabbitmq
  become: true
  vars:
    rabbitmq_vhosts:
      - name: /mars
      - name: /jupiter
      - name: /saturn
    rabbitmq_users:
      - name: titan
        password: '{{ vault_rabbitmq_titan_pass }}'
        vhost: '/saturn'
  roles:
    - role: Rabbitmq
```
