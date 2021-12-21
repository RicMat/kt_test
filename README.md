# kt_test

## Initial setup

Following the [tutorial](https://www.hamvocke.com/blog/local-ansible-testing/) by Ham Vocke:
- Install Vagrant
  - ```brew install vagrant```
- Install Ansible
  - ```brew install ansible```
- Create two VMs with at least 40GB of storage and centOs as OS
  - Inside Vagrant file
    - ```config.disksize.size = '50GB'```
    - ```d2.vm.box = "centos/8"```
- Define a playbook and an inventory
  - ```ansible.inventory_path = "hosts"```
  - ```ansible.playbook = "playbook.yml"```



