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

## Check the partition's size

There are a few ways to proceed. One would be to use 
the widely used ```kevincoakley.disk``` role which unfortunately doesn't work for me. 
The other way is to implement a basic resizing role, which is implemented in ```roles/vm_check_setup```.  
This role, which is very similar to the aforementioned one, checks the current partition, creates a new one, 
and after it's formatted, it mounts it:
- ```name: Check device info```
- ```name: Create new partition```
- ```name: Format partition```
- ```name: Mount partition```


