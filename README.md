# mgmt__ovirt
ansible role for ovirt provisioning
# requirements
# variables
# dependencies
# examples
```
---

- name: ovirt authentication
  hosts: localhost
  gather_facts: false

  tasks:

    - name: ovirt login
      ansible.builtin.include_role:
        name: mgmt__ovirt
        tasks_from: login.yml

- name: vm provisioning
  hosts: "{{ nodes }}"
  gather_facts: false

  tasks:

    - name: "generate password(s) for {{ ansible_ssh_user }}"
      ansible.builtin.shell:
        cmd: "echo '{{ ansible_become_pass }}' | mkpasswd -s -m sha512crypt"
      register: ansible_pass
      delegate_to: localhost

    - name: provision vm
      ansible.builtin.include_role:
        name: mgmt__ovirt
        tasks_from: vm_from_template.yml
      vars:
        ovirt_auth: "{{ hostvars['localhost']['ovirt_auth'] }}"

- name: ovirt authentication
  hosts: localhost
  gather_facts: false

  tasks:

    - name: ovirt logout
      ansible.builtin.include_role:
        name: mgmt__ovirt
        tasks_from: logout.yml

...
```
