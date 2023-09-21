# Trabalho-redes 

## Projeto de Administração de Redes usando Vagrant com 3 VMs
Cujos nomes foram **server1, server2, server3**.
Foi usado para a configuração de cada uma das VMs :
- Sistema Operacional Linux Mint: **linuxmint-21.2-cinnamon-64bit.iso** ;
- Visual Studio Code (para execução dos projetos/ Vagrantfile -> a fim de automatizar a criação e configuração das VMs.): **code_1.82.0-1694039253_amd64** ;
- Virtualbox (para que as máquinas rodem de forma adequada): **Virtualbox-6.1_6.1.46-158378Ubuntujammy_amd64**.


Foi feito a utilização de 3 máquinas virtuais , cada todas com a configuração de mesmo Sistema Operacional:
**Ubuntu Server 20.04 LTS**, o usado no caso, que está disponível em: 

https://app.vagrantup.com/boxes/search?utf8=%E2%9C%93&sort=downloads&provider=&q=gusztavvargadr%2Fubuntu-server
 "gusztavvargadr/ubuntu-server"


### Máquina virtual 1 : cuja função é ser um servidor WEB (server1)
- Interface de Rede 1 (eth0): IP Privado Estático (192.168.50.10) , usando:
  
    server1.vm.network "private_network", ip: "192.168.50.10"

- Função: Servidor Web (instalar o Apache), usando:

    server1.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y apache2

- Pasta Compartilhada: `/var/www/html` na máquina host deve ser compartilhada com `/var/www/html` na VM1, usando:

     server1.vm.synced_folder "/var/www/html", "/var/www/html"
 
    
### Máquina virtual 2 : cuja função é ser Servidor de Banco de Dados (server2)
- Interface de Rede 1 (eth0): IP Privado Estático (192.168.50.11), usando:

     server2.vm.network "private_network", ip: "192.168.50.11"

- Função: Servidor de Banco de Dados (instalar o MySQL ou PostgreSQL)

    server2.vm.provision "shell", inline: <<-SHELL
      apt-get install -y mysql-server
  

### Máquina virtual 3 : Gateway - dispositivo ou software que atua como intermediário entre duas redes diferentes, permitindo a comunicação entre as 2 VMs (server3)
- Interface de Rede 1 (eth0): IP Privado Estático (192.168.50.12)

    server3.vm.network "private_network", type: "dhcp"
    server3.vm.network "public_network", type: "dhcp", bridge: "wlp2s0"
    server3.vm.hostname = "server3"


  
