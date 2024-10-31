# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

Vagrant.configure("2") do |config|

  config.vm.provision "file", source: "files/vagrant_test.pub", destination: "/home/vagrant/.ssh/"

  config.vm.define "controlnode" do |controlnode|
    controlnode.vm.box = "ubuntu/focal64"
    controlnode.vm.hostname = "controlnode"
    controlnode.vm.network "private_network", ip: "192.168.9.6"
    controlnode.vm.synced_folder "./sync","/home/vagrant/sync"
    controlnode.vm.provision "file", source: "files/vagrant_test", destination: "/home/vagrant/.ssh/"
    controlnode.vm.provision "shell", inline: <<-SHELL
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
      service ssh restart
      sudo add-apt-repository ppa:ansible/ansible
      sudo apt update -y && sudo apt -y install sshpass ansible 
      sudo -H -u vagrant bash -c 'ansible-galaxy role install geerlingguy.postgresql'
      sudo -H -u vagrant bash -c 'ansible-galaxy role install geerlingguy.docker'
      sudo -H -u vagrant bash -c 'ansible-galaxy collection install community.postgresql'
      sudo -H -u vagrant bash -c 'ansible-galaxy collection install community.docker'
      chmod 600 /home/vagrant/.ssh/vagrant_test
      chmod 644 /home/vagrant/.ssh/vagrant_test.pub
    SHELL
  end

  config.vm.define "server" do |server|
     server.vm.box = "ubuntu/focal64"
     server.vm.hostname = "server"
     server.vm.network "private_network", ip: "192.168.9.7"
     server.vm.provision "shell", inline: <<-SHELL
       sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
       chmod 644 /home/vagrant/.ssh/vagrant_test.pub
       cat /home/vagrant/.ssh/vagrant_test.pub >> /home/vagrant/.ssh/authorized_keys
       service ssh restart
     SHELL
    end
end

