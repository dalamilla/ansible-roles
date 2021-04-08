LXD
=========

Installation and creation of default profile of LXD.

Requirements
------------

None.

Role Variables
--------------

None of the variables are required. if variables are not defined, the default variables are used.

| Variable                     | Default                                                  | Description                                 |
| -----------------------------| ---------------------------------------------------------| --------------------------------------------|
| **lxd_version**              | '4.0/stable'                                             | Channel version of snap lxd.                |
| **lxd_ipv4_address**         | '10.233.62.1'                                            | IP4 address of interface lxdbr0.            |
| **lxd_ipv6_address**         | 'fd42:620f:5094:29f0::1'                                 | IP6 address of interface lxdbr0.            |
| **lxd_storage_driver**       | 'dir'                                                    | Driver of Storage Pool.                     |
| **lxd_storage_config**       | source: '/var/snap/lxd/common/lxd/storage-pools/default' | Configuration of Driver.                    |

Dependencies
--------------

None.

Example Playbook
----------------

```yaml
- hosts: lxd
  become: true
  vars:
    lxd_version: 'latest/stable'
  roles:
      - role: lxd
```
