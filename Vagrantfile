require 'yaml'

# reference: https://www.debugcn.com/en/article/57056373.html
CONFIG = YAML.load_file(File.join(File.dirname(__FILE__), 'config.yml'))

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false

  # controlplanes
  CONFIG['controlplanes'].each do |controlplane|
    config.vm.define controlplane['name'] do |cfg|
      cfg.vm.box = controlplane['box']
      cfg.vm.network "public_network", ip: controlplane['ip']
      cfg.vm.hostname = controlplane['hostname']
      
      cfg.vm.provider "virtualbox" do |v|
        v.memory = controlplane['memory']
        v.cpus = controlplane['cpu']
        v.name = controlplane['name']
      end
      cfg.vm.provision "shell", inline: <<-SCRIPT
        sed -i -e 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
        systemctl restart sshd
      SCRIPT

      # chrony configuration
      cfg.vm.provision "file", source: "chrony.conf", destination: "/tmp/chrony.conf"
      cfg.vm.provision  "shell", inline: <<-SCRIPT
        apt-get update
        apt-get install chrony -y
        cp /tmp/chrony.conf /etc/chrony.conf
        timedatectl set-timezone Asia/Seoul
        systemctl enable chrony
        systemctl restart chrony
        timedatectl set-ntp true
      SCRIPT
    end
  end

  # worker nodes
  CONFIG['workers'].each do |worker|
    config.vm.define worker['name'] do |cfg|
      cfg.vm.box = worker['box']
      cfg.vm.network "public_network", ip: worker['ip']
      cfg.vm.hostname = worker['hostname']
      
      cfg.vm.provider "virtualbox" do |v|
        v.memory = worker['memory']
        v.cpus = worker['cpu']
        v.name = worker['name']
      end
      cfg.vm.provision "shell", inline: <<-SCRIPT
        sed -i -e 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
        systemctl restart sshd
      SCRIPT

      # chrony configuration
      cfg.vm.provision "file", source: "chrony.conf", destination: "/tmp/chrony.conf"
      cfg.vm.provision  "shell", inline: <<-SCRIPT
        apt-get update
        apt-get install chrony -y
        cp /tmp/chrony.conf /etc/chrony.conf
        timedatectl set-timezone Asia/Seoul
        systemctl enable chrony
        systemctl restart chrony
        timedatectl set-ntp true
      SCRIPT
    end
  end

  # bootstrap-server
  CONFIG['bootstrap'].each do |bootstrap|
    config.vm.define bootstrap['name'] do |cfg|
      cfg.vm.box = bootstrap['box']
      cfg.vm.hostname = bootstrap['hostname']
      cfg.vm.network "public_network", ip: bootstrap['ip']
  
      cfg.vm.provider "virtualbox" do |v|
        v.memory = bootstrap['memory']
        v.cpus = bootstrap['cpu']
        v.name = bootstrap['name']
      end
      
      cfg.vm.provision  "shell", inline: <<-SCRIPT
        sed -i -e 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
        systemctl restart sshd
      SCRIPT

      # chrony configuration
      cfg.vm.provision "file", source: "chrony.conf", destination: "/tmp/chrony.conf"
      cfg.vm.provision  "shell", inline: <<-SCRIPT
        apt-get update
        apt-get install chrony -y
        cp /tmp/chrony.conf /etc/chrony.conf
        timedatectl set-timezone Asia/Seoul
        systemctl enable chrony
        systemctl restart chrony
        timedatectl set-ntp true
      SCRIPT
    end
  end
end