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

## Check (and resize) the partition

There are a few ways to proceed. One would be to use 
the widely used ```kevincoakley.disk``` role which unfortunately doesn't work for me. 
The other way is to implement a basic resizing role, which is implemented in ```roles/vm_check_setup``` and added to
the playbook:
```
roles:
  - vm_check_setup
```  
This role, which is very similar to the aforementioned one, checks the current partition, creates a new one, 
and after it's formatted, it mounts it:
- ```name: Check device info```
- ```name: Create new partition```
- ```name: Format partition```
- ```name: Mount partition```

## Install Docker

There is a very famous and widely used role for this task, which is 
```geerlingguy.docker``` available on [GitHub](https://github.com/geerlingguy/ansible-role-docker) and that we 
add to the playbook:  
```
roles: 
  - vm_check_setup 
  - geerlingguy.docker
```  
Another option would be to create a role ourselves, one similar to 
[this one](https://www.gabrielbautista.com/post/instalando-docker-ce-desde-un-playbook-para-centos-7-y-8).  

## Docker's configuration

The first step is to secure Docker. This is easily done by means of the 
```alexinthesky.secure-docker-daemon``` role, available on [GitHub](https://github.com/alexinthesky/role-secure-docker-daemon).  
However, because we also want to make sure that Docker will start automatically, instead of adding it
directly to the playbook, we create a role which includes it. A variety
of online resources are available in this sense, for example [this tutorial](https://sleeplessbeastie.eu/2020/09/11/how-to-start-docker-service-at-system-boot/)
on sleeplessbeastie.eu or one could use the ```restart policy``` available in the [documentation](https://docs.ansible.com/ansible/latest/collections/community/docker/docker_container_module.html#parameter-restart_policy).
This new role is finally also added to the playbook:  
```
roles: 
  - vm_check_setup 
  - geerlingguy.docker
  - vm_setup_secure_docker
```  
## CI
CI is configured through Travis (as suggested) and it consists of one ```.travis.yml``` file which declares what 
language we are going to use, its version, what packages need to be installed and what script to run.  
Linting is performed with [ansible-lint](https://github.com/ansible-community/ansible-lint) which consists of one 
```.ansible-lint.yml``` file. This file declares that there are two paths which need to be excluded, and in this case they
are the paths for ```geerlingguy.docker``` and ```alexinthesky.secure-docker-daemon```.
