Consul Ansible Role
===================

An ansible role for installing Hashicorp Consul

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

consul_version: 1.0.1
consul_version_latest: 1.0.1
consul_name: "consul_{{ consul_version }}"
consul_archive: "{{ consul_name }}_linux_amd64.zip"
consul_webui_archive: "{{ consul_name }}_web_ui.zip"
consul_download_path: "https://releases.hashicorp.com/consul/{{ consul_version }}"
consul_download_checksum: "sha256:eac5755a1d19e4b93f6ce30caaf7b3bd8add4557b143890b1c07f5614a667a68"
consul_webui_download_checksum: "sha256:9212c44c3ee4acaeca88d794225a2858a558be531a3cd44c741990c4493d6f12"
consul_user: consul
consul_group: configuration
consul_server_enabled: no
consul_webui_enabled: no
consul_agent_log_level: INFO
cluster_name: main

consul_join_hosts: list of ip addresses 
consul_retry_interval


Dependencies
------------

- mjcramer.system

Tags
----
- require
- download
- apply
- configure
- initialize
- check

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: mjcramer.consul, x: 42 }

License
-------

Unlicensed

Author Information
------------------

Michael J. Cramer (github: mjcramer), michael@cramer.name
