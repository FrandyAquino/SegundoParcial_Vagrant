Vagrant.configure("2") do |config|
  # Configuración de la máquina 1 (Servidor Apache 1)
  config.vm.define "web1" do |web1|
    web1.vm.box = "ubuntu/bionic64"
    web1.vm.network "private_network", ip: "192.168.56.10"
    web1.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    web1.vm.provision "shell", inline: <<-SHELL
      sudo apt update
      sudo apt install -y apache2
      sudo ufw allow 'Apache'
    SHELL
  end

  # Configuración de la máquina 2 (Servidor Apache 2)
  config.vm.define "web2" do |web2|
    web2.vm.box = "ubuntu/bionic64"
    web2.vm.network "private_network", ip: "192.168.56.11"
    web2.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    web2.vm.provision "shell", inline: <<-SHELL
      sudo apt update
      sudo apt install -y apache2
      sudo ufw allow 'Apache'
    SHELL
  end

  # Configuración de la máquina 3 (Servidor Nginx)
  config.vm.define "loadbalancer" do |lb|
    lb.vm.box = "ubuntu/bionic64"
    lb.vm.network "private_network", ip: "192.168.56.12"
    lb.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    lb.vm.provision "shell", inline: <<-SHELL
      sudo apt update
      sudo apt install -y nginx
      sudo ufw allow 'Nginx Full'
      echo "upstream web_backend {
        server 192.168.56.10;
        server 192.168.56.11;
      }
      server {
        listen 80;
        location / {
          proxy_pass http://web_backend;
        }
      }" | sudo tee /etc/nginx/sites-available/loadbalancer
      sudo ln -s /etc/nginx/sites-available/loadbalancer /etc/nginx/sites-enabled/
      sudo nginx -t && sudo systemctl restart nginx
    SHELL
  end
end
