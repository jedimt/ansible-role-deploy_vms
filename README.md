Ansible Role: Deploy VMs
=========

Deploys one or more VMs from a template in vCenter.

Requirements
------------

- Host packages
    apt update && sudo apt install nfs-common sshpass -y

- OVFTool 4.5+ (for vSphere 8)

- Pyvmomi and jmespath
    python3 -m pip install pyvmomi jmespath

Role Variables
--------------

The vCenter username and password are intended to be stored in Ansible Vault like so:

    # vCenter credentials, stored in an Ansible Vault
    vault_vcenter_username: youraccounthere@vsphere.local
    vault_vcenter_password: supersekritpassword

There are VM specific variables defined in the inventory for each VM to be deployed by default, including the IP address, notes, disk size, memory size and number of vCPUs. For example:

    k8s_master:
      hosts:
        dev-k8s-master-01.tme.nebulon.com:
          guest_custom_ip: '10.100.24.41'
          guest_notes: "Dev K8s control node"
          size_gb: 40
          memory_mb: 8192
          guest_vcpu: 2
          loadbalancer: "dev-k8s-haproxy-01.tme.nebulon.com:6443"

All other variables are defined in the vars/main.yml file.

Dependencies
------------

There must be a virtual machine template in the inventory that can be used for creating the clones.

Example Playbook
----------------

    # ===========================================================================
    # Deploy VMs to a vCenter server
    # ===========================================================================
    - name: Deploy VMs
      hosts: localhost
      connection: local
      gather_facts: false
      tags: play_deploy_vms

      vars_files:
        # Ansible vault with all required passwords
        - "/home/apatt/github/demopod-ansible/credentials.yml"

      roles:
        - ansible-role-deploy-vms

License
-------

MIT

Author Information
------------------

Aaron Patten
aaronpatten@gmail.com
