Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
  end

  config.vm.provision "shell" do |s|
    ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
    s.inline = <<-SHELL
      echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
    SHELL
  end

  config.vm.define "node1" do |master|
    config.vm.box = "ubuntu/bionic64"
    master.vm.hostname = "node1.vagrant.host"
    master.vm.network :private_network, ip: "192.168.56.21"
  end

  config.vm.define "node2" do |minion|
    config.vm.box = "almalinux/8"
    minion.vm.hostname = "node2.vagrant.host"
    minion.vm.network :private_network, ip: "192.168.56.22"
    minion.vm.provision :shell, inline: 'grep -q salt /etc/hosts || (echo "192.168.33.21 salt" >> /etc/hosts)'
  end
end
