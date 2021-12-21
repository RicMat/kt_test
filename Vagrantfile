Vagrant.configure(2) do |config|
  # define the size for the partition to be 50Gb which is over the necessary 40Gb
  config.disksize.size = '50GB'

  config.vm.define "vm1" do |d1|
    d1.vm.box = "centos/8"
    d1.vm.hostname = "vm1"
    d1.vm.network "private_network", ip: "192.168.56.10"
  end

  config.vm.define "vm2" do |d2|
    d2.vm.box = "centos/8"
    d2.vm.hostname = "vm2"
    d2.vm.network "private_network", ip: "192.168.56.20"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.inventory_path = "hosts"
    ansible.playbook = "playbook.yml"
  end

end