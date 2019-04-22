Migration from Red Hat A-MQ 6 to Red Hat AMQ 7
==============================================

This is a role is designed to migrate Messaging Services from Red Hat A-MQ 6 to Red Hat AMQ 7 on OpenShift.

The Broker Cluster and namespace lists are defined in ```namespaces.yml``` files in [vars folder](../../vars).

Requirements
------------

* [OpenShift CLI](https://docs.openshift.com/container-platform/3.11/cli_reference/get_started_cli.html)

Role Variables
--------------

### Inputs

Some vars used by this role are defined dinamically by the Ansible playbook and the rest are defined in different vars files.

**Set Fact**

|Variable|Description|Type|
|---|---|---|
|ocp|Red Hat OpenShift Master URL|String|
|ocp_token|Red Hat OpenShift Access Token|String|

**Include Role Vars**

|Variable|Description|Type|
|---|---|---|
|namespace|Red Hat OpenShift namespace or project|String|
|amq6_name|Red Hat A-MQ 6 Broker Cluster name|String|
|amq_name|Red Hat AMQ 7 Broker Cluster name|String|
|amq_replicas|Red Hat AMQ 7 Broker pod replica count|String|
|state|Red Hat AMQ 7 Broker Cluster state|String|

### Outputs

None.

Example Playbook
----------------

Including an example of how to use this role:

```
- hosts: localhost
  gather_facts: no
  vars_files:
  - vars/namespaces.yml
  vars_prompt:
  - name: "ocp"
    prompt: "Enter OCP Master URL"
    private: no
  - name: "ocp_username"
    prompt: "Enter OCP username"
    private: no
  - name: "ocp_password"
    prompt: "Enter OCP password"
    private: yes
  tasks:
  - name: "[include_role] Role Name: ocp-get-token"
    include_role:
      name: ocp-get-token
  - name: "[include_role] Role Name: migration-amq6-to-amq7"
    include_role:
      name: migration-amq6-to-amq7
    vars:
      namespace: "{{ item.namespace }}"
      amq6_name: "{{ item.amq6_name }}"
      amq_name: "{{ item.amq_name }}"
      state: "{{ item.state }}"
    with_items: "{{ namespaces }}"
    when: item.amq6_name is not undefined
```

License
-------

BSD

Author Information
------------------

* Francisco Rodríguez Rocha ([fran@redhat.com](mailto:fran@redhat.com))
* Román Martín Gil ([roman.martin@redhat.com](mailto:roman.martin@redhat.com))

Main References
---------------

* [Migrating to Red Hat AMQ 7](https://access.redhat.com/documentation/en-us/red_hat_amq/7.2/html/migrating_to_red_hat_amq_7/index)
