Vagrant.configure("2") do |config|
  config.vm.box = "gusztavvargadr/ubuntu-server"

  #Baixando as versoes de UBUNTU SERVER  como foi pedido no exercicio para todas as VMs

  #Agora iniciemos a criacao e configuracao da VM1 , cujo nome e server1

  config.vm.define "server1" do |server1|
    server1.vm.network "private_network", ip: "192.168.50.10"
    server1.vm.synced_folder "/var/www/html", "/var/www/html"
    server1.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2 

    SHELL

  end
  

  config.vm.define "server2" do |server2|
    server2.vm.network "private_network", ip: "192.168.50.11"
    server2.vm.provision "shell", inline: <<-SHELL
    apt-get install -y mysql-server

    SHELL

  end

  config.vm.define "server3" do |server3|
    server3.vm.network "private_network", type: "dhcp"
    server3.vm.network "public_network", type: "dhcp", bridge: "wlp2s0"
    server3.vm.hostname = "server3"

    server3.vm.provision "shell", inline: <<-SHELL
    export DEBIAN_FRONTEND=noninteractive
    sudo ip addr add 192.168.56.12/24
    apt-get install -y 
    
