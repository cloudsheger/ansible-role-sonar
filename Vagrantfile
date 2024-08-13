require 'yaml'
settings = YAML.load_file(File.join(File.dirname(__FILE__), 'settings.yaml'))

Vagrant.configure("2") do |config|
  config.vm.box = settings['box_name']
  config.vm.box_check_update = true
  # config.ssh.private_key_path = "/Users/apple/.ssh/id_rsa"

  settings['vm'].each do |vm_config|
    config.vm.define vm_config['name'] do |vm|
      vm.vm.hostname = vm_config['name']
      vm.vm.network "private_network", ip: vm_config['ip']
      vm.vm.synced_folder ".", "/vagrant"

      vm.vm.provider "virtualbox" do |vb|
        vb.memory = vm_config['memory']
        vb.cpus = vm_config['cpus']
        vb.gui = false
      end

      vm.vm.provision "shell", inline: <<-SHELL
        # Update package list and upgrade all packages
        apt-get update
        apt-get -y upgrade
    
        # Install necessary packages
        apt-get -y install python3 python3-pip podman gcc libssl-dev libffi-dev python3-dev python3-cryptography ufw

        # Install Java (OpenJDK 11 as an example)
        apt-get -y install openjdk-11-jdk
        
        # Install Maven
        apt-get -y install maven
    
        # Install Ansible
        pip3 install ansible-core
    
        # Configure UFW firewall
        ufw allow 22/tcp
        ufw allow 9092/tcp
        ufw allow 9000/tcp
        ufw allow 8080/tcp
    
        # Enable and start UFW
        ufw --force enable
        #pip3 install --user ansible-core
        
        echo "192.168.201.13 sonar" >> /etc/hosts
        
      SHELL
    end
  end
end