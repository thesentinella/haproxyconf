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
    mode    http
    option  httplog
    option  dontlognull
    retries 3
    timeout http-request 10s
    timeout queue        1m
    timeout connect      10s
    timeout client       1m
    timeout server       1m
    timeout http-keep-alive 10s
    timeout check        10s
    maxconn 3000

frontend http_front
    bind *:80
      # Define ACLs based on Host header
    acl host_app1 hdr(host) -i app1.example.com
    acl host_app2 hdr(host) -i app2.example.com

    # Use backends based on the ACLs
    use_backend app1_backend if host_app1
    use_backend app2_backend if host_app2

backend app1_backend
    balance roundrobin
    server web01 192.168.56.12:80 check
    server web02 192.168.56.13:80 check

backend app2_backend
    balance roundrobin
    server web01 192.168.56.12:8080 check
    server web02 192.168.56.13:8080 check
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
    mode    http
    option  httplog
    option  dontlognull
    retries 3
    timeout http-request 10s
    timeout queue        1m
    timeout connect      10s
    timeout client       1m
    timeout server       1m
    timeout http-keep-alive 10s
    timeout check        10s
    maxconn 3000

frontend http_front
    bind *:80
      # Define ACLs based on Host header
    acl host_app1 hdr(host) -i app1.example.com
    acl host_app2 hdr(host) -i app2.example.com

    # Use backends based on the ACLs
    use_backend app1_backend if host_app1
    use_backend app2_backend if host_app2

backend app1_backend
    balance roundrobin
    server web01 192.168.56.12:80 check
    server web02 192.168.56.13:80 check

backend app2_backend
    balance roundrobin
    server web01 192.168.56.12:8080 check
    server web02 192.168.56.13:8080 check
EOL
  
        systemctl restart haproxy
      SHELL
    end

    # Load Balancer 3
    config.vm.define "lb03" do |lb02|
      lb02.vm.box = "bento/ubuntu-24.04"
      lb02.vm.hostname = "lb03"
      lb02.vm.network "private_network", ip: "192.168.56.14"
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
    mode    http
    option  httplog
    option  dontlognull
    retries 3
    timeout http-request 10s
    timeout queue        1m
    timeout connect      10s
    timeout client       1m
    timeout server       1m
    timeout http-keep-alive 10s
    timeout check        10s
    maxconn 3000
frontend http_front
    bind *:80
      # Define ACLs based on Host header
    acl host_app1 hdr(host) -i app1.example.com
    acl host_app2 hdr(host) -i app2.example.com

    # Use backends based on the ACLs
    use_backend app1_backend if host_app1
    use_backend app2_backend if host_app2

backend app1_backend
    balance roundrobin
    server web01 192.168.56.12:80 check
    server web02 192.168.56.13:80 check

backend app2_backend
    balance roundrobin
    server web01 192.168.56.12:8080 check
    server web02 192.168.56.13:8080 check
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
        # Create a new configuration for port 8080
    cat <<EOL > /etc/nginx/sites-available/8080
    server {
        listen 8080;
        root /var/www/8080;
        index index.html;
    }
EOL

    # Enable the new site
    mkdir -p /var/www/8080
    echo "<h3>Page for Port 8080</h3>" > /var/www/8080/index.html
    ln -s /etc/nginx/sites-available/8080 /etc/nginx/sites-enabled/8080

    echo "#{hosts_entries}" >> /etc/hosts
    systemctl restart nginx
      
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

      echo "<h3>2</h3>" >> /var/www/html/index.nginx-debian.html
        # Create a new configuration for port 8080
    cat <<EOL > /etc/nginx/sites-available/8080
    server {
        listen 8080;
        root /var/www/8080;
        index index.html;
    }
EOL

    # Enable the new site
    mkdir -p /var/www/8080
    echo "<h3>Page for Port 8080</h3>" > /var/www/8080/index.html
    ln -s /etc/nginx/sites-available/8080 /etc/nginx/sites-enabled/8080

    echo "#{hosts_entries}" >> /etc/hosts
    systemctl restart nginx
      
    SHELL
  end
  
  
  # Web Server 3
  config.vm.define "web03" do |web03|
    web03.vm.box = "bento/ubuntu-24.04"
    web03.vm.hostname = "web03"
    web03.vm.network "private_network", ip: "192.168.56.20"
    web03.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
    web03.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y nginx
      systemctl enable nginx
      systemctl restart nginx

      echo "<h3>3</h3>" >> /var/www/html/index.nginx-debian.html
        # Create a new configuration for port 8080
    cat <<EOL > /etc/nginx/sites-available/8080
    server {
        listen 8080;
        root /var/www/8080;
        index index.html;
    }
EOL

    # Enable the new site
    mkdir -p /var/www/8080
    echo "<h3>Page for Port 8080</h3>" > /var/www/8080/index.html
    ln -s /etc/nginx/sites-available/8080 /etc/nginx/sites-enabled/8080

    echo "#{hosts_entries}" >> /etc/hosts
    systemctl restart nginx
      
    SHELL
  end
    
  end
    