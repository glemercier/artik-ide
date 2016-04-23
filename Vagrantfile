Vagrant.configure(2) do |config|
  config.vm.box = "centos71-docker-java-v1.0"
  config.vm.box_url = "https://install.codenvycorp.com/centos71-docker-java-v1.0.box"
  config.vm.box_download_insecure = true
  config.ssh.insert_key = false
  config.vm.network :private_network, ip: "192.168.28.28"
  config.vm.define "artik" do |artik|
  end
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.name = "artik-ide-vm"
    vb.customize ["modifyvm", :id, "--usb", "on"]
    vb.customize ["modifyvm", :id, "--usbehci", "on"]
    vb.customize ["usbfilter", "add", "0", 
                  "--target", :id, 
                  "--name", "Artik"]
  end

  config.vm.provision "shell", inline: <<-SHELL
    usermod -aG docker vagrant &>/dev/null
    echo "."
    echo "."
    echo "ARTIK IDE: DOWNLOADING ARTIK IDE"
    echo "."
    echo "."
    curl -O "https://install.codenvycorp.com/artik/samsung-artik-ide-nightly.tar.gz"
    tar xvfz samsung-artik-ide-nightly.tar.gz &>/dev/null
    sudo chown -R vagrant:vagrant * &>/dev/null
    export JAVA_HOME=/usr &>/dev/null

    echo "."
    echo "."
    echo "ARTIK IDE: DOWNLOADING ARTIK RUNTIME IMAGE"
    echo "."
    echo "."
    docker pull codenvy/artik

    echo "."
    echo "."
    echo "ARTIK IDE: PREPPING SERVER"
    echo "."
    echo "."

    echo vagrant | sudo -S -E -u vagrant /home/vagrant/eclipse-che-*/bin/che.sh --remote:192.168.28.28 --skip:client -g start

  SHELL

  config.vm.provision "shell", run: "always", inline: <<-SHELL
    echo "."
    echo "."
    echo "ARTIK IDE: SERVER BOOTING ~10s"
    echo "AVAILABLE: http://192.168.28.28:8080"
    echo "."
    echo "."

  SHELL
end
