Elasticsearch
=============

[![Build Status](https://travis-ci.org/jebovic/ansible-elasticsearch.svg?branch=master)](https://travis-ci.org/jebovic/ansible-elasticsearch) [![Ansible Galaxy](https://img.shields.io/badge/galaxy-jebovic.elasticsearch-blue.svg?style=flat)](https://galaxy.ansible.com/jebovic/elasticsearch)

Install and configure elasticsearch

This role is a part of my [OPS project](https://github.com/jebovic/ops), follow this link to see it in action. OPS provides a lot of stuff, like a vagrant file for development VMs, playbooks for roles orchestration, inventory files, examples for roles configuration, ansible configuration file, and many more.

Compatibility
-------------

Tested and approved on :

* Debian jessie (8+)
* Ubuntu Trusty (14.04 LTS)
* Ubuntu Xenial (16.04 LTS)

Dependencies
------------

This role depends on [jebovic.java](https://github.com/jebovic/ansible-java) role to be fully functional

Role Variables
--------------

```yaml
# elasticsearch install configuration
elasticsearch_dep_packages:
  - apt-transport-https
  - ca-certificates
elasticsearch_apt_key_url: "https://artifacts.elastic.co/GPG-KEY-elasticsearch"
elasticsearch_apt_repositories:
  - "deb https://artifacts.elastic.co/packages/{{ elasticsearch_major_version }}/apt stable main"
elasticsearch_packages:
  - elasticsearch
elasticsearch_daemon: elasticsearch

# elasticsearch JVM options
elasticsearch_jvm_xms: 512m
elasticsearch_jvm_xmx: 512m

# elasticsearch basic configuration
elasticsearch_major_version: "5.x"
elasticsearch_version: "5.1.1"
elasticsearch_user: elasticsearch
elasticsearch_group: elasticsearch
elasticsearch_conf_dir: "/etc/elasticsearch"
elasticsearch_pid_dir: "/var/run/elasticsearch"
elasticsearch_data_dir: "/var/lib/elasticsearch"
elasticsearch_log_dir: "/var/log/elasticsearch"
elasticsearch_work_dir: "/tmp/elasticsearch"
elasticsearch_http_host: 0.0.0.0
elasticsearch_http_port: 9201
elasticsearch_tcp_port: 9301
elasticsearch_logger_level: INFO
elasticsearch_config: {
  node.name: "node1",
  cluster.name: "elasticcluster",
  #discovery.zen.ping.unicast.hosts: "localhost:{{ elasticsearch_tcp_port }}",
  #discovery.zen.ping.multicast.enabled: false,
  discovery.zen.minimum_master_nodes: 1,
  http.port: "{{ elasticsearch_http_port }}",
  network.host: "{{ elasticsearch_http_host }}",
  transport.tcp.port: "{{ elasticsearch_tcp_port }}",
  node.data: true,
  node.master: true,
  bootstrap.memory_lock: false
}

# Kibana configuration
kibana_enabled: yes
kibana_apt_key_url: "https://artifacts.elastic.co/GPG-KEY-elasticsearch"
kibana_apt_repositories:
  - "deb https://artifacts.elastic.co/packages/{{ kibana_major_version }}/apt stable main"
kibana_packages:
  - kibana
kibana_daemon: kibana

# elasticsearch basic configuration
kibana_major_version: "5.x"
kibana_http_host: 0.0.0.0
kibana_http_port: 5601
kibana_es_http_host: localhost
```

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
     - { role: jebovic.java }
     - { role: jebovic.elasticsearch }
```

Example : config
----------------

```yaml
# Java configuration for elastic stack 5.x (java 8 needed)
java_apt_repositories:
  - "{% if (ansible_distribution == 'Ubuntu') %}ppa:openjdk-r/ppa{% else %}deb http://http.debian.net/debian jessie-backports main{% endif %}"
java_packages:
  - openjdk-8-jre

# Start elasticsearch with max memory upper than 512Mo and different data dir
elasticsearch_jvm_xms: 1g
elasticsearch_jvm_xmx: 1g
elasticsearch_data_dir: /srv/data
```

Tags
----

* elasticsearch_config : only update config and restart elasitcsearch service
* kibana : only install and start kibana service
* kibana_config : only update config and restart kibana service

License
-------

MIT

Author Information
------------------

Jérémy Baumgarth https://github.com/jebovic
