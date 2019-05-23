# See: https://manski.net/2016/09/vagrant-multi-machine-tutorial/
# for information about machine names on private network
Vagrant.configure("2") do |config|
  config.vm.box = "envimation/ubuntu-xenial"

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update -y
    sudo apt-get install iputils-ping -y
    sudo apt-get install python3 --yes
  SHELL

  config.vm.define "host1" do |host1|
    host1.vm.box = "envimation/ubuntu-xenial"
    host1.vm.hostname = "host1"
    host1.vm.network "private_network", ip: "192.168.33.11"

    # We want to access wildfly from the host using port 8080
    host1.vm.network "forwarded_port", guest: 8080, host: 8080
  end

  config.vm.define "host2" do |host2|
    host2.vm.box = "envimation/ubuntu-xenial"
    host2.vm.hostname = "host2"
    host2.vm.network "private_network", ip: "192.168.33.12"
  end

  config.vm.define "ansible" do |ansible|
    ansible.vm.box = "envimation/ubuntu-xenial"
    ansible.vm.hostname = "ansible"
    ansible.vm.network "private_network", ip: "192.168.33.10"

    ansible.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=755,fmode=600"]

    ansible.vm.provider "virtualbox" do |v|
      # We need 2048Mb so that Wildfly and Errai can execute
      v.memory = 2048
    end

    ansible.vm.provision "shell", inline: <<-SHELL
      sudo apt-get install -y --no-install-recommends apt-utils
      sudo apt-get install software-properties-common --yes
      sudo apt-add-repository --yes --u ppa:ansible/ansible
      sudo apt-get install ansible --yes
    SHELL
  end
end
