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

- Redirecionamento de porta: A configuração "forwarded_port" em uma máquina virtual (VM) geralmente é usada para redirecionar portas de rede de um sistema hospedeiro (host) para um sistema convidado (guest) em uma máquina virtual:

       server1.vm.network "forwarded_port", guest: 80, host: 8000

 
- Interface de Rede 1 (eth0): IP Privado Estático (192.168.56.10) , usando:
  
      server1.vm.network "private_network", ip: "192.168.56.10"

- Função: Servidor Web (instalar o Apache), usando:

        server1.vm.provision "shell", inline: <<-SHELL
         apt-get update
         apt-get install -y apache2
        SHELL

    
- Pasta Compartilhada: `/var/www/html` na máquina host deve ser compartilhada com `/var/www/html` na VM1, usando:

       server1.vm.synced_folder "/var/www/html", "/var/www/html"

    
### Máquina virtual 2 : cuja função é ser Servidor de Banco de Dados (server2)


- Redirecionamento de porta: A configuração "forwarded_port" em uma máquina virtual (VM) geralmente é usada para redirecionar portas de rede de um sistema hospedeiro (host) para um sistema convidado (guest) em uma máquina virtual:

       server2.vm.network "forwarded_port", guest: 80, host: 8888

  
- Interface de Rede 1 (eth0): IP Privado Estático (192.168.56.11), usando:

       server2.vm.network "private_network", ip: "192.168.56.11"

- Função: Servidor de Banco de Dados (instalar o MySQL ou PostgreSQL)

  
       server2.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y mysql-server
       SHELL
  

### Máquina virtual 3 : Gateway - dispositivo ou software que atua como intermediário entre duas redes diferentes, permitindo a comunicação entre as 2 VMs (server3)

- Interface de Rede 1 (eth0): IP Privado Estático (192.168.56.12), usando:

      server3.vm.network "private_network", ip: "192.168.56.12"

  
- Configurando uma interface de rede pública usando DHCP e especifica a interface de rede do host como "wlp2s0". Essa configuração permite que a máquina virtual obtenha um endereço IP da rede externa.

       server3.vm.network "public_network", type: "dhcp", bridge: "wlp2s0"

-  Definindo o nome de host da máquina virtual como "server3", usando:

        server3.vm.hostname = "server3"

- Executando comandos shell dentro da máquina virtual durante o provisionamento. Os comandos dentro do bloco configuram algumas configurações de rede e exportam variáveis de ambiente.

Este código é usado para configurar uma máquina virtual chamada "server3" com interfaces de rede privada e pública, bem como realizar algumas configurações de rede adicionais dentro da máquina virtual. Certifique-se de que a configuração do seu ambiente Vagrant seja compatível com essas configurações.

       server3.vm.provision "shell", inline: <<-SHELL
         export DEBIAN_FRONTEND=noninteractive
         sudo ip link set eth0 up
         echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
        SHELL

- No final de cada código, utilizamos "end" para fechar cada comando "do" que é aberto no começo dos algoritmos.

      

       

  
