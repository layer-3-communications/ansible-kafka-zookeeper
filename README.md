# Ansible Kafka Zookeeper

[![Build Status](https://travis-ci.org/jaytaylor/ansible-kafka.svg?branch=master)](https://travis-ci.org/jaytaylor/ansible-kafka)
[![Galaxy](https://img.shields.io/badge/galaxy-jaytaylor.kafka-blue.svg)](https://galaxy.ansible.com/list#/roles/4083)

An ansible role to install and configure [kafka](https://kafka.apache.org/) distributed pub/sub messaging queue clusters.


## How to get it

Add to your playbooks requirements.yml:

    - src: https://github.com/layer-3-communications/ansible-kafka-zookeeper

and then run:

    ansible-galaxy install -r requirements.yml --ignore-errors

Or if you just want to grab this role and start using it:

    cd my-playbook-folder/roles
    git clone https://github.com/layer-3-communications/ansible-kafka-zookeeper.git
    cd -

## How to use it

Once the role is installed, configure it from your playbook file (e.g. playbook.yml).

Example:

```yml
- hosts: kafka
  become: yes
  vars:
    kafka_server:
      listeners: "PLAINTEXT://0.0.0.0:9092"
      advertised_listeners: "PLAINTEXT://{{inventory_hostname}}:9092"
      host_name: "{{inventory_hostname}}"
    kafka_java_version: openjdk-8-jre-headless
    kafka_hosts: "{{play_hosts}}"
    kafka_zookeeper_hosts: "{{play_hosts}}"
    kafka_version: 2.0.0 # Kafka version override.
    kafka_scala_serverion: 2.10 # Scala version override.
    offsets_topic_num_partitions: 12
  roles:
    - role: ansible-kafka-zookeeper

```

## Role variables

- `kafka_hosts` - list of hosts in the cluster.
- `kafka_zookeeper_hosts` - list of zookeeper hosts for the cluster.
- `kafka_broker_id` - Integer uniquely identifying the broker, by default one will be generated for you either by this role or by kafka itself for versions >= 0.9.
- `kafka_generate_broker_id` - Flag controlling whether to generate a broker id, defaults to `yes`.
- `kafka_server_defaults` - Default Kafka server settings. This variable shoulnd't usually be changed.
- `kafka_producer_defaults` - Default Kafka producer settings. This variable shouldn't usually be changed.
- `kafka_server` - Allows to overwrite particular default server settings (from `kafka_server_defaults` variable). Values in this hash are combined with `kafka_server_defaults` variable.
- `kafka_producer` - Allows to overwrite particular default producer settings (from `kafka_producer_defaults` variable). Values in this hash are combined with `kafka_server_defaults` variable.
- `kafka_healthcheck_address` - If not defined, this will dynamicaly set to `kafka_server_defaults.host_name` or `kafka_server.host_name` if defined. Defaults to '127.0.0.1'
- `kafka_java_version` - Java version to install. Defaults to "openjdk-7-jre-headless"

## License

BSD

## Author

Jay Taylor

[@jtaylor](https://twitter.com/jtaylor) - [jaytaylor.com](http://jaytaylor.com) - [github](https://github.com/jaytaylor)
