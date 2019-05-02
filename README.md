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

    solr_version: 7.5.0
    solr_auth_user: sunrise
    solr_auth_pass: yYWmPj9fVDGq5aft
    solr_port: 8983
    solr_zookeeper_hosts: server-1:2181,server-2:2181,server-3:2181
    solr_zookeeper_client_timeout: 15000

    solr_servers: "{{groups['solr']}}"


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
* The ZooKeeper service can be started via: `systemctl start solr`
* The ZooKeeper service can be stopped via: `systemctl stop solr`

## Dependencies

Zookeeper

## Example Playbook

    - hosts: zookeeper-nodes

    tasks:
    - name: Install Java 8 (OpenJDK)
        yum:
        name: java-1.8.0-openjdk
        state: latest

    roles:
        - sleighzy.zookeeper

    - hosts: solr-nodes

    tasks:
    - name: Install lsof
        yum:
        name: lsof
        state: latest
    
    - name: Install Java 8 (OpenJDK)
        yum:
        name: java-1.8.0-openjdk
        state: latest

    roles:
        - ansible-solrcloud