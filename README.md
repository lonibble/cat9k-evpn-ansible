# General description #

The main goal of this project is creating ansible playbooks for automating Campus EVPN Deployment.

Each directory has scripts for specific steps for EVPN provisioning:
* **dag_add** DAG (Distributed Anycast Gateway) provisioning
* **dag_delete** DAG selective unprovisioning

# Playbooks description #

Each directory may has several playbook. Usually there are 4 of them:

```
playbook_underlay.yml:
      underlay config. Please check the host_vars/xx.yml

playbook_overlay.yml: 
       overlay config. Please refer to the overlay configuration parameters

playbook_output.yml:  
        executes overlay show commands including the config per node into output/xxx.txt

playbook_all.yml:      
         run above three.

Each of the playbooks above can be run independantly.
```

# Variables config description #

Configuration on the fabric switch usually consists from two parts:
- individual node configuration. Ususally it is Underlay config with unique ip addresses, interfaces, etc.
- shared configuration. Usually it is Overlay part like VLANs, SVIs, EVIs, etc.

Individual part of the configuration is stored in **host_vars/Leaf-xx.yml** and **host_vars/Spine-xx.yml**.
This configuration will be applied to the specific node only.

Shared configuration is stored in **groups/all.yml**.
This configuration will be applied to all nodes in group.

File **inventory.yaml** has an information about switch ip addresses, hostnames and groups that it belongs to.

## Observe changes on the switch ##

For checking the configuration that is deployed by Ansible on the switch next configuration could be used.

```
conf t
archive
 log config
  logging enable
  notify syslog contenttype plaintext
end
term mon
```