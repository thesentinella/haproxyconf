Vagrant.configure("2") do |config|

    # Define host entries for all VMs
    hosts_entries = <<-HOSTS
  192.168.56.10 lb01
  192.168.56.11 lb02
  192.168.56.12 web01
  192.168.56.13 web02
  HOSTS
  
    # Load Balancer 1
    config.vm.define "lb01" do |lb01|
      lb01.vm.box = "bento/ubuntu-24.04"
      lb01.vm.hostname = "lb01"
      lb01.vm.network "private_network", ip: "192.168.56.10"
      lb01.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 1
      end
      lb01.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y haproxy
        sed -i 's/ENABLED=0/ENABLED=1/' /etc/default/haproxy
        echo "#{hosts_entries}" >> /etc/hosts
        # HAProxy configuration
        cat <<EOL > /etc/haproxy/haproxy.cfg
  global
      log /dev/log local0
      log /dev/log local1 notice
      chroot /var/lib/haproxy
      stats socket /run/haproxy/admin.sock mode 660 level admin
      stats timeout 30s
      user haproxy
      group haproxy
      daemon
  
  defaults
      log     global
      option  httplog
      option  dontlognull
      timeout connect 5000ms
      timeout client  50000ms
      timeout server  50000ms
  
  frontend http_front
      bind *:80
      default_backend web_servers
  
  backend web_servers
      balance roundrobin
      server web01 192.168.56.12:80 check
      server web02 192.168.56.13:80 check
  EOL
  
        systemctl restart haproxy
      SHELL
    end
  
    # Load Balancer 2
    config.vm.define "lb02" do |lb02|
      lb02.vm.box = "bento/ubuntu-24.04"
      lb02.vm.hostname = "lb02"
      lb02.vm.network "private_network", ip: "192.168.56.11"
      lb02.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 1
      end
      lb02.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y haproxy
        sed -i 's/ENABLED=0/ENABLED=1/' /etc/default/haproxy
        echo "#{hosts_entries}" >> /etc/hosts
        # HAProxy configuration
        cat <<EOL > /etc/haproxy/haproxy.cfg
  global
      log /dev/log local0
      log /dev/log local1 notice
      chroot /var/lib/haproxy
      stats socket /run/haproxy/admin.sock mode 660 level admin
      stats timeout 30s
      user haproxy
      group haproxy
      daemon
  
  defaults
      log     global
      option  httplog
      option  dontlognull
      timeout connect 5000ms
      timeout client  50000ms
      timeout server  50000ms
  
  frontend http_front
      bind *:80
      default_backend web_servers
  
  backend web_servers
      balance roundrobin
      server web01 192.168.56.12:80 check
      server web02 192.168.56.13:80 check
  EOL
  
        systemctl restart haproxy
      SHELL
    end
  
    # Web Server 1
    config.vm.define "web01" do |web01|
      web01.vm.box = "bento/ubuntu-24.04"
      web01.vm.hostname = "web01"
      web01.vm.network "private_network", ip: "192.168.56.12"
      web01.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 1
      end
      web01.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y nginx
        systemctl enable nginx
        systemctl restart nginx
  
        echo "<h3>1</h3>" >> /var/www/html/index.nginx-debian.html
        echo "#{hosts_entries}" >> /etc/hosts
      SHELL
    end
  
    # Web Server 2
    config.vm.define "web02" do |web02|
      web02.vm.box = "bento/ubuntu-24.04"
      web02.vm.hostname = "web02"
      web02.vm.network "private_network", ip: "192.168.56.13"
      web02.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 1
      end
      web02.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y nginx
        systemctl enable nginx
        systemctl restart nginx
  
        echo "<h3>1</h3>" >> /var/www/html/index.nginx-debian.html
        echo "#{hosts_entries}" >> /etc/hosts
      SHELL
    end
  
  end
  