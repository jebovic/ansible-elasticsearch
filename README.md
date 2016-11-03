Elasticsearch
=============

[![Build Status](https://travis-ci.org/jebovic/ansible-elasticsearch.svg?branch=master)](https://travis-ci.org/jebovic/ansible-elasticsearch) [![Ansible Galaxy](https://img.shields.io/badge/galaxy-jebovic.elasticsearch-blue.svg?style=flat)](https://galaxy.ansible.com/jebovic/elasticsearch)

Install and configure elasticsearch

This role is a part of my [OPS project](https://github.com/jebovic/ops), follow this link to see it in action. OPS provides a lot of stuff, like a vagrant file for development VMs, playbooks for roles orchestration, inventory files, examples for roles configuration, ansible configuration file, and many more.

Dependencies
------------

This role depends on [jebovic.java](https://github.com/jebovic/ansible-java) role to be fully functional

Role Variables
--------------

```yaml
# elasticsearch install configuration
elasticsearch_apt_key_url: "http://packages.elasticsearch.org/GPG-KEY-elasticsearch"
elasticsearch_apt_repositories:
  - "deb http://packages.elastic.co/elasticsearch/{{ elasticsearch_major_version }}/debian stable main"
elasticsearch_packages:
  - elasticsearch
elasticsearch_daemon: elasticsearch

# elasticsearch basic configuration
elasticsearch_major_version: "2.x"
elasticsearch_version: "2.3.4"
elasticsearch_user: elasticsearch
elasticsearch_group: elasticsearch
elasticsearch_conf_dir: "/etc/elasticsearch"
elasticsearch_pid_dir: "/var/run/elasticsearch"
elasticsearch_data_dir: "/var/lib/elasticsearch"
elasticsearch_log_dir: "/var/log/elasticsearch"
elasticsearch_work_dir: "/tmp/elasticsearch"
elasticsearch_api_host: "localhost"
elasticsearch_api_port: 9200
elasticsearch_logger_level: INFO
elasticsearch_config: {
  node.name: "node1",
  cluster.name: "elasticcluster",
  discovery.zen.ping.unicast.hosts: "localhost:9301",
  http.port: 9201,
  network.host: 0.0.0.0,
  transport.tcp.port: 9301,
  node.data: false,
  node.master: true,
  bootstrap.mlockall: true,
  discovery.zen.ping.multicast.enabled: false
}
```

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
     - { role: jebovic.java }
     - { role: jebovic.elasticsearch }
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
