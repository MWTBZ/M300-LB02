Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"

  # Creation of Port Forwards
  config.vm.network "forwarded_port", guest:80, host:80, auto_correct: true
  config.vm.network "forwarded_port", guest:8080, host:8080, auto_correct: true
  config.vm.network "forwarded_port", guest:8081, host:8081, auto_correct: true
  config.vm.network "forwarded_port", guest:8082, host:8082, auto_correct: true
  config.vm.network "forwarded_port", guest:3306, host:3306, auto_correct: true  
  config.vm.network "forwarded_port", guest:139, host:139, auto_correct: true 
  config.vm.network "forwarded_port", guest:445, host:445, auto_correct: true 
  config.vm.network "forwarded_port", guest:137, host:137, auto_correct: true
  config.vm.network "forwarded_port", guest:138, host:138, auto_correct: true
    
  # Definition of Ubuntu
  config.vm.hostname = "docker"
  config.vm.network "private_network", ip:"192.168.0.100"  # 10.71.13.20

  # Definition of VM
  config.vm.provider "virtualbox" do |vb|
     vb.memory = "2048"
     vb.name = "Docker-LB02"
  end

  # Docker Provisioner
  config.vm.provision "docker" do |d|
    # Image for Apache
   d.pull_images "ubuntu:latest"
    # Image for Samba
   d.pull_images "alpine:latest"
  end

  # Install Docker-Composes
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get install -y docker-compose
  SHELL
end
