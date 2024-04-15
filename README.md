# mgmt__ovirt
ansible role for ovirt provisioning
# requirements
# variables
# dependencies
# examples
```
---

- name: ovirt login
  hosts: localhost
  gather_facts: false

  tasks:

    - name: ovirt authentication
      ansible.builtin.include_role:
        name: mgmt__ovirt
        tasks_from: authenticate.yml
      vars:
        ovirt_task: acquire_token


- name: spawn ovirt vm(s)
  hosts: "{{ nodes }}"
  gather_facts: false

  vars:
    ovirt_cluster: "{{ ovirt_cluster_name }}"
    ovirt_token: "{{ hostvars['localhost']['ovirt_auth'] }}"

  tasks:

    - name: clone virtual machine from template
      ansible.builtin.include_role:
        name: mgmt__ovirt
        tasks_from: virtual_machine.yml
      vars:
        ovirt_task: clone_template
        ovirt_template: rhel_9_demo

    - name: initialize virtual machine
      ansible.builtin.include_role:
        name: mgmt__ovirt
        tasks_from: virtual_machine.yml
      vars:
        ovirt_task: initialize_vm

    - name: "set virtual machine state to {{ ovirt_vm_state }}"
      ansible.builtin.include_role:
        name: mgmt__ovirt
        tasks_from: virtual_machine.yml
      vars:
        ovirt_task: set_vm_state
        ovirt_vm_state: running

- name: ovirt logout
  hosts: localhost
  gather_facts: false

  vars:
    ovirt_token: "{{ hostvars['localhost']['ovirt_auth'] }}"

  tasks:

    - name: ovirt authentication
      ansible.builtin.include_role:
        name: mgmt__ovirt
        tasks_from: authenticate.yml
      vars:
        ovirt_task: release_token

...
```
