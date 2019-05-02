# Apache ZooKeeper

[![Build Status](https://travis-ci.org/boringguy/ansible-solrcloud.svg?branch=master)](https://travis-ci.org/boringguy/ansible-solrcloud)

Ansible role for installing and configuring Apache SolrCloud on RHEL / CentOS 7.

This role can be used to install and cluster multiple Solr nodes, this uses all hosts defined for the "solr" group
in the inventory file by default.

## Requirements

Platform: RHEL / CentOS 7

Java: Java 8
Zookeeper: 

The Zookeeper role from Ansible Galaxy can be used if one is needed.

`$ ansible-galaxy install sleighzy.zookeeper`

## Role Variables

    solr_version - Solr version
    solr_auth_user - Solr admin user
    solr_auth_pass - Solr admin password
    solr_port - Solr port
    solr_zookeeper_hosts - Zookeeper connection string (e.g. server-1:2181,server-2:2181,server-3:2181)
    solr_zookeeper_client_timeout - Zookeeper client timeout

    solr_servers: "{{groups['solr']}}" - Group of solr servers


### Default Ports

| Port | Description |
|------|-------------|
| 2181 | Client connection port |
| 2888 | Quorum port for clustering |
| 3888 | Leader election port for clustering |
| 8983 | Solr Web Client |


### Default Directories and Files

 Description                               | Directory / File 
-------------------------------------------|------------------
Installation directory                     | `/opt/solr/solr-<version>`
Symlink to install directory               | `/opt/solr` 
Configuration                              | `/etc/default/solr.in.sh` 
Log files                                  | `/var/log/solr` 
Solr home directory                        | `/var/solr`
Data directory                             | `/var/solr/data` 
Systemd service                            | `/usr/lib/systemd/system/solr.service` 

## Starting and Stopping ZooKeeper services
* The ZooKeeper service can be started via: `systemctl start zookeeper`
* The ZooKeeper service can be stopped via: `systemctl stop zookeeper`

## Starting and Stopping Solr services
* The Solr service can be started via: `systemctl start solr`
* The Solr service can be stopped via: `systemctl stop solr`

## Dependencies

Zookeeper

## Example Playbook

```yaml
- hosts: zookeeper-nodes

  tasks:
  - name: Install Java 8 (OpenJDK)
    yum:
      name: java-1.8.0-openjdk
      state: latest

  roles:
    - sleighzy.zookeeper

# Install Solr
- hosts: solr-nodes

  tasks:
  - name: Install Java 8 (OpenJDK)
    yum:
      name: java-1.8.0-openjdk
      state: latest
      
  - name: Install lsof
    yum:
      name: lsof
      state: latest

  roles:
    - ansible-solrcloud
    ```