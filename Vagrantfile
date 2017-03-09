
Vagrant.configure("2") do |config|

   config.ssh.insert_key = false
 
  config.vm.define "dn2" do |dn2|
    #dn2.vm.box = "opscode-ubuntu-14.04"
    dn2.vm.box = "bento/ubuntu-16.04"
    dn2.vm.hostname = 'dn2' 
    #dn2.vm.box_url = 'https://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-14.04_chef-provisionerless.box'

    dn2.vm.network :private_network, ip: "192.168.56.103"
    dn2.vm.network :forwarded_port, guest: 22, host: 10222, id: "ssh"

#    dn2.vm.provision "shell", inline: "mkdir -p /home/vagrant/.karamel/install && chown -R vagrant /home/vagrant/.karamel"
#    dn2.vm.provision "file", source: "downloads/chefdk_0.16.28-1_amd64.deb", destination: "/home/vagrant/.ssh/install/chefdk_0.16.28-1_amd64.deb"
#    dn2.vm.provision "file", source: "downloads/karamel-0.3.tgz", destination: "/home/vagrant/karamel-0.3.tgz"
    
    dn2.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 4048]
      v.customize ["modifyvm", :id, "--name", "dn2"]
    end

    dn2.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = "cookbooks"
      chef.json = {
          "karamel" => {
	    "default" =>      { 
              "private_ips" => ["192.168.56.101","192.168.56.102","192.168.56.103"]
	    },
          },
        }
      chef.add_recipe "karamel::install"
    end
  end

  config.vm.define "dn1" do |dn1|
    dn1.vm.box = "bento/ubuntu-16.04"
    dn1.vm.hostname = 'dn1'
    dn1.vm.network :private_network, ip: "192.168.56.102"
    dn1.vm.network :forwarded_port, guest: 22, host: 10322, id: "ssh"

#    dn1.vm.provision "shell", inline: "mkdir -p /home/vagrant/.karamel/install && chown -R vagrant /home/vagrant/.karamel"
#    dn1.vm.provision "file", source: "downloads/chefdk_0.16.28-1_amd64.deb", destination: "/home/vagrant/.ssh/install/chefdk_0.16.28-1_amd64.deb"
#    dn1.vm.provision "file", source: "downloads/karamel-0.3.tgz", destination: "/home/vagrant/karamel-0.3.tgz"
    
    dn1.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 4048]
      v.customize ["modifyvm", :id, "--name", "dn1"]
    end

    dn1.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = "cookbooks"
      chef.json = {
          "karamel" => {
	    "default" =>      { 
              "private_ips" => ["192.168.56.101","192.168.56.102","192.168.56.103"]
	    },
          },
        }
      chef.add_recipe "karamel::install"
    end
  end


  config.vm.define "dn0", primary: true do |dn0|
    dn0.vm.box = "bento/ubuntu-16.04"
    dn0.vm.hostname = 'dn0'

    dn0.vm.network :private_network, ip: "192.168.56.101"
    dn0.vm.network :forwarded_port, guest: 22, host: 10122, id: "ssh"
    dn0.vm.network(:forwarded_port, {:guest=>9090, :host=>9090})     
    dn0.vm.network(:forwarded_port, {:guest=>8080, :host=>8080})     


    dn0.vm.provision "file", source: "cluster.yml", destination: "cluster.yml"
    dn0.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "~/.ssh/id_rsa"    
    dn0.vm.provision "shell", inline: "cp /home/vagrant/.ssh/authorized_keys /home/vagrant/.ssh/id_rsa.pub && sudo chown vagrant:vagrant /home/vagrant/.ssh/id_rsa.pub"
#    dn0.vm.provision "shell", inline: "mkdir -p /home/vagrant/.karamel/install && chown -R vagrant /home/vagrant/.karamel"
#    dn0.vm.provision "file", source: "downloads/chefdk_0.16.28-1_amd64.deb", destination: "/home/vagrant/.ssh/install/chefdk_0.16.28-1_amd64.deb"
#    dn0.vm.provision "file", source: "downloads/karamel-0.3.tgz", destination: "/home/vagrant/karamel-0.3.tgz"
    
    dn0.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 16048]
      v.customize ["modifyvm", :id, "--name", "dn0"]      
    end

    dn0.vm.provision :chef_solo do |chef|
        chef.cookbooks_path = "cookbooks"
        chef.json = {
          "karamel" => {
	    "default" =>      { 
              "private_ips" => ["192.168.56.101","192.168.56.102","192.168.56.103"]
	    },
          },
        }
        chef.add_recipe "karamel::install"
        chef.add_recipe "karamel::default"     
        chef.add_recipe "karamel::run"     
      end
    
  end

end
