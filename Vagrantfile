# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# Vagrant box with docker-ce
#
# https://docs.docker.com/engine/installation/linux/docker-ce/debian/#install-using-the-repository
#

DOCKER_VERSION = "18.03.1~ce-0~debian"

$script = <<SCRIPT

export DEBIAN_FRONTEND=noninteractive

apt update && apt install -y --no-install-recommends \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg2 \
    software-properties-common \
    vim \
    ansible

echo "Installing docker via apt repo..."
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | sudo apt-key add -
apt-key fingerprint 0EBFCD88

add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
    $(lsb_release -cs) \
    stable"

apt update && apt install -y --no-install-recommends \
    docker-ce=#{DOCKER_VERSION}

echo "Adding vagrant user to docker and adm groups..."
groupadd docker &> /dev/null
usermod -aG docker vagrant
usermod -aG adm vagrant

echo 'Welcome to Expert DevOps Website' > /var/www/html/index.html
echo "Writing docker aliases..."
cat > /etc/profile.d/00-aliases.sh <<EOF
alias d="docker"
EOF
echo "Enojy! :)"
SCRIPT

def get_ipaddr(hostname, default)
  return Socket::getaddrinfo(hostname, nil)[0][3]
  rescue SocketError
    return default
end

Vagrant.configure("2") do |config|
    config.vm.box = "debian/buster64"
    config.vm.hostname = "buster-docker"

    private_network_ip = get_ipaddr(config.vm.hostname, "192.168.33.100")
    config.vm.network "private_network", ip: private_network_ip

    # install required software
    config.vm.provision "shell", inline: $script

    # Export 80 HTTP port
    config.vm.network :forwarded_port, host: 8080, guest: 8080

    # Build docker image
    config.vm.provision :docker do |docker|
      docker.build_image '/vagrant/.docker/', args: '-t web'
      docker.run 'web', args: '-it -p 8080:80 -v /var/www/html:/usr/share/nginx/html'
    end


    config.vm.post_up_message = \
      "The private network IP address is: #{private_network_ip}\n\n" \
      "To customize, set the host called '#{config.vm.hostname}'\n" \
      "to the desired IP address in your /etc/hosts and run \n" \
      "'vagrant reload'!\n"
end
