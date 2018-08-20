# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<-SCRIPT
echo I am provisioning...
date > /etc/vagrant_provisioned_at
# start here 
sudo yum -y install https://yum.puppetlabs.com/puppet5/puppet5-release-el-7.noarch.rpm
sudo yum -y install http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum -y install https://yum.theforeman.org/releases/1.18/el7/x86_64/foreman-release.rpm
sudo yum -y install foreman-installer
sudo foreman-installer | tee outputfile.txt
sudo systemctl restart httpd
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
    config.vm.define "foreman" do |foreman|
    config.vm.provider "virtualbox" do |v|
      v.memory = 4096
      v.cpus = 4
    end
      foreman.vm.hostname = "foreman.orplab.xt"
      foreman.vm.network :private_network, :auto_network => true
      foreman.vm.provision :hostsupdate, run: 'always' do |foreman|
        foreman.hostname = "foreman.orplab.xt"
        foreman.manage_guest = true
        foreman.manage_host = true
        foreman.aliases = [
          "foreman"
         ]
      end
      foreman.vm.provision "shell", inline: $script
    end

  (1..3).each do |i|
    config.vm.define "node-#{i}" do |node|
    node.vm.network :private_network, :auto_network => true
      node.vm.provision :hostsupdate, run: 'always' do |node|
        node.hostname = "node-#{i}.orplab.xt"
        node.manage_guest = true
        node.manage_host = true
        node.aliases = [
          "node-#{i}"
         ]
      end
    end
  end
end
