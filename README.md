# Ansible Galaxy role for install and configure Bareos-FD (ver. 22).

![Build Status](https://github.com/leadlineit/ansible-role-bareos_fd/actions/workflows/ansible-galaxy-ci.yml/badge.svg)
[![Galaxy Role](https://img.shields.io/badge/Ansible--Galaxy-leadlineit.bareos_fd-blue.svg?logo=ansible&logoColor=white)](https://galaxy.ansible.com/leadlineit/bareos_fd/)

This role helps to install and configure Bareos-FD (ver. 22).

Supported OSes
--------------
- Debian 12 (bookworm)
- Debian 11 (bullseye)
- Debian 10 (buster)
- RHEL 9 (CentOS Stream 9)
- RHEL 8 (CentOS Stream 8)
- RHEL 7 (CentOS 7)

Requirements
------------

This role requires Ansible 2.11 or higher.

Role Variables
--------------

The variables that can be passed to this role and a brief description about them are as follows:

```yaml
---
bareos_tls_path: /etc/bareos/tls
bareos_tls_certs: your.bareos.dir.com

bareos_fd:
  director:
    - name: your-dir
      description: Director, who is permitted to contact this file daemon.
      password: DIRAver@gEStr0ngPaSSw0rd
      tls_enable: "yes"
    - name: your-mon
      description: Restricted Director monitor description
      password: MONAver@gEStr0ngPaSSw0rd
      monitor: "yes"
      tls_enable: "yes"
  client:
    - name: your-client
      description: Your Bareos client
      fdport: 9102
      max_jobs: 20
      tls_enable: "yes"
  messages:
    - name: your-messages
      description: Messages description
      server: your-dir
```

The variables above are optional. They don't have a default value, so if you don't define them - tasks using them will be skipped. 
You can set only some of them, or not set at all (in this case, you will simply install Bareos-FD with default configuration).

You can install and adjust Percona XtraBackup for MySQL bkp with:

```yaml
---
  percona_xtrabackup: "yes"
```

The version of Percona XtraBackup will be dependent on your MySQL Server('mysql-community-server') version.
Say "yes", only when you had 'mysql-community-server' on host.

Also, you can use HashiCorp Vault for store client certificates (when you use Bareos with TLS)
Variable for this (optional too):

```yaml
---
  hashicorp_vault:
    address: your.vault.com
    token: your_token
    path: your-path-to-certs
    clients:
      - name: host1.client1
        client: client1
        role: role1
        ttl: 24h
      - name: host2.client1
        client: client1
        role: role1
        ttl: 18h
      - name: host01.client2
        client: client2
        role: client2
        ttl: 12h
      - name: host02.client2
        client: client2
        role: client2
        ttl: 96h
```

Another thing you can do is remotely add client configuration to your main Bareos Director server.
Variable for this (optional too):

```yaml
---
bareos_server: you.bareos.dir.server

bareos_dir:
  client:
    - name: your-client
      description: Your client configuration
      address: 10.0.0.1
      fdport: 9102
      max_jobs: 20
      passive: "yes"
      tls_enable: "yes"
      jobs:
        - name: client-job1
          description: Job1 for client
          client: client.name.com
          jobdef: your-jobdefs1
        - name: client-job2
          description: Job2 for client
          pool: your-pool
          fileset: "your-fileset"
          schedule: "your-schedule"
```

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- hosts: servers
  roles:
    - { role: leadlineit.bareos_fd, tags: bareos_fd }
```

License
-------

MIT

Author Information
------------------

This role was created by Artem Kasianchuk.
